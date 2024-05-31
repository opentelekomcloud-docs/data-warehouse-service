:original_name: dws_01_1334.html

.. _dws_01_1334:

Real-Time Queries
=================

Going to the Real-time Query Page
---------------------------------

#. Log in to the GaussDB(DWS) management console.

#. On the **Clusters** > **Dedicated Clusters** page, locate the cluster to be monitored.

#. In the **Operation** column of the target cluster, click **Monitoring Panel**.

#. In the navigation pane, choose **Monitoring** > **Queries**.

   You can check the real-time information about all queries and sessions running in the cluster.

.. important::

   -  Real-time query is supported only in clusters of version 8.1.2 and later.
   -  To enable real-time query monitoring, choose **Settings** > **Monitoring**, click the **Monitoring Collection** tab, and enable **Real-Time Query Monitoring**. For details, see . Enabling real-time query may cause a large amount of data. Exercise caution when performing this operation.

Prerequisites
-------------

You need to set GUC parameters before viewing data on the monitoring page. If GUC parameters are not set, real-time or historical query may be unavailable. However, if this parameter is set, the cluster performance may deteriorate. Therefore, you need to balance the settings of related parameters. The following table describes recommended settings. For details about how to modify parameters, see "Modifying Database Parameters". :ref:`Setting GUC Parameters <en-us_topic_0000001707254569__en-us_topic_0000001372679770_en-us_topic_0000001076708521_section3665174263916>` provides parameter details.

.. table:: **Table 1** Recommended GUC parameter settings

   ========================= ================ ================
   GUC Parameter             CN Configuration DN Configuration
   ========================= ================ ================
   max_active_statements     10               10
   enable_resource_track     on               on
   resource_track_level      query            query
   resource_track_cost       0                0
   resource_track_duration   10               10
   enable_resource_record    on               on
   session_statistics_memory 1000MB           1000MB
   ========================= ================ ================

Querying Information
--------------------

You can view the queries statistics, the number of sessions, average session duration, number of queries, average query duration, and average query waiting time.

|image1|

Checking Live Sessions
----------------------

On the **Sessions** tab, you can browse the real-time information about all running queries,

-  Session ID
-  Username
-  Session duration
-  Application name
-  QueryBand
-  Client IP address
-  Connected CN
-  Session status. It can be:

   -  **idle**: The backend is waiting for new client commands.
   -  **active**: The backend is executing queries.
   -  **idle in transaction**: The backend is in a transaction, but there is no statement being executed in the transaction.
   -  **idle in transaction (aborted)**: The backend is in a transaction, but there are statements failed in the transaction.
   -  **fastpath function call**: The backend is executing a **fast-path** function.

-  Start time
-  Lock mode
-  Lock holding status
-  Locked object
-  Query SQL
-  Lock wait
-  Current query duration
-  Current query start time

|image2|

.. note::

   -  You can click a session ID to view the queries in the current session. For details, see :ref:`Viewing Historical Query Monitoring Details <en-us_topic_0000001659054514__en-us_topic_0000001372679826_en-us_topic_0000001076579507_section5136123611463>`.
   -  To terminate a session, select the session, click **Terminate a Session**, and confirm your operation.
   -  The fine-grained permission control function is added. Only users with the operate permission are able to terminate sessions. For users with the read-only permission, the **Terminate a Session** button is grayed out.

Checking Real-time Queries
--------------------------

On the **Queries** tab, you can browse all the queries that are running in a specified time period, including:

-  Query ID
-  Username
-  Database name
-  Submission time
-  Execution time
-  Statement
-  Lane
-  Query status. It can be:

   -  **idle**: The backend is waiting for new client commands.
   -  **active**: The backend is executing queries.
   -  **idle in transaction**: The backend is in a transaction, but there is no statement being executed in the transaction.
   -  **idle in transaction (aborted)**: The backend is in a transaction, but there are statements failed in the transaction.
   -  **fastpath function call**: The backend is executing a **fast-path** function.

|image3|

.. note::

   -  You can click a query ID to view the monitoring details. However, details cannot be displayed for queries whose ID is **0**. Query **0** indicates that an exception occurs during the query.
   -  To terminate a query, select the query, click **Terminate Query**, and confirm your operation.
   -  The fine-grained permission control function is added. Only users with the operate permission are able to terminate queries. For users with the read-only permission, the **Terminate Query** button is grayed out.

Viewing Real-time Query Monitoring Details
------------------------------------------

You can click a query ID to view the query details, including the basic information of query statements, real-time and historical resource consumption, SQL description, and query plan.

|image4|

.. |image1| image:: /_static/images/en-us_image_0000001711438108.png
.. |image2| image:: /_static/images/en-us_image_0000001711597584.png
.. |image3| image:: /_static/images/en-us_image_0000001759517025.png
.. |image4| image:: /_static/images/en-us_image_0000001759357153.png
