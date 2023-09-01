:original_name: dws_01_00135.html

.. _dws_01_00135:

Settings
========

The **Monitoring** page displays the collection period and data aging period of monitoring metrics.

.. note::

   -  The cluster monitoring function is enabled by default.
   -  Disable the function if the cluster is being recovered. Enable the function when the fault is rectified.
   -  When a node in the cluster is powered off or the management IP address of the cluster is unavailable, the cluster monitoring switch and the button for configuring cluster indicator collection are unavailable.

.. _en-us_topic_0000001467074038__en-us_topic_0000001076708691_section149871230683:

Monitoring Collection
---------------------

#. Log in to the GaussDB(DWS) management console.

#. On the **Clusters** page, locate the target cluster.

#. In the **Operation** column of the target cluster, choose **Monitoring Panel**. The database monitoring page is displayed.

#. In the navigation pane on the left, choose **Settings** > **Monitoring**. You can reconfigure the collection frequency or disable the collection of the monitoring item.

   |image1|

   .. note::

      Click **Update** of **DDL Audit** to reset the automatic audit frequency or audit items.

      |image2|

Collection Storage
------------------

#. Log in to the GaussDB(DWS) management console.

#. On the **Clusters** page, locate the target cluster.

#. In the **Operation** column of the target cluster, choose **Monitoring Panel**. The database monitoring page is displayed.

#. In the navigation pane on the left, choose **Settings** > **Monitoring** and switch to the **Collection Storage** tab page. Update the retention days.

   |image3|

.. |image1| image:: /_static/images/en-us_image_0000001467074270.png
.. |image2| image:: /_static/images/en-us_image_0000001517754469.png
.. |image3| image:: /_static/images/en-us_image_0000001517914049.png
