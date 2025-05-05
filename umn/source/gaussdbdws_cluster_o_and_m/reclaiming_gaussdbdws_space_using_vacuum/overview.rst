:original_name: dws_01_1161.html

.. _dws_01_1161:

Overview
========

GaussDB(DWS) provides the intelligent O&M feature to help users quickly and efficiently execute O&M tasks. Intelligent O&M selects a proper time window and concurrency to complete specified tasks based on the cluster load. During O&M tasks, intelligent O&M monitors user service changes and promptly adapts task execution policies to minimize the impact on user services. Periodic tasks and one-off tasks are supported, and you can configure the time window as required.

Intelligent O&M ensures high availability. When the cluster is abnormal, failed O&M tasks will be retried. If some steps of an O&M task cannot be completed due to an abnormal cluster, the failed steps will be skipped for cost saving.

The intelligent O&M page consists of the following parts:

-  Setting the common configurations of O&M tasks

   -  **Maximum number of concurrent O&M tasks in the VacuumFull user table**: applies to VacuumFull O&M tasks for each user table. You are advised to set this parameter based on the available disk space and I/O load within a specific time window. The value ranges from 1 to 24. The recommended value is **5**.
   -  Small CU threshold: helps identify small CU tables. A table is classified as small if its CU value is lower than the threshold. When the CU value is at or below the threshold, it triggers Vacuum. A higher threshold value increases the trigger sensitivity. The recommended default value for this parameter is **1000**.
   -  Small CU ratio: indicates the ratio of small CU tables to all CU tables. When the ratio is at or above the parameter value, Vacuum is triggered. A lower value increases the trigger sensitivity. The recommended default value for this parameter is **50%**.

-  Information about ongoing O&M tasks. (Currently, only VACUUM tasks are displayed. If disk space is insufficient because of table bloating, you can vacuum tables.).

   -  Frequent table creation and deletion can lead to table bloating. To free up space, you can run the **VACUUM** command on system catalogs.
   -  Frequently update and delete operations can lead to table bloating. To free up space, you can run the **VACUUM** or **VACUUM FULL** command on system catalogs.

-  O&M details: **O&M Plan** and **O&M Status**. **O&M Plan** displays the basic information about all O&M tasks, and **O&M Status** displays the running status.

.. note::

   -  This feature is supported only in 8.1.3 or later. The small CU threshold and small CU ratio are displayed only for version 9.1.0.200 or later clusters.
   -  After completing the **VACUUM FULL** O&M task, the system automatically performs the **ANALYZE** operation.
   -  Only cluster 8.1.3 and later versions support the common configuration module for O&M tasks. For earlier versions, contact technical support to upgrade them.
