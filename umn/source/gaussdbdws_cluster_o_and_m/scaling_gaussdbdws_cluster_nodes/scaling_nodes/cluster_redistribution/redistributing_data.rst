:original_name: dws_01_8201.html

.. _dws_01_8201:

Redistributing Data
===================

Data redistribution, where data in existing nodes is evenly allocated to the new nodes after you scale out a cluster, is a time-consuming yet crucial task that accelerates service response.

By default, redistribution is automatically started after cluster scale-out. For enhanced reliability, disable the automatic redistribution function and manually start a redistribution task after the scale-out is successful. In this way, both scale-out and redistribution can be retried upon failures.

GaussDB(DWS) supports :ref:`offline redistribution <en-us_topic_0000002203312109__section858715017148>`.

Before redistribution starts or when redistribution is paused, you can set redistribution priorities for the tables that have not been redistributed by schema or table.

.. important::

   -  The cluster redistribution function is supported in 8.1.1.200 or later cluster versions.
   -  This function can be manually enabled only when the cluster task information displays **To be redistributed** after scale-out.
   -  You can also select the redistribution mode when you configure cluster scale-out (see :ref:`Configure advanced parameters <en-us_topic_0000002203312085__li85162515474>`).
   -  Redistribution queues are sorted based on the relpage size of tables. To ensure that the relpage size is correct, you are advised to perform the **ANALYZE** operation on the tables to be redistributed.

.. _en-us_topic_0000002203312109__section858715017148:

Offline Redistribution
----------------------

**Precautions**

-  In offline redistribution mode, the database does not support DDL and DCL operations. Tables that are being redistributed support only simple DQL operations.
-  During table redistribution, a shared lock is added to tables. All insert, update, and delete operations as well as DDL operations on the tables are blocked for a long time, which may cause a lock wait timeout. Do not perform queries that take more than 20 minutes during the redistribution (the default time for applying for the write lock during redistribution is 20 minutes). Otherwise, data redistribution may fail due to lock wait timeout.

**Procedure**

#. Log in to the GaussDB(DWS) console.

#. Choose **Dedicated Clusters** > **Clusters**. All clusters are displayed by default.

#. In the **Operation** column of the target cluster, choose **More** > **Scale Node** > **Redistribute**, as shown in the following figure.

   The **Redistribution** page is displayed.

#. On the **Redistribute** page that is displayed, keep the default **offline** redistribution mode and click **Next: Confirm** to submit the task.
