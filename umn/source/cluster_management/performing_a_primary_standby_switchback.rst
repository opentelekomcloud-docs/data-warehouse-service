:original_name: dws_01_8115.html

.. _dws_01_8115:

Performing a Primary/Standby Switchback
=======================================

Context
-------

In the **Unbalanced** state, the number of primary instances on some nodes increases. As a result, the load pressure is high. In this case, the cluster is normal, but the overall performance is not as good as that in a balanced state. Restore the primary-standby relationship to recover the cluster to the available state.

.. note::

   -  Only 8.1.1.202 and later versions support primary/standby cluster restoration.
   -  Cluster restoration interrupts services for a short period of time. The interruption duration depends on the service volume. You are advised to perform this operation during off-peak hours.

Procedure
---------

#. Log in to the GaussDB(DWS) console.

#. On the **Clusters** > **Dedicated Clusters** page, locate the cluster in unbalanced state.

#. In the **Cluster Status** column of the cluster, click **Fix** under **Unbalanced**.

   |image1|

#. In the dialog box that is displayed, confirm that the service is in off-peak hours, and click **Yes**. A message will be displayed in the upper right corner, indicating that the switchback request is being processed.

#. Check the cluster status. During the switchback, the cluster status is **Switching back**. After the switchback, the cluster status will change to **Available**.

.. |image1| image:: /_static/images/en-us_image_0000001924729148.png
