:original_name: dws_01_1337.html

.. _dws_01_1337:

Performance Monitoring
======================


Performance Monitoring
----------------------

#. Log in to the GaussDB(DWS) management console.

#. On the **Clusters** page, locate the target cluster.

#. In the **Operation** column of the target cluster, click **Monitoring Panel**.

#. In the navigation pane on the left, choose **Monitoring** > **Performance Monitoring**.

   The **Performance Monitoring** page displays the resource consumption trends of clusters and databases.

Monitoring Panel
----------------

You can configure monitoring views by customizing monitoring panels. Monitoring views are bound to users. After logging in to the system, you can view the user-defined monitoring panels.

-  Creating a monitoring panel: You can click **Create Panel** to customize a monitoring panel.
-  Modifying a monitoring panel: You can click **Modify** to change the name of a monitoring panel.
-  Deleting a monitoring panel: You can click **Delete** to delete a monitoring panel. The default monitoring panel cannot be deleted.

|image1|

Adding a Monitoring View
------------------------

Currently, DMS provides two types of monitoring views: cluster and database. You can click **Add View** to add a monitoring view as required The monitoring indicators are as follows:

-  Cluster: CPU Usage, Memory Usage, Disk Usage, Disk I/O, Network I/O, Status, Abnormal CNs, Read-only, Sessions, Queries, Deadlocks, Abnormal DNs, CPU Usage of DNs, TPS, and QPS

-  Database: Length of the Request Waiting Queue, Sessions, Queries, Submitted Transactions, Rollback Transactions, Scanning Rows, Index Query Rows, Inserted Rows, Updated Rows, Deleted Rows, and Capacity, and TPS

   |image2|

   |image3|

   .. note::

      -  A maximum of 20 views can be added to each panel. Adding too many views will increase the number of page requests and the rendering time.
      -  Performance Monitoring allows you to view data trends in different time ranges in five modes, as shown in the following figure.

      |image4|

Exporting Monitoring Data
-------------------------

Performance Monitoring supports data export. You can click **Export Data** to further process data. By default, data in all monitoring views on the current page is exported. The export time range is subject to the selected time range.

|image5|

.. note::

   Performance Monitoring allows data aggregation of different periods. You can aggregate raw data based on the corresponding sampling period to display indicator trends of a longer period.

   |image6|

.. |image1| image:: /_static/images/en-us_image_0000001180320289.png
.. |image2| image:: /_static/images/en-us_image_0000001134400850.png
.. |image3| image:: /_static/images/en-us_image_0000001180440217.png
.. |image4| image:: /_static/images/en-us_image_0000001180440221.png
.. |image5| image:: /_static/images/en-us_image_0000001180440215.png
.. |image6| image:: /_static/images/en-us_image_0000001180440219.png
