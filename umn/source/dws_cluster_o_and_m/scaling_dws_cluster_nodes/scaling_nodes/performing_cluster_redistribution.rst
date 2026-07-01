:original_name: dws_01_8201.html

.. _dws_01_8201:

Performing Cluster Redistribution
=================================

Data redistribution, where data in existing nodes is evenly allocated to the new nodes after you scale out a cluster, is a time-consuming yet crucial task that accelerates service response.

By default, redistribution is automatically started after cluster scale-out. For enhanced reliability, disable the automatic redistribution function and manually start a redistribution task after the scale-out is successful. In this way, both scale-out and redistribution can be retried upon failures.

DWS supports :ref:`offline redistribution <en-us_topic_0000002310493850__en-us_topic_0000001423159669_section17985143110112>`. The redistribution mode is the same as the scale-out mode. If online scale-out is used for the cluster, online redistribution is also used by default.

Before redistribution starts or when redistribution is paused, you can set redistribution priorities for the tables that have not been redistributed by schema or table.

Notes and Constraints
---------------------

-  The cluster redistribution function is supported in 8.1.1.200 or later cluster versions.
-  This function can be manually enabled only when the cluster task information displays **To be redistributed** after scale-out.
-  You can also select the redistribution mode when you configure cluster scale-out (see :ref:`Configure advanced parameters <en-us_topic_0000002270373757__li85162515474>`).
-  Redistribution queues are sorted based on the relpage size of tables. To ensure that the relpage size is correct, you are advised to perform the **ANALYZE** operation on the tables to be redistributed.
-  **Constraints on offline redistribution are as follows:**

   -  In offline redistribution mode, the database does not support DDL and DCL operations. Tables that are being redistributed support only simple DQL operations.
   -  During table redistribution, a shared lock is added to tables. All insert, update, and delete operations as well as DDL operations on the tables are blocked for a long time, which may cause a lock wait timeout. Do not perform queries that take more than 20 minutes during the redistribution (the default time for applying for the write lock during redistribution is 20 minutes). Otherwise, data redistribution may fail due to lock wait timeout.

-  The function of viewing redistribution details is supported by 8.1.1.200 and later cluster versions. Details about the data table redistribution progress are supported only by 8.2.1 and later cluster versions.
-  You can check redistribution details only if the cluster is being redistributed, failed to be redistributed, or is suspended. There may be a delay in the statistics update.

.. _en-us_topic_0000002310493850__en-us_topic_0000001423159669_section17985143110112:

Offline Redistribution
----------------------

#. Log in to the DWS console.

#. Choose **Dedicated Clusters** > **Clusters**. All clusters are displayed by default.

#. In the **Operation** column of the target cluster, choose **More** > **Scale Node** > **Redistribute**.

   The **Redistribution** page is displayed.

#. On the **Redistribute** page that is displayed, keep the default **Offline** redistribution mode and click **Next: Confirm** to submit the task.

Viewing Redistribution Details
------------------------------

#. Log in to the DWS console.

#. Choose **Dedicated Clusters** > **Clusters**. All clusters are displayed by default.

#. In the **Task Information** column of a cluster, click **View Details**.

#. On the **View Redistribution Details** page, you can check the monitoring information, including the redistribution mode, redistribution progress, and table redistribution details of the current cluster. You can pause and resume redistribution, set the redistribution priority, and change the number of concurrent redistribution tasks.

   Check the redistribution status, configuration, progress, and redistribution details of all the tables in a specified database. Specify a database that and can be searched by table redistribution status and table name. If all the tables in a database have completed redistribution, no data will be displayed for the database.

#. When redistribution is paused, you can set the redistribution priority (in schema or table dimension), and redistribution will be performed based on the configured redistribution sequence. You can also set the redistribution priority before the redistribution starts.

#. The number of concurrent redistribution tasks can be adjusted during redistribution.

   .. note::

      Cluster 8.1.0 and earlier versions do not support dynamic adjustment. To change redistribution concurrency, suspend redistribution first.

#. Check the redistribution progress. After the redistribution is complete, the amount of completed data, amount of remaining data, number of completed tables, number of remaining tables, and average rate during redistribution are displayed.
