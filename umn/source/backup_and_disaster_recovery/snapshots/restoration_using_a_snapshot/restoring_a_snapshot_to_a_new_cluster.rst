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
   -  If cold and hot tables are used, snapshots cannot be used to restore cold data to a new cluster.
   -  Fine-grained restoration does not support tables in absolute or relative tablespace.
   -  Logical clusters and resource pools cannot be restored to a new cluster.

Prerequisites
-------------

-  The resources required for restoring data to a new cluster do not exceed your available resource quota.
-  The snapshot is in the **Available** state.

Procedure
---------

#. Log in to the GaussDB(DWS) management console.

#. In the navigation pane, choose **Snapshots**. All snapshots are displayed by default.

#. In the **Operation** column of a snapshot, click **Restore**.

   |image1|

#. On the **Restore Snapshot** page, configure the parameters of the new cluster, as shown in the following figure.

   a. Restore to a single-AZ cluster.

   You can modify cluster parameters. For details, see :ref:`Table 1 <en-us_topic_0000001659054582__en-us_topic_0000001423159701_en-us_topic_0000001360289709_en-us_topic_0000001180320217_table2991343171911>`. By default, other parameters are the same as those in the snapshot. For details, see :ref:`Table 2 <en-us_topic_0000001658895258__en-us_topic_0000001423119261_en-us_topic_0000001360169333_en-us_topic_0000001231278872_table1355651818416>`.

   .. _en-us_topic_0000001659054582__en-us_topic_0000001423159701_en-us_topic_0000001360289709_en-us_topic_0000001180320217_table2991343171911:

   .. table:: **Table 1** Parameters for the new cluster

      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------+
      | Category                          | Operation                                                                                                                |
      +===================================+==========================================================================================================================+
      | Basic settings                    | Region, AZ, node flavor, cluster name, database port, VPC, subnet, security group, public access, and enterprise project |
      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------+
      | Advanced settings                 | If **Custom** is selected, configure the following parameters:                                                           |
      |                                   |                                                                                                                          |
      |                                   | -  **Tag**: If encryption is enabled for the original cluster, you can configure a key name.                             |
      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------+

#. Click **Restore** to go to the confirmation page.

#. Click **Submit** to restore the snapshot to the new cluster.

   When the status of the new cluster changes to **Available**, the snapshot is restored.

   After the snapshot is restored, the private network address and EIP (if **EIP** is set to **Automatically assign**) are automatically assigned.

   .. note::

      If the number of requested nodes, vCPU (cores), or memory (GB) exceed the user's remaining quota, a warning dialog box is displayed, indicating that the quota is insufficient and displaying the detailed remaining quota and the current quota application. You can click **Increase quota** in the warning dialog box to submit a service ticket and apply for higher node quota.

.. |image1| image:: /_static/images/en-us_image_0000001759518489.png
