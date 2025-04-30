:original_name: dws_01_0029.html

.. _dws_01_0029:

Restoring a Snapshot to a New Cluster
=====================================

Scenario
--------

This section describes how to restore a snapshot to a new cluster when you want to check point-in-time snapshot data of the cluster.

When a snapshot is restored to a new cluster, the restoration time is determined by the amount of data backed up by the snapshot. If a snapshot contains a large amount of data, the restoration will be slow. A small snapshot can be quickly restored.

Automatic snapshots are incremental backups. When restoring a snapshot to a new cluster, GaussDB(DWS) uses all snapshots between the latest full backup and the current snapshot. You can set the backup frequency. If snapshots are backed up only once a week, the backup will be slow if the incremental data volume is large. You are advised to increase the backup frequency.

.. important::

   -  Currently, you can only use the snapshots stored in OBS to restore data to a new cluster.
   -  By default, the new cluster created during restoration has the same specifications and node quantity as the original cluster.
   -  Restoring data to a new cluster does not affect the services running in the original cluster.
   -  Fine-grained restoration does not support tables in absolute or relative tablespace.
   -  Logical clusters and resource pools cannot be restored to a new cluster.

Prerequisites
-------------

-  The resources required for restoring data to a new cluster do not exceed your available resource quota.
-  The snapshot is in the **Available** state.

Procedure
---------

#. Log in to the GaussDB(DWS) console.

#. Choose **Management** > **Snapshots**. Alternatively, in the cluster list, click the name of the target cluster to switch to the **Cluster Information** page. Then, click **Snapshots**. All snapshots are displayed by default.

#. In the **Operation** column of a snapshot, click **Restore**.

#. On the **Restore Snapshot** page, configure the parameters of the new cluster, as shown in the following figure.

   -  Restore to a single-AZ cluster.
   -  Restore to a multi-AZ cluster.

      .. note::

         -  Only clusters later than 8.2.0.100 can be restored to a multi-AZ cluster.
         -  This feature is only available for storage-compute coupled clusters.
         -  The number of AZs in the current region is greater than or equal to 3.
         -  The number of nodes and CNs must be a multiple of 3.
         -  DNs in the multi-AZ cluster must be less than or equal to 2.

   You can modify cluster parameters. For details, see :ref:`Table 1 <en-us_topic_0000002203426633__en-us_topic_0000001360289709_en-us_topic_0000001180320217_table2991343171911>`. By default, other parameters are the same as those in the snapshot. For details, see :ref:`Table 2 <en-us_topic_0000002168065724__en-us_topic_0000001360169333_en-us_topic_0000001231278872_table1355651818416>`.

   .. _en-us_topic_0000002203426633__en-us_topic_0000001360289709_en-us_topic_0000001180320217_table2991343171911:

   .. table:: **Table 1** Parameters for the new cluster

      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------+
      | Category                          | Operation                                                                                                                       |
      +===================================+=================================================================================================================================+
      | Basic settings                    | Configure the AZ, node flavor, cluster name, database port, VPC, subnet, security group, public access, and enterprise project. |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------+
      | Advanced settings                 | If **Custom** is selected, configure the following parameters:                                                                  |
      |                                   |                                                                                                                                 |
      |                                   | -  Backup devices: Select OBS or NFS from the drop-down list.                                                                   |
      |                                   | -  Label: a key-value pair used to identify a cluster. For details about labels, see :ref:`Overview <dws_01_0104>`.             |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------+

#. Click **Restore** to go to the confirmation page.

#. Click **Submit** to restore the snapshot to the new cluster.

   When the status of the new cluster changes to **Available**, the snapshot is restored.

   After the snapshot is restored, the private network address and EIP (if **EIP** is set to **Automatically assign**) are automatically assigned.

   .. note::

      If the number of requested nodes, vCPU (cores), or memory (GB) exceed the user's remaining quota, a warning dialog box is displayed, indicating that the quota is insufficient and displaying the detailed remaining quota and the current quota application. You can click **Increase quota** in the warning dialog box to submit a service ticket and apply for higher node quota. Once approved, we will update your resource quota accordingly and send you a notification.

Viewing Restoration Details
---------------------------

#. Log in to the GaussDB(DWS) console.
#. Choose **Dedicated Clusters** > **Clusters**. By default, all clusters of the user are displayed.
#. In the cluster list, if the cluster status is **Restoring**, click **View Details**.
#. You can view the snapshot restoration progress of the cluster on the task details page.

   .. note::

      -  The estimated duration in the task details is for reference only. The actual duration depends on the current data volume.
      -  In the restore phase, click **View** to view the kernel restoration process. Note that there may be a time gap between the task time displayed in the task details area and the actual kernel execution time due to task scheduling and restart.
