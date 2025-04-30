:original_name: dws_01_0035.html

.. _dws_01_0035:

Handling Abnormal GaussDB(DWS) Clusters
=======================================

.. _en-us_topic_0000002167905968__section112712497:

Removing the Read-only Status
-----------------------------

A cluster in read-only status does not allow write operations. You can remove this status on the management console. A cluster becomes read-only probably because of high disk usage. For how to solve this problem, see "Solution to High Disk Usage and Cluster Read-Only" in *Data Warehouse Service (DWS) Troubleshooting Guide*.

.. note::

   -  Clusters of version 1.7.2 or later support removal of the read-only state.
   -  In 8.2.0 and later versions, you can free up disk space by using **DROP/TRUNCATE TABLE** in a read-only cluster.

**Impacts on the System**

-  You can cancel the read-only status only when a cluster is read-only.
-  When a cluster is in read-only status, stop the write tasks to prevent data loss caused by used up disk space.
-  After the read-only status is canceled, clear the data as soon as possible to prevent the cluster from entering the read-only status again after a period of time.

**Procedure**

#. Log in to the GaussDB(DWS) console.

#. Choose **Dedicated Clusters** > **Clusters**.

   All clusters are displayed by default.

#. In the row containing the cluster whose cluster status is **Read-only**, click **Cancel Read-only**.

#. In the dialog box that is displayed, click **OK** to confirm and remove the read-only status for the cluster.

.. _en-us_topic_0000002167905968__section17567184318267:

Performing a Primary/Standby Switchback
---------------------------------------

In the **Unbalanced** state, the number of primary instances on some nodes increases. As a result, the load pressure is high. In this case, the cluster is normal, but the overall performance is not as good as that in a balanced state. Restore the primary-standby relationship to recover the cluster to the available state.

.. note::

   -  Only and later versions support primary/standby cluster restoration.
   -  Cluster restoration interrupts services for a short period of time. The interruption duration depends on the service volume. You are advised to perform this operation during off-peak hours.

#. Log in to the GaussDB(DWS) console.

#. Choose **Dedicated Clusters** > **Clusters** and locate the cluster whose load is unbalanced.

#. In the **Cluster Status** column of the cluster, click **Fix** under **Unbalanced**.

   |image1|

#. In the dialog box that is displayed, confirm that the service is in off-peak hours, and click **Yes**. A message will be displayed in the upper right corner, indicating that the switchback request is being processed.

#. Check the cluster status. During the switchback, the cluster status is **Switching back**. After the switchback, the cluster status will change to **Available**.

.. |image1| image:: /_static/images/en-us_image_0000002168066048.png
