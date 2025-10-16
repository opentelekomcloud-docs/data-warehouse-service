:original_name: dws_04_0103.html

.. _dws_04_0103:

GaussDB(DWS) Connection Management Specifications
=================================================

.. _en-us_topic_0000002100586254__en-us_topic_0000002134166293_section1066113812212:

Rule 1.1: Configuring Load Balancing for GaussDB(DWS) Clusters
--------------------------------------------------------------

.. note::

   **Impact of rule violation:**

   -  Load imbalance causes performance problems and even service interruption.
   -  When a CN is faulty, services cannot be automatically recovered or the recovery may take a long time.

   **Solution:**

   -  Configure ELB load balancing and connect the application to the load balancing IP address.

.. _en-us_topic_0000002100586254__en-us_topic_0000002134166293_section13247184232218:

Rule 1.2: Ending the Database Connection After Necessary Operations (Except in Connection Pool Scenarios)
---------------------------------------------------------------------------------------------------------

.. note::

   **Impact of rule violation:**

   -  The number of idle connections exceeds the maximum limit, causing connection creation failure.
   -  Resource overload occurs because there are too many idle connections.

   **Solution:**

   -  After the connection between the application and the database is established and used, manually end the connection.
   -  Set the **session_timeout** parameter on the service side to set the idle timeout duration. The connection will be automatically ended when the idle timeout duration expires.

.. _en-us_topic_0000002100586254__en-us_topic_0000002134166293_section1568113152617:

Rule 1.3: Ensuring a Started Transaction Is Committed or Rolled Back
--------------------------------------------------------------------

.. note::

   **Impact of rule violation:**

   -  If a transaction remains uncommitted for an extended period, it blocks operations such as **ALTER**, thereby affecting all services.
   -  The number of idle connections exceeds the maximum limit, causing connection creation failure.

   **Solution:**

   -  **autocommit** is enabled by default, so there is no need to manually commit any transaction unless you modify the default setting.
   -  If a transaction is explicitly started, it must be explicitly ended (either by committing or rolling back) once the relevant operations are finished.

.. _en-us_topic_0000002100586254__en-us_topic_0000002134166293_section201871334162616:

Rule 1.4: Ensuring the Idle Timeout Duration Is Shorter Than SESSION_TIMEOUT Value When Connection Pool Is Used for Applications
--------------------------------------------------------------------------------------------------------------------------------

.. note::

   **Impact of rule violation:**

   -  The idle timeout mechanism on the service side clears connections in the connection pool, which negatively impacts connection reuse.

   **Solution:**

   -  To ensure everything works correctly, make sure the idle timeout duration of the connection pool is shorter than the **SESSION_TIMEOUT** value in GaussDB(DWS). It is advised to adjust the idle timeout duration rather than modifying the **SESSION_TIMEOUT** value.

.. _en-us_topic_0000002100586254__en-us_topic_0000002134166293_section161471750112613:

Rule 1.5: Restoring Parameters to Default Values in Connections Before Returning Them to the Pool
-------------------------------------------------------------------------------------------------

.. note::

   **Impact of rule violation:**

   -  When a connection is reused by another service, the parameters set by the service may also be reused. This can result in performance issues or service errors.

   **Solution:**

   -  Before returning the connection to the connection pool, use **SET SESSION AUTHORIZATION DEFAULT;RESET ALL;** to reset parameters.

   **Notes:**

   When connection pool is used for applications, if you set the global GUC parameter using **GS_GUC RELOAD** in GaussDB(DWS), restart the connection pool for the changes to be applied. This is because the modification only affects new connections in the connection pool.

.. _en-us_topic_0000002100586254__en-us_topic_0000002134166293_section8501127112718:

Rule 1.6: Manually Clearing Temporary Tables Created with a Connection Before Returning it to the Pool
------------------------------------------------------------------------------------------------------

.. note::

   **Impact of rule violation:**

   -  When a connection is reused by other services, an error may be reported when a temporary table is created.

   **Solution:**

   -  Before returning a connection to the connection pool, use **DROP** to clear the temporary table created by the current session.
