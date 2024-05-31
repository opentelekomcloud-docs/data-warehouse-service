:original_name: dws_01_1333.html

.. _dws_01_1333:

Historical Queries
==================

Going to the Historical Query Page
----------------------------------

#. Log in to the GaussDB(DWS) management console.

#. On the **Clusters** > **Dedicated Clusters** page, locate the cluster to be monitored.

#. In the **Operation** column of the target cluster, click **Monitoring Panel**.

#. In the navigation pane on the left, choose **Monitoring** > **History**.

   All the historical queries in the current cluster will be displayed.

.. note::

   -  Historical queries can be viewed only in clusters of version 8.1.2 and later.
   -  To enable historical query monitoring, choose **Settings** > **Monitoring**, click the **Monitoring Collection** tab, and enable **Historical Query Monitoring**. For details, see .

Checking Historical Queries
---------------------------

In the **History** area, you can browse all historical query information based on the specified time period, including:

-  Query ID

-  Username

-  Application name

-  Database name

-  Resource pool

-  Submission time

-  Blocking time (ms)

-  Execution time (ms)

-  CPU time (ms)

-  CPU time skew (%)

-  Statement

-  Connected CN

-  Client IP address

-  Query status

-  Completion time

-  Estimated execution time (ms)

-  Cancellation reason

   |image1|

   .. note::

      If you do not want to see historical system queries, you can toggle on **Hide System Queries**.

      |image2|

.. _en-us_topic_0000001659054514__en-us_topic_0000001372679826_en-us_topic_0000001076579507_section5136123611463:

Viewing Historical Query Monitoring Details
-------------------------------------------

You can click a historical query ID to view the query details, including the basic information of query statements, real-time and historical resource consumption, SQL description, and query plan.

|image3|

.. |image1| image:: /_static/images/en-us_image_0000001711438112.png
.. |image2| image:: /_static/images/en-us_image_0000001711597592.png
.. |image3| image:: /_static/images/en-us_image_0000001759517029.png
