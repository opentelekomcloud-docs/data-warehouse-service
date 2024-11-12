:original_name: dws_01_1333.html

.. _dws_01_1333:

Historical Queries
==================

Going to the Historical Query Page
----------------------------------

#. Log in to the GaussDB(DWS) console.

#. On the **Clusters** > **Dedicated Clusters** page, locate the cluster to be monitored.

#. In the **Operation** column of the target cluster, click **Monitoring Panel**.

#. In the navigation pane on the left, choose **Monitoring** > **History**.

   All the historical queries in the current cluster will be displayed.

.. note::

   -  Historical queries can be viewed only in clusters of version 8.1.2 and later.
   -  To enable historical query monitoring, choose **Settings** > **Monitoring**, click **Monitoring Collection**, and enable **Historical Query Monitoring**. For details, see :ref:`Monitoring Collection <en-us_topic_0000001924728812__en-us_topic_0000001076708691_section149871230683>`. Enabling history query may cause a large amount of data. Exercise caution when performing this operation.

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

   .. note::

      If you do not want to see historical system queries, you can toggle on **Hide System Queries**.

.. _en-us_topic_0000001924569492__en-us_topic_0000001076579507_section5136123611463:

Viewing Historical Query Monitoring Details
-------------------------------------------

You can click a historical query ID to view the query details, such as basic information about query statements, real-time resource consumption during execution, complete SQL statements, and query plans.

|image1|

.. |image1| image:: /_static/images/en-us_image_0000001924729352.png
