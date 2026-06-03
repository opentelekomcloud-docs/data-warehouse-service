:original_name: dws_01_0089.html

.. _dws_01_0089:

Configuring and Managing DWS Automated Snapshots
================================================

After a cluster is created, automated snapshots are enabled by default. DWS automatically creates snapshots based on the specified time and interval. The automated snapshot created for the first time is a full backup (base version), and then the system creates full backups at a specified interval. Incremental backups are generated between two full backups. The incremental backup records the changes compared with the previous backup. (By default, an incremental backup is performed every eight hours and a full backup is performed every week.)

You can configure one or more automated snapshot policies for the cluster as required. For details, see :ref:`Configuring an Automated Snapshot Policy <en-us_topic_0000002329649414__en-us_topic_0000001423119273_section069713349183>`. If no full backup policy is configured, a full backup is performed every 15 incremental backups.

:ref:`Figure 1 <en-us_topic_0000002329649414__en-us_topic_0000001423119273_en-us_topic_0000001360289561_en-us_topic_0000001276484833_fig1499915514436>` shows the snapshot backup process. The retention period of an automated snapshot can be set to 1 to 31 days. The default retention period is 7 days. The system deletes the snapshot at the end of the retention period. The retention period sets the duration for which users can access snapshots. If an incremental snapshot does not expire, both the incremental and full snapshots will be kept available instead of being deleted right away. Expired snapshots are hidden and can no longer be viewed by users. After all incremental snapshots expire, the hidden snapshots are physically deleted. If you want to keep an automated snapshot for a longer period, you can create a copy of it as a manual snapshot. The automated snapshot is retained until the end of the retention period, whereas the corresponding manual snapshot is retained until you manually delete it. For details about how to copy an automated snapshot, see :ref:`Copying an Automated Snapshot <en-us_topic_0000002329649414__en-us_topic_0000001423119273_section10944639153615>`.

.. _en-us_topic_0000002329649414__en-us_topic_0000001423119273_en-us_topic_0000001360289561_en-us_topic_0000001276484833_fig1499915514436:

.. figure:: /_static/images/en-us_image_0000002363847465.png
   :alt: **Figure 1** Snapshot backup process

   **Figure 1** Snapshot backup process

Notes and Constraints
---------------------

-  If the automated snapshot function is disabled, the configured snapshot policy will not appear on the snapshot preview page.
-  If the fine-grained snapshot function is disabled in the snapshot list, the schema-level automatic policy will not be shown on the preview page.
-  The policy preview time is for your reference only. The cluster triggers a snapshot within one hour before and after the preset time.
-  If you scale out or upgrade a cluster, modify the snapshot storage media, perform in-place restoration, or enable/disable fine-grained snapshots, the next automated snapshot will be a full snapshot by default.
-  If a periodic policy is used for a cluster, no automatic backup is allowed within 4 hours after the last automated snapshot is complete.
-  In the event of conflicting triggering times between multiple policies, the following priority order applies: cluster-level > schema-level, one-time > periodic, and full > incremental.
-  You can use any backup, full or incremental, to restore the full data of a resource.
-  When both the schema-level automatic incremental policy and the cluster-level full or incremental policies are in use, incremental schema snapshots will automatically be converted to full snapshots when certain conditions are met.

.. _en-us_topic_0000002329649414__en-us_topic_0000001423119273_section069713349183:

Configuring an Automated Snapshot Policy
----------------------------------------

You can choose the type of snapshot you need and configure either one or 10 automated snapshot policies at the cluster level, as well as one or 50 snapshot policies at the schema level for a cluster.

After an automated snapshot policy is enabled, the system automatically creates snapshots based on the time, period, and snapshot type you configured.

#. Log in to the DWS console.

#. Choose **Dedicated Clusters** > **Clusters** in the navigation pane.

#. In the cluster list, click the name of the target cluster. The **Cluster Information** page is displayed.

#. Click the **Snapshots** tab page and click **Policy List**. All policies of the current cluster are displayed on the **Policy List** page. Toggle on **Snapshot Policy**.

#. (Optional) Click **Automated Snapshot** to enable the snapshot policy.

   -  |image1| indicates that the policy is enabled (default). The default retention period is seven days.
   -  |image2| indicates that the policy is disabled. Once disabled, snapshots will not be automatically created.

#. Set the retention mode and the backup device used by the current cluster for automated snapshots. For more information, see :ref:`Table 1 <en-us_topic_0000002329649414__en-us_topic_0000001423119273_en-us_topic_0000001360169333_en-us_topic_0000001231278872_table11860173413712>`.

   .. _en-us_topic_0000002329649414__en-us_topic_0000001423119273_en-us_topic_0000001360169333_en-us_topic_0000001231278872_table11860173413712:

   .. table:: **Table 1** Automated snapshot parameters

      +--------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                            | Description                                                                                                                                                                                     |
      +======================================+=================================================================================================================================================================================================+
      | Retention Days                       | Retention days of the snapshots that are automatically created. The value ranges from 1 to 31 days.                                                                                             |
      |                                      |                                                                                                                                                                                                 |
      |                                      | .. note::                                                                                                                                                                                       |
      |                                      |                                                                                                                                                                                                 |
      |                                      |    Snapshots that are automatically created cannot be deleted manually. The system automatically deletes these snapshots when their retention duration exceeds the threshold.                   |
      +--------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Backup Device                        | Select **OBS** or **NFS** from the drop-down list.                                                                                                                                              |
      +--------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | NFS Backup File System Address (NFS) | NFS shared IP address. To mount the SFS shared path, enter its IP address. If successful, a mount directory will be created in the **/var/chroot/nfsbackup** directory of the cluster instance. |
      +--------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Click **Add Snapshot Policy** and set the parameters. :ref:`Table 2 <en-us_topic_0000002329649414__en-us_topic_0000001423119273_en-us_topic_0000001360169333_en-us_topic_0000001231278872_table1355651818416>` describes the parameters.

   -  If you choose a full snapshot, you can either create a one-time snapshot or set up periodic snapshots. For schema-level full snapshots, you need to select the database associated with the schema. You can back up one or up to 50 schemas at a time.
   -  Incremental snapshots can be set only to **Periodic**. You can configure a periodic snapshot policy. Specify a target week or date, along with the trigger time, start time, and interval for the snapshot.

   .. _en-us_topic_0000002329649414__en-us_topic_0000001423119273_en-us_topic_0000001360169333_en-us_topic_0000001231278872_table1355651818416:

   .. table:: **Table 2** Snapshot policy parameters

      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Definition                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
      +===================================+=====================================================================================================================================================================================================================================================================================================================================================================================================================================================================================+
      | Snapshot Policy Name              | The policy name must be unique, consist of 4 to 92 characters, and start with a letter. It is case-insensitive and can contain only letters, digits, hyphens (-), and underscores (_).                                                                                                                                                                                                                                                                                              |
      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Snapshot Level                    | The available options are **Cluster** and **Schema**.                                                                                                                                                                                                                                                                                                                                                                                                                               |
      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Snapshot Type                     | You can select **Full** or **Incremental**.                                                                                                                                                                                                                                                                                                                                                                                                                                         |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
      |                                   | -  A full snapshot is created after every fifteen incremental snapshots are created.                                                                                                                                                                                                                                                                                                                                                                                                |
      |                                   | -  Incremental snapshot restoration is based on full snapshots. Incremental snapshots are used to restore all data to the time point when they were created.                                                                                                                                                                                                                                                                                                                        |
      |                                   | -  An incremental snapshot records the changes made after the previous snapshot was created. A full snapshot backs up the data of an entire cluster. It takes a short time to create an incremental snapshot, and a long time to create a full snapshot. When restoring a snapshot to a new cluster, DWS uses all snapshots between the latest full backup and the current snapshot.                                                                                                |
      |                                   | -  The following versions support automated incremental policies at the schema level:                                                                                                                                                                                                                                                                                                                                                                                               |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
      |                                   |    -  9.1.0.100 or later                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
      |                                   |    -  8.3.0.110 or later 8.3.0.\ *xxx*                                                                                                                                                                                                                                                                                                                                                                                                                                              |
      |                                   |    -  8.2.1.230 or later 8.2.1.2\ *xx*                                                                                                                                                                                                                                                                                                                                                                                                                                              |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
      |                                   | -  Schema-level policies, the policy level, database, and schema attributes cannot be modified after configuration.                                                                                                                                                                                                                                                                                                                                                                 |
      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Snapshot Policy                   | You can select **Periodic** or **One-off**. You can select **One-off** only when you have selected **Full** for **Snapshot Type**.                                                                                                                                                                                                                                                                                                                                                  |
      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | One-time Snapshot Settings        | You can create a full snapshot at a specified time in the future. The local time is used.                                                                                                                                                                                                                                                                                                                                                                                           |
      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Periodic Snapshot Settings        | You can set a periodic snapshot triggering policy and set a local time.                                                                                                                                                                                                                                                                                                                                                                                                             |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
      |                                   | -  **Days**: Specify days for every week or every month. **Specified day in week** and **Specified date** cannot be selected at the same time. If the current month does not contain the selected date, the operation will be postponed to the next month. (Exercise caution when selecting the 29th, 30th, or 31st as the end date for a month, as this may result in missing data. The specific policy and execution are subject to the actual month and date.)                   |
      |                                   | -  **Time**: Specify the exact time on the selected days. For incremental snapshots, you can specify the start time and interval. The interval for cluster-level snapshots ranges from 4 to 24 hours, while for schema-level snapshots, it ranges from 1 to 24 hours. This setting determines the frequency at which snapshots will be taken. If the incremental data is large and the backup period is long, the backup will be slow. In this case, increase the backup frequency. |
      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Click **OK**.

#. (Optional) To modify an automated snapshot policy, click **Modify** in the **Operation** column.

#. (Optional) Click the **Snapshots** tab. Click |image3| next to the target snapshot to view its policy name.

#. (Optional) To preview a policy, click **Preview Policy**. The next seven snapshots of the cluster will be displayed. If no full snapshot policy is configured for the cluster, the default policy is used, that is, a full snapshot is taken after every 15 incremental snapshots.

.. _en-us_topic_0000002329649414__en-us_topic_0000001423119273_section10944639153615:

Copying an Automated Snapshot
-----------------------------

This section describes how to copy snapshots that are automatically created for long-term retention.

#. Log in to the DWS console.

#. In the navigation pane on the left, choose **Dedicated Clusters** > **Management** > **Snapshots**. All snapshots are displayed by default. You can copy the snapshots that were automatically created.

#. In the **Operation** column of the snapshot that you want to copy, choose **More** > **Copy**.

   .. table:: **Table 3** Parameters for copying a snapshot

      +----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter            | Description                                                                                                                                                                                              |
      +======================+==========================================================================================================================================================================================================+
      | Source Snapshot Name | Source snapshot to be copied                                                                                                                                                                             |
      +----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | New Snapshot Name    | Name of the new snapshot. The snapshot name must be 4 to 64 characters in length and start with a letter. It is case-insensitive and can contain only letters, digits, hyphens (-), and underscores (_). |
      +----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Snapshot Description | Enter the snapshot description. This parameter is optional. The snapshot description must be 0 to 256 characters long and cannot include special characters like !<>'=&"                                 |
      +----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Click **OK**. The system starts to copy the snapshot for the cluster.

   The system displays a message indicating that the snapshot is successfully copied and delivered. After the snapshot is copied, the status of the copied snapshot is **Available**.

Stopping an Automated Snapshot
------------------------------

#. Log in to the DWS console.
#. In the navigation pane on the left, choose **Dedicated Clusters** > **Management** > **Snapshots**. Alternatively, in the cluster list, click the name of the target cluster to switch to the **Cluster Information** page, and then choose **Snapshots** from the navigation pane. All snapshots are displayed by default.
#. In the **Operation** column of a snapshot that is being created, and click **Cancel Snapshot Creation**.
#. In the dialog box that is displayed, click **Yes** to stop the snapshot. The snapshot state will change to **Unavailable**.

   -  This feature is supported only in cluster version 8.1.3.200 and later.
   -  If the snapshot creation is about to complete, the command for stopping the snapshot will not take effect, and the snapshot will be created.
   -  Only the snapshots in the **Creating** state can be stopped. A snapshot creation task that just started or is about to complete cannot be stopped.

Deleting an Automated Snapshot
------------------------------

Only DWS can delete automated snapshots; you cannot delete them manually. When the retention period of an automated snapshot ends or a cluster is deleted, DWS deletes the automated snapshot.

To help users restore a cluster deleted by mistake, DWS provides the following policies (supported only in clusters of version 8.2.0 and later):

-  If the latest snapshot is an automated snapshot, it will be retained for one day.
-  If the latest snapshot is a manual snapshot, the automated snapshot of the cluster will be deleted.

.. |image1| image:: /_static/images/en-us_image_0000002329769164.png
.. |image2| image:: /_static/images/en-us_image_0000002329928968.png
.. |image3| image:: /_static/images/en-us_image_0000002363767369.png
