:original_name: dws_01_0029.html

.. _dws_01_0029:

Restoring a DWS Cluster Snapshot
================================

During the restoration, DWS uses all the snapshots between the latest full backup and the current snapshot to restore the cluster, preventing data loss.

If the retention period of an incremental snapshot exceeds the maximum retention period, DWS does not delete the snapshot immediately. Instead, DWS retains it until the next full snapshot is completed, when the deletion of the snapshot will not hinder incremental data backup and restoration.

The following methods are supported:

-  :ref:`Restoring a Cluster Snapshot to a New Cluster <en-us_topic_0000002329649418__en-us_topic_0000001423159701_en-us_topic_0000001360289709_en-us_topic_0000001180320217_section3487218143910>`: You must restore a snapshot to a new cluster when you want to check point-in-time snapshot data of the cluster. If you restore a snapshot to a new cluster, DWS creates a cluster based on the cluster information recorded in the snapshot, and then restores data from the snapshot. When a snapshot is restored to a new cluster, the restoration time is determined by the amount of data backed up by the snapshot. If a snapshot contains a large amount of data, the restoration will be slow. A small snapshot can be quickly restored.
-  :ref:`Restoring a Cluster Snapshot to the Current Cluster <en-us_topic_0000002329649418__en-us_topic_0000001423159701_section4992195213416>`: If you restore a snapshot to the original cluster, DWS clears the existing data in the cluster, and then restores the database information from the snapshot to the cluster. This function is used when a cluster is faulty or data needs to be rolled back to a specified snapshot version.

Notes and Constraints
---------------------

-  If the backup device is NFS, a cluster snapshot can be restored to the current cluster but not to a new cluster.

-  By default, the new cluster created during restoration using a snapshot has the same specifications and node quantity as the original cluster.
-  Snapshots can be restored to the current cluster only when the cluster version is 8.1.3.200 or later.
-  Logical clusters and resource pools cannot be restored to a new cluster or the current cluster.

-  The resources required for restoring data to a new cluster do not exceed your available resource quota.
-  The snapshot used for restoration must be in the **Available** state.

.. _en-us_topic_0000002329649418__en-us_topic_0000001423159701_en-us_topic_0000001360289709_en-us_topic_0000001180320217_section3487218143910:

Restoring a Cluster Snapshot to a New Cluster
---------------------------------------------

#. Log in to the DWS console.

#. In the navigation pane on the left, choose **Dedicated Clusters** > **Management** > **Snapshots**. Alternatively, in the cluster list, click the name of the target cluster to switch to the **Cluster Information** page, and then choose **Snapshots** from the navigation pane. All snapshots are displayed by default.

#. In the **Operation** column of a snapshot, click **Restore**.

#. On the **Restore Snapshot** page, select **New cluster** and **Cluster level**, and configure the parameters of the new cluster.

   You can modify cluster parameters. For details, see :ref:`Table 1 <en-us_topic_0000002329649418__en-us_topic_0000001423159701_en-us_topic_0000001360289709_en-us_topic_0000001180320217_table2991343171911>`. The values of other parameters are the same as those of the original cluster by default. For details about the parameter settings, see :ref:`Creating a Dedicated DWS Cluster <dws_01_0221>`.

   .. _en-us_topic_0000002329649418__en-us_topic_0000001423159701_en-us_topic_0000001360289709_en-us_topic_0000001180320217_table2991343171911:

   .. table:: **Table 1** Parameters for the new cluster

      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------+
      | Configuration Type                | Configuration Name                                                                                                       |
      +===================================+==========================================================================================================================+
      | Basic settings                    | Region, AZ, node flavor, cluster name, database port, VPC, subnet, security group, public access, and enterprise project |
      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------+
      | Advanced settings                 | If **Custom** is selected, configure the following parameters:                                                           |
      |                                   |                                                                                                                          |
      |                                   | -  Backup devices: Select OBS or NFS from the drop-down list.                                                            |
      |                                   | -  Tag: a key-value pair used to identify a cluster. For details about tags, see :ref:`Overview <dws_01_0104>`.          |
      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------+

#. Click **Next: Confirm Settings**.

#. Click **Restore Now** to restore the snapshot to the new cluster. Restoring data to a new cluster does not affect the services running in the original cluster.

   When the status of the new cluster changes to **Available**, the snapshot is restored.

   After the snapshot is restored, the private network address and EIP (if **EIP** is set to **Automatically assign**) are automatically assigned.

.. _en-us_topic_0000002329649418__en-us_topic_0000001423159701_section4992195213416:

Restoring a Cluster Snapshot to the Current Cluster
---------------------------------------------------

#. Log in to the DWS console.
#. In the navigation pane on the left, choose **Dedicated Clusters** > **Management** > **Snapshots**. Alternatively, in the cluster list, click the name of the target cluster to switch to the **Cluster Information** page, and then choose **Snapshots** from the navigation pane. All snapshots are displayed by default.
#. In the **Operation** column of a snapshot, click **Restore**.
#. On the **Restore Snapshot** page, select **Current cluster**. If you use a snapshot to restore data to the original cluster, the cluster will be unavailable during the restoration.

Viewing Restoration Details
---------------------------

#. Log in to the DWS console.
#. Choose **Dedicated Clusters** > **Clusters**. By default, all clusters of the user are displayed.
#. In the cluster list, if the cluster status is **Restoring**, click **View Details**.
#. You can view the snapshot restoration progress of the cluster on the task details page. The estimated duration in the task details is for reference only. The actual duration depends on the current data volume.
#. In the restore phase, click **View** to view the kernel restoration process. Note that there may be a time gap between the task time displayed in the task details area and the actual kernel execution time due to task scheduling and restart.
