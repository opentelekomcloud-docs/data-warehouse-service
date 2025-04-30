:original_name: dws_01_1333.html

.. _dws_01_1333:

Historical Queries
==================

Going to the Historical Query Page
----------------------------------

#. Log in to the GaussDB(DWS) console.

#. Choose **Dedicated Clusters** > **Clusters** and locate the cluster to be monitored.

#. In the **Operation** column of the target cluster, click **Monitoring Panel**.

#. In the navigation pane on the left, choose **Monitoring** > **History**.

   All the historical queries in the current cluster will be displayed.

.. note::

   -  Historical queries can be viewed only in clusters of version 8.1.2 and later.
   -  The historical query monitoring function is disabled by default. To enable it, choose **Settings** > **Monitoring**, click **Monitoring Collection**, and enable **Historical Query Monitoring**. For details, see :ref:`Monitoring Collection <en-us_topic_0000002203426701__en-us_topic_0000001076708691_section149871230683>`. Enabling history query may cause a large amount of data. Exercise caution when performing this operation.

Checking Historical Queries
---------------------------

In the **History** area, you can browse all historical query information based on the specified time period. You can click the setting button in the upper right corner of the list to select the metrics to be displayed in the list. The metrics are as follows:

Query ID, username, application name, database name, resource pools, submission time, blocking time (ms), execution time (ms), CPU time (ms), CPU time skew (%), average spill to disk (MB), query statement, access CN, client IP address, query status, completion time, estimated total execution time (ms), and cancellation reason.

.. note::

   If you do not want to see historical system queries, you can toggle on **Hide System Queries**.

Viewing Historical Query Monitoring Details
-------------------------------------------

You can click a historical query ID to view the query details, such as basic information about query statements, real-time resource consumption during execution, complete SQL statements, and query plans.

|image1|

.. |image1| image:: /_static/images/en-us_image_0000002167906540.png
