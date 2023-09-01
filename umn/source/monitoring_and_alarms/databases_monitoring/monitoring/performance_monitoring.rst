:original_name: dws_01_1337.html

.. _dws_01_1337:

Performance Monitoring
======================


Performance Monitoring
----------------------

#. Log in to the GaussDB(DWS) management console.

#. On the **Clusters** page, locate the target cluster.

#. In the **Operation** column of the target cluster, click **Monitoring Panel**.

#. In the navigation pane on the left, choose **Monitoring** > **Performance Monitoring**. The **Performance Monitoring** page displays the resource consumption trends of clusters, databases, and nodes.

   You can select a time range and check the performance trend in this range.

   -  By default, the monitoring information of the last hour is displayed.
   -  You can view the monitoring information of the last seven days.

   |image1|

Monitoring Panel
----------------

You can configure monitoring views by customizing monitoring panels. Monitoring panels are bound to users. After logging in to the system, you can view the user-defined monitoring panels.

-  Creating a monitoring panel: You can click **Create Panel** to customize a monitoring panel.
-  Modifying a monitoring panel: You can click **Modify** to change the name of a monitoring panel.
-  Deleting a monitoring panel: You can click **Delete** to delete a monitoring panel. The default monitoring panel cannot be deleted.

|image2|

Adding a Monitoring View
------------------------

Currently, DMS provides monitoring views for clusters, databases, and nodes. You can click **Add View** to add a monitoring view as required. The monitoring metrics are as follows:

-  Cluster: CPU Usage, Memory Usage, Disk Usage, Disk I/O, Network I/O, Status, Abnormal CNs, Read-only, Sessions, Queries, Deadlocks, Abnormal DNs, CPU Usage of DNs, TPS, and QPS

-  Database metrics: query waiting queue length, number of sessions, number of queries, number of inserted rows, number of updated rows, number of deleted rows, and capacity.

-  Node: CPU usage, memory usage, average disk usage, disk I/O, TCP retransmission rate, network I/O, total disk space, disk usage, disk read rate, disk write rate, disk I/O wait time, disk I/O service time, disk I/O usage, NIC status, number of received packets, number of sent packets, number of lost packets received, receive rate, and transmit rate.

   |image3|

   |image4|

   .. note::

      A maximum of 20 views can be added to each panel. Adding too many views will increase the number of page requests and the rendering time.

Exporting Monitoring Data
-------------------------

Performance Monitoring supports data export. You can click **Export Data** to further process data. By default, data in all monitoring views on the current page is exported. The export time range is subject to the selected time range.

|image5|

.. note::

   Performance Monitoring allows data aggregation of different periods. You can aggregate raw data based on the corresponding sampling period to display indicator trends of a longer period.

   |image6|

.. |image1| image:: /_static/images/en-us_image_0000001576698381.png
.. |image2| image:: /_static/images/en-us_image_0000001466754606.png
.. |image3| image:: /_static/images/en-us_image_0000001517754305.png
.. |image4| image:: /_static/images/en-us_image_0000001466754602.png
.. |image5| image:: /_static/images/en-us_image_0000001517913885.png
.. |image6| image:: /_static/images/en-us_image_0000001466914222.png
