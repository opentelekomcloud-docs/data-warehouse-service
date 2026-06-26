:original_name: dws_01_00132.html

.. _dws_01_00132:

Viewing the DWS Cluster Monitoring Overview
===========================================

DMS is provided by DWS to ensure the fast and stable running of databases. It collects, monitors, and analyzes the disk, network, and OS metric data used by the service database, as well as key performance metric data of cluster running. It also diagnoses database hosts, instances, and service SQL statements based on the collected metrics to expose key faults and performance problems in a database in a timely manner, and guides customers to optimize and resolve the problems.

On the cluster overview page, you can view the cluster status, real-time resource consumption, cluster resource consumption, and key database metrics. Metrics can be collected and displayed on the cluster overview page only if their collection items are enabled. If a collection item is disabled, its metric will not be displayed, and a prompt will be displayed indicating this problem. To resolve this problem, enable the corresponding collection item by referring to :ref:`Enabling and Disabling DWS Monitoring Metrics <dws_01_00135>`.

Notes and Constraints
---------------------

-  Database monitoring is supported by 8.1.1.200 and later versions.
-  The database monitoring function and Cloud Eye monitor different data sources. In database monitoring, the size of a database is the total disk space used by the database, including the space occupied due to bloating.

Cluster Status
--------------

In the **Cluster Status** area, you can view the statistics about the current cluster status and instance status, including cluster statistics in the last 24 hours, cluster specifications, available/total CNs and DNs, used/total disk capacity, the number of CCN switchovers in the last 24 hours, and the number of primary/standby DN switchovers in the last 24 hours.

Alarms
------

In the **Alarms** area, you can view all the uncleared alarms of the current cluster and the alarms generated in the last seven days. You can click **More** in the upper right corner to view details about the existing cluster alarms. For details, see :ref:`Viewing and Subscribing to DWS Cluster Alarms <dws_01_1240>`.

Cluster Resources
-----------------

In the **Cluster Resources** area, you can view the resource usage of the current cluster, including the average CPU usage, disk I/O, disk usage, maximum disk usage, memory usage, and network I/O. You can click the metric of a resource to view its trend in the last 24 hours and the top five services that are occupying this resource. You can click **More** in the upper right corner of the area to go to the **Node Monitoring** page. Nodes are sorted by the metric value. For details, see :ref:`Viewing Node Metrics of a DWS Cluster <dws_01_1331>`.

Workloads
---------

In the **Workloads** area, you can view the workload metrics of the current database, including TPS, QPS, stacked SQL queries, and resource pools. You can also click a workload metric to view its trend in the last 24 hours. The **SQL Stack Queries** and **Resource Pools** metrics depend on real-time query monitoring. If this function is disabled, no data will be displayed for the metrics.

Data Volume
-----------

In the **Data Volume** area, you can view the used capacity of the current database and schema. You can click a capacity metric to view the database or schema capacity trend in the last 24 hours and the top five databases or schemas ranked by capacity usage in the current cluster. You can click **More** in the upper right corner of the area to go to the **Database Monitoring** page. Databases are sorted by used capacity. For details, see :ref:`Viewing DWS Database Details <dws_01_1332>`.

.. note::

   The database capacity data is collected once a day. Therefore, the data volume fluctuates greatly. To view real-time capacity monitoring information, choose **Node Monitoring** > **Disks**.

Queries
-------

In the **Queries** area, you can check the average number of queries, sessions, and transactions. You can click a metric to view its trend in the last 24 hours. The **Average Queries** and **Average Sessions** metrics depend on the real-time query monitoring function. If this function is disabled, no data will be displayed for the metrics.
