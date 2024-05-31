:original_name: dws_01_0825.html

.. _dws_01_0825:

Changing All Specifications
===========================

If you want to change your cluster topology or capacity but the **Change node flavor** option is grayed out, you can select **Change all specifications** to increase or decrease the nodes and their capacities on the GaussDB(DWS) console. First, you need to configure the new specifications you want, and a cluster with these specifications will be created. Then, data will be migrated from the old cluster to the new one. In case you need to restore data, a full snapshot will be taken for the old cluster, and the old cluster will be retained for a period of time.

.. note::

   -  To use this feature, contact technical support engineers to upgrade your version first.

   -  A cluster can have up to 240 nodes. The old and new clusters can have up to 480 nodes in total.
   -  The **Change all specifications** option do not support logical clusters.

Impact of Changing All Specifications
-------------------------------------

-  Before the change, you need to exit the client connections that have created temporary tables, because temporary tables created before or during the change will become invalid and operations performed on these temporary tables will fail. The temporary tables created after the change are not affected.
-  The change involves data redistribution, during which the cluster is read-only.
-  After the specifications are changed, the private IP address changes, which should be updated for connection.
-  After the specifications are changed, the domain name remains unchanged, and the IP address bound to the domain name is switched. During the switchover, the connection is interrupted for a short period of time. Therefore, avoid writing service statements in the switchover. If the service side uses a domain name for connection, you need to update the cache information corresponding to the domain name to prevent connection failure after the change.
-  If an ELB is bound to the cluster, the connection address on the service side remains unchanged after the specifications are changed, while the internal server address of the ELB is changed to the new connection address.
-  In case you need to restore data, a full snapshot will be taken for the old cluster (on condition that your cluster support snapshot creation). You can check it in the snapshot list and manually delete it if it is no longer necessary.
-  During the change, the cluster is read-only, affecting intelligent O&M tasks. You are advised to start these tasks after the change or pause them before the change.

Prerequisites
-------------

-  The cluster to be changed is in the **Available**, **Read-only**, or **Unbalanced** state.
-  The number of nodes after resizing must be smaller than or equals the available node quotas, or the resizing fill fail.
-  The total capacity of the new cluster after the change must be at least 1.2 times greater than the used capacity of the old cluster.


Changing All Specifications
---------------------------

#. Log in to the GaussDB(DWS) management console.
#. Choose **Clusters** > **Dedicated Cluster**. All clusters are displayed by default.
#. In the row of a cluster, choose **More** > **Change Specifications** in the **Operation** column and click **Change all specifications**.

   -  For the **Node Flavor** parameter, select a flavor.

      .. note::

         The VPC, subnet, and security group of the new cluster are the same as those of the original cluster.

   -  For the **Set to** parameter, set the number of nodes you want for the new cluster.

#. (Optional) If the cluster storage can be modified, you can set the storage type and the available storage for each node.
#. Read the nodes and select **Confirmed**. Click **Resize Cluster Now**.
#. Click **Submit**.

   -  After the change request is submitted, **Task Status** of the cluster changes to **Changing all specifications**. The process will take several minutes.
   -  During the change, the cluster automatically restarts, and **Cluster Status** is **Unavailable** for a period of time. After the restart is complete, **Cluster Status** changes to **Available**. Data is redistributed during resizing. During the redistribution, **Cluster Status** is **Read-only**.
   -  The resizing succeeds only when **Cluster Status** is **Available** and the **Change all specifications** task in **Task Information** is complete. Then the cluster begins providing services.
   -  If **Change all specifications failed** is displayed, the cluster failed to be changed.
   -  If change fails, and a message requiring retry is displayed when you click **Resize**, the failure is probably caused by abnormal cluster status or network problems. In this case, contact technical support to troubleshoot the problem and try again.
