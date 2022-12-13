:original_name: dws_01_8115.html

.. _dws_01_8115:

Performing a Primary/Standby Switchback
=======================================

Context
-------

In the **Unbalanced** state, the number of primary instances on some nodes increases. As a result, the load pressure is high. In this case, the cluster is normal, but the overall performance is lower than that in the balanced state. During off-peak hours, you are advised to perform a switchback to restore the primary/standby relationship of your cluster.

.. note::

   -  Only 8.1.1.202 and later versions support primary/standby switchback.
   -  You are advised to perform a switchback during off-peak hours. Switchback interrupts services for a short period of time. The actual interruption duration depends on your service volume.

Procedure
---------

#. Log in to the GaussDB(DWS) management console.

#. On the **Clusters** page, find a cluster in **Unbalanced** state.

#. In the **Operation** column of the cluster, choose **More** > **Switchback**.

   |image1|

#. In the dialog box that is displayed, confirm that the service is in off-peak hours, and click **Yes**. A message will be displayed in the upper right corner, indicating that the switchback request is being processed.

   |image2|

#. Check the cluster status. During the switchback, the cluster status is **Switching back**. After the switchback, the cluster status will change to **Available**.

   |image3|

.. |image1| image:: /_static/images/en-us_image_0000001216175352.png
.. |image2| image:: /_static/images/en-us_image_0000001260615077.png
.. |image3| image:: /_static/images/en-us_image_0000001216015456.png
