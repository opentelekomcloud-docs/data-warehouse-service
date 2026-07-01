:original_name: dws_01_0828.html

.. _dws_01_0828:

Disk Scaling
============

Context
-------

As customer services evolve, disk space often becomes the initial bottleneck. In scenarios where other resources are ample, the conventional scale-out process is not only time-consuming but also resource-inefficient. Disk capacity expansion can quickly increase storage without service interruption. You can expand the disk capacity when no other services are running. If the disk space is insufficient after the expansion, you can continue to expand the disk capacity. If the expansion fails, you can expand the disk capacity again.

.. note::

   -  Disk capacity can be expanded only if the cluster is in **Available**, **To be restarted**, **Read-only**, **Unbalanced**, **Node fault**, or **Unavailable** state.

Precautions
-----------

-  Hot storage disks cannot be scaled down.
-  Scale up hot data storage during off-peak hours.
-  If the cluster is in the read-only state, a message will be displayed after you click **Change Disk Capacity**. After you start expansion, wait until it is completed and the cluster changes to the available state.

Procedure
---------

#. Log in to the DWS console.
#. Choose **Dedicated Clusters** > **Clusters**. All clusters are displayed by default.
#. In the **Operation** column of the target cluster, choose **More** > **Change Specifications** > **Change disk capacity** and click **OK**.
#. Select the appropriate storage space based on the storage step of the corresponding flavor. The step refers to the interval for increasing or decreasing storage space. Click **Resize Cluster Now**.
#. Confirm the settings and click **Submit**.
#. Return to the cluster list and check the disk capacity expansion progress.
