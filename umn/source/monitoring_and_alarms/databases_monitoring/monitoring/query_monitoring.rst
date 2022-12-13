:original_name: dws_01_1334.html

.. _dws_01_1334:

Query Monitoring
================


Query Monitoring
----------------

#. Log in to the GaussDB(DWS) management console.

#. On the **Clusters** page, locate the target cluster.

#. In the **Operation** column of the target cluster, click **Monitoring Panel**.

#. In the navigation pane on the left, choose **Monitoring** > **Query Monitoring**.

   The **Query Monitoring** page displays the real-time information about all queries that are running in a cluster and the historical information about the queries that have been run.

Prerequisites
-------------

You need to set GUC parameters before viewing data on the monitoring page. If GUC parameters are not set, real-time or historical query may be unavailable. However, if this parameter is set, the cluster performance may deteriorate. Therefore, you need to balance the settings of related parameters. The following table describes recommended settings. For details about how to modify parameters, see "Modifying Database Parameters". :ref:`Setting GUC Parameters <en-us_topic_0000001134400694__en-us_topic_0000001076708521_section3665174263916>` provides parameter details.

.. table:: **Table 1** Recommended GUC parameter settings

   ========================= ================ ================
   GUC Parameter             CN Configuration DN Configuration
   ========================= ================ ================
   max_active_statements     10               10
   enable_resource_track     on               on
   resource_track_level      query            query
   resource_track_cost       0                0
   resource_track_duration   0                0
   enable_resource_record    on               on
   session_statistics_memory 1000MB           1000MB
   ========================= ================ ================

Querying Information
--------------------

In this area, you can browse the number of queries in different status, including Running, Blocked, Delayed, Canceled, Fast Lane, and Slow Lane.

|image1|

Real-Time Query
---------------

In the **Real-Time Query** area, you can browse the real-time information about all running queries, including:

-  Query ID

-  User Name

-  Application Name

-  Database Name

-  Workload Queue

-  Submitted

-  Blocking Time (ms)

-  Execution Time (ms)

-  CPU Time (ms)

-  CPU Time Skew (%)

-  Average Written Data (MB)

-  Statement

-  Connected CN

-  Client IP Address

-  Lane

-  Query Status

-  Session ID

-  Queuing Status

   |image2|

   .. note::

      Click the query ID to view the details. However, details cannot be displayed for queries whose ID is **0**. Query **0** indicates that an exception occurs during the query.

Terminating a Query
-------------------

Select a query to be terminated, click **Terminate Query**, and confirm your operation.

.. note::

   The fine-grained permission control function is added. Only users with the operate permission are able to terminate queries. For users with the read-only permission, the **Terminate Query** button is grayed out.

Historical Query
----------------

In the **History** area, you can browse all historical query information based on the specified time period, including:

-  Query ID

-  User Name

-  Application Name

-  Database Name

-  Workload Queue

-  Submitted

-  Blocking Time (ms)

-  Execution Time (ms)

-  CPU Time (ms)

-  CPU Time Skew (%)

-  Average Written Data (MB)

-  Statement

-  Connected CN

-  Client IP Address

-  Query Status

-  Completed

-  Estimated Execution Time (ms)

-  Cancellation Reason

   |image3|

Viewing Query Monitoring Details
--------------------------------

You can click a query ID to view the query details, including the basic information of query statements, real-time and historical resource consumption, SQL description, and query plan.

|image4|

.. |image1| image:: /_static/images/en-us_image_0000001180320417.png
.. |image2| image:: /_static/images/en-us_image_0000001180320421.png
.. |image3| image:: /_static/images/en-us_image_0000001180320419.png
.. |image4| image:: /_static/images/en-us_image_0000001134400986.png
