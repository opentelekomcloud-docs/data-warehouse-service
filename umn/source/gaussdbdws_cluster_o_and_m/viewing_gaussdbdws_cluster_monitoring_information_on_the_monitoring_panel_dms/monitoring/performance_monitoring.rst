:original_name: dws_01_1337.html

.. _dws_01_1337:

Performance Monitoring
======================


Performance Monitoring
----------------------

#. Log in to the GaussDB(DWS) console.

#. Choose **Dedicated Clusters** > **Clusters** and locate the cluster to be monitored.

#. In the **Operation** column of the target cluster, click **Monitoring Panel**.

#. In the navigation pane on the left, choose **Monitoring** > **Performance Monitoring**. The **Performance Monitoring** page displays the resource consumption trends of clusters, databases, nodes, and instances.

   You can select a time range and check the performance trend in this range.

   -  By default, the monitoring information of the last hour is displayed.
   -  You can view the monitoring information of the last seven days.

Monitoring Panel
----------------

You can configure monitoring views by customizing monitoring panels. Monitoring panels are bound to users. After logging in to the system, you can view the user-defined monitoring panels.

-  Creating a monitoring panel: You can click **Create Panel** to customize a monitoring panel.
-  Modifying a monitoring panel: You can click **Modify** to change the name of a monitoring panel.
-  Deleting a monitoring panel: You can click **Delete** to delete a monitoring panel. The default monitoring panel cannot be deleted.
-  Sharing a monitoring panel: You can click **Share** to share a monitoring panel. The recipients can view the panel but cannot modify it.

Adding a Monitoring View
------------------------

Currently, DMS provides monitoring views for clusters, databases, nodes, and instances. You can click **Add View** to add a monitoring view as required. The monitoring metrics are as follows:

-  Cluster-level monitoring metrics: CPU usage, memory usage, disk usage, disk I/O, network I/O, status, number of abnormal CNs, read-only or not, number of sessions, number of active sessions, number of deadlocks, number of abnormal DNs, DN CPU usage, average number of transactions per second, average number of queries per second, capacity, mode capacity, number of stacked SQL statements, number of queries, resource pools, and number of transactions.

-  Database monitoring metrics: number of active sessions, number of sessions, number of inserted rows, number of updated rows, number of deleted rows, and capacity.

-  Instance-level monitoring metrics: instance memory usage and dynamic memory usage.

-  Node-level monitoring metrics: CPU usage, memory usage, average disk usage, TCP protocol stack retransmission rate, disk I/O, network I/O, total disk capacity, disk usage, disk read rate, disk write rate, disk I/O waiting time, disk I/O service time, disk I/O usage, NIC status, number of received packets, number of sent packets, number of discarded received packets, receiving rate, sending rate, CPU usage, and memory usage.

   .. note::

      -  A maximum of 20 views can be added to each panel. Adding too many views will increase the number of page requests and the rendering time.
      -  A maximum of 20 monitored objects can be selected in the node dimension. This feature is supported only in 8.1.3.310 and later cluster versions.

Exporting Monitoring Data
-------------------------

Performance Monitoring supports data export. You can click **Export Data** to further process data. By default, data in all monitoring views on the current page is exported. The export time range is subject to the selected time range.

.. note::

   Performance Monitoring allows data aggregation of different periods. You can aggregate raw data based on the corresponding sampling period to display indicator trends of a longer period.
