:original_name: dws_01_0821.html

.. _dws_01_0821:

Scaling In a Cluster
====================

You can scale in your clusters on the console to release unnecessary computing and storage resources provided by GaussDB(DWS).

Impact on the System
--------------------

-  Before the scale-in, exit the client connections that have created temporary tables, because temporary tables created before or during the scale-in will become invalid and operations performed on these temporary tables will fail. Temporary tables created after the scale-in will not be affected.
-  If you start a scale-in, an automatic snapshot will be created for the cluster before scale-in. If you do not need the snapshot, you can disable the automated backup function on the scale-in page.
-  In a cluster that is being scaled in, the following functions are disabled: cluster restart, cluster scale-out, snapshot creation, node management, intelligent O&M, resource management, parameter modification, security configurations, log service, database administrator password resetting, and cluster deletion.
-  During offline scale-in, stop all services or run only a few query statements. During table redistribution, a shared lock is added to tables. All insert, update, and delete operations as well as DDL operations on the tables are blocked for a long time, which may cause a lock wait timeout. After a table is redistributed, you can access the table. Do not perform queries that take more than 20 minutes during the redistribution (the default time for applying for the write lock during redistribution is 20 minutes). Otherwise, data redistribution may fail due to lock wait timeout.
-  During online scale-in, you can perform insert, update, and delete operations on tables, but data updates may still be blocked for a short period of time. Redistribution consumes lots of CPU and I/O resources, which will greatly impact job performance. Therefore, perform redistribution when services are stopped or during periods of light load.
-  During offline scale-in, if a node is deleted while DDL statements are executed (to create a schema or function), these statements may report errors, because the DN cannot be found. In this case, you simply need to retry the statements.
-  If a cluster scale-in fails, the database does not automatically roll back the scale-in operation, and no O&M operations can be performed. In this case, you need to click the **Scale In** on the console to try again.

Prerequisites
-------------

-  The cluster is in **Available** state, is not read-only, and there is no data being redistributed in the cluster.
-  A cluster configuration file has been generated, and configuration information is consistent with the current cluster configuration.
-  Before the scale-in operation starts, the value of **default_storage_nodegroup** is **installation**.
-  The cluster is configured in the ring mode. A ring is the smallest unit for scale-in. Four or five hosts form a ring. The primary, standby, and secondary DNs are deployed in this ring.
-  The scale-in host does not contain the GTM, ETCD, or CM Server component.
-  There are no CNs on the nodes to be scaled in.
-  Scale-in does not support rollback but supports retry. A data redistribution failure after a scale-in does not affect services. You can complete scale-in at other appropriate time. Otherwise, unbalanced data distribution will persist for a long time.
-  Before redistribution, ensure that the **data_redis** schema in the corresponding database is reserved for redistribution and that no user operation on it or its tables is allowed. During redistribution, **data_redis** is used. After the operation is complete, the schema will be deleted. User tables (if any) in the schema will also be deleted.
-  **gs_cgroup** cannot be used during scale-in.
-  Before the scale-in, check the remaining capacity of the cluster. The nodes remaining in a scale-in must have sufficient space to store the data of the entire cluster. Otherwise, the scale-in cannot be properly performed.

   -  The used physical disk space on each node is less than 80%.
   -  All the users and roles use less than 80% of resource quota in total.
   -  The estimated space usage after scale-in must be less than 80%.
   -  The available space is 1.5 times larger than the maximum size of a single table.

      .. note::

         To check the maximum size of a single table, use the following inspection tool:

         ::

            gs_check -i CheckBiggestTable -L

-  Automatic removal of faulty CNs is disabled during the scale-in and is enabled after the scale-in is complete.

Procedure
---------

#. Log in to the GaussDB(DWS) management console.

#. Choose **Clusters**.

#. In the **Operation** column of the target cluster, choose **More** > **Scale Node** > **Scale In**.

   |image1|

#. The scale-in page is displayed. You can select the number of nodes to be scaled in. The automated backup function is enabled by default.

   |image2|

#. Click **Next: Confirm**. The system will check the cluster status before scale-in. If your cluster fails the check, an error message will be displayed.

   |image3|

#. After the check is passed, click **Confirm** to return to the cluster list. The cluster status is **Scaling in**. Wait for a while.

   |image4|

.. |image1| image:: /_static/images/en-us_image_0000001466754762.png
.. |image2| image:: /_static/images/en-us_image_0000001466595110.png
.. |image3| image:: /_static/images/en-us_image_0000001517754461.png
.. |image4| image:: /_static/images/en-us_image_0000001517355437.png
