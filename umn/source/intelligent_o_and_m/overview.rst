:original_name: dws_01_1161.html

.. _dws_01_1161:

Overview
========

GaussDB(DWS) provides the intelligent O&M feature to help users quickly and efficiently execute O&M tasks. Intelligent O&M selects a proper time window and concurrency to complete specified tasks based on the cluster load. During O&M tasks, intelligent O&M monitors user service changes and promptly adapts task execution policies to minimize the impact on user services. Periodic tasks and one-off tasks are supported, and you can configure the time window as required.

Intelligent O&M ensures high availability. When the cluster is abnormal, failed O&M tasks will be retried. If some steps of an O&M task cannot be completed due to an abnormal cluster, the failed steps will be skipped for cost saving.

The intelligent O&M page consists of the following parts:

-  Common configuration of O&M tasks: Currently, you can only configure **Maximum number of concurrent O&M tasks in the VacuumFull user table**. This configuration takes effect on all the VACUUM FULL tasks of user tables.
-  Information about ongoing O&M tasks. (Currently, only VACUUM tasks are displayed. If disk space is insufficient because of table bloating, you can vacuum tables.).

   -  Frequent table creation and deletion can lead to table bloating. To free up space, you can run the **VACUUM** command on system catalogs.
   -  Frequently update and delete operations can lead to table bloating. To free up space, you can run the **VACUUM** or **VACUUM FULL** command on system catalogs.

-  O&M details: **O&M Plan** and **O&M Status**. **O&M Plan** displays the basic information about all O&M tasks, and **O&M Status** displays the running status.

|image1|

.. note::

   -  This feature is supported only in 8.1.3 or later.
   -  The intelligent O&M function is not supported in hybrid data warehouses (standalone mode).
   -  Only cluster 8.1.3 and later versions support the common configuration module for O&M tasks. For earlier versions, contact technical support to upgrade them.

.. |image1| image:: /_static/images/en-us_image_0000001711439924.png
