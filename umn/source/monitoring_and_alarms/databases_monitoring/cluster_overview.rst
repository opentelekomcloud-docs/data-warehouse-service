:original_name: dws_01_00132.html

.. _dws_01_00132:

Cluster Overview
================


Cluster Overview
----------------

#. Log in to the GaussDB(DWS) management console.

#. On the **Clusters** page, locate the target cluster.

#. In the **Operation** column of the target cluster, click **Monitoring Panel**. The database monitoring page is displayed.

#. In the navigation pane on the left, click **Cluster Overview**.

   On the page that is displayed, you can view the cluster status, real-time resource consumption, top SQL statements, cluster resource consumption, and key database metrics.

Cluster Status
--------------

In the **Cluster Status** area, you can view the status of the current cluster and the number of available resources, including the numbers of nodes, CNs, and databases.

|image1|

Resource Consumption
--------------------

In **Resource Consumption**, you can view the real-time resource consumption of the current cluster, including **Memory Usage**, **Disk Usage**, **CPU Usage**, **Disk I/O (KB/s)**, and **Network I/O (KB/s)**.

|image2|

TOP SQL
-------

In the **TOP SQL** area, you can query the SQL statements that take the longest time and have the largest amount of data flushed to disks in the current cluster.

|image3|

.. note::

   Top 5 SQL statement query is implemented by **max_ctime**, which displays the query duration and the amount of data written to disks in the current collection period. If no query is executed during off-peak hours, the top 5 time-consuming query page will not be refreshed.

Key Metrics
-----------

On the **Cluster Overview** page, you can also view the database metrics of the current cluster, including **Sessions** and **Queries**.

|image4|

|image5|

.. |image1| image:: /_static/images/en-us_image_0000001180320467.png
.. |image2| image:: /_static/images/en-us_image_0000001180440403.png
.. |image3| image:: /_static/images/en-us_image_0000001180440401.png
.. |image4| image:: /_static/images/en-us_image_0000001152748072.png
.. |image5| image:: /_static/images/en-us_image_0000001152588340.png
