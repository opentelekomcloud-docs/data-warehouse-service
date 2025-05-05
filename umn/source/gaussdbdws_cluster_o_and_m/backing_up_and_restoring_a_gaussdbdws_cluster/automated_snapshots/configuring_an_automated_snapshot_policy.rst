:original_name: dws_01_0089.html

.. _dws_01_0089:

Configuring an Automated Snapshot Policy
========================================

You can choose the type of snapshot you need and configure either one or three automated snapshot policies at the cluster level, as well as one or twenty snapshot policies at the schema level for a cluster.

After an automated snapshot policy is enabled, the system automatically creates snapshots based on the time, period, and snapshot type you configured.

Procedure
---------

#. Log in to the GaussDB(DWS) console.

#. Choose **Dedicated Clusters** > **Clusters** in the navigation pane.

#. In the cluster list, click the name of the target cluster. The **Cluster Information** page is displayed.

#. Click **Snapshots** and click **Policy List**. All policies of the current cluster are displayed on the **Policy List** page. Toggle on **Snapshot Policy**.

#. (Optional) Click **Automated Snapshot** to enable the snapshot policy.

   -  |image1| indicates that the policy is enabled (default). The default retention period is seven days.
   -  |image2| indicates that the policy is disabled. Once disabled, snapshots will not be automatically created.

#. Set the retention mode and the backup device used by the current cluster for automated snapshots. For more information, see :ref:`Table 1 <en-us_topic_0000002168065724__en-us_topic_0000001360169333_en-us_topic_0000001231278872_table11860173413712>`.

   .. _en-us_topic_0000002168065724__en-us_topic_0000001360169333_en-us_topic_0000001231278872_table11860173413712:

   .. table:: **Table 1** Automated snapshot parameters

      +--------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                            | Description                                                                                                                                                                                                       |
      +======================================+===================================================================================================================================================================================================================+
      | Retention Days                       | Retention days of the snapshots that are automatically created. The value ranges from 1 to 31 days.                                                                                                               |
      |                                      |                                                                                                                                                                                                                   |
      |                                      | .. note::                                                                                                                                                                                                         |
      |                                      |                                                                                                                                                                                                                   |
      |                                      |    Snapshots that are automatically created cannot be deleted manually. The system automatically deletes these snapshots when their retention duration exceeds the threshold.                                     |
      +--------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Backup Device                        | Select **OBS** or **NFS** from the drop-down list.                                                                                                                                                                |
      +--------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | NFS Backup File System Address (NFS) | NFS shared IP address. Enter the IP address of the SFS shared path. After the mounting is successful, a mount directory is created in the **/var/chroot/nfsbackup** directory of the cluster instance by default. |
      +--------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. After automated snapshot is enabled, you can configure its parameters. For more information, see :ref:`Table 2 <en-us_topic_0000002168065724__en-us_topic_0000001360169333_en-us_topic_0000001231278872_table1355651818416>`.

   -  If you choose a cluster-level or schema-level full snapshot, you can either create a one-time snapshot or set up periodic snapshots. For schema-level full snapshots, you need to select the database associated with the schema. You can back up one or up to 20 schemas at a time.

      -  To schedule periodic full snapshots, you can specify a day of the week or a specific date, and choose a specific time for the snapshot to be triggered.

         .. warning::

            -  Exercise caution when selecting the 29th, 30th, or 31st as end dates for a month, as this may result in missing data. The specific policy and execution are subject to the actual month and date.
            -  The snapshot policy time is the local time.

      -  Set a cluster-level or schema-level one-time full snapshot policy. Specify the desired date and time for the snapshot to be triggered.

   -  Incremental cluster-level or schema-level snapshots can be set only to **Periodic**.

      You can configure an incremental periodic snapshot policy at the cluster or schema level. Specify a target week or date, along with the trigger time, start time, and interval for the snapshot.

   .. _en-us_topic_0000002168065724__en-us_topic_0000001360169333_en-us_topic_0000001231278872_table1355651818416:

   .. table:: **Table 2** Snapshot policy parameters

      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                                                                                                                                                                                                                                                                                                      |
      +===================================+==================================================================================================================================================================================================================================================================================================================================================================================================+
      | Snapshot Policy Name              | The policy name must be unique, consist of 4 to 92 characters, and start with a letter. It is case-insensitive and can contain only letters, digits, hyphens (-), and underscores (_).                                                                                                                                                                                                           |
      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Snapshot Level                    | The available options are **Cluster** and **Schema**.                                                                                                                                                                                                                                                                                                                                            |
      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Snapshot Type                     | You can choose either full or incremental snapshots.                                                                                                                                                                                                                                                                                                                                             |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                  |
      |                                   | .. note::                                                                                                                                                                                                                                                                                                                                                                                        |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                  |
      |                                   |    -  A full snapshot is created after every fifteen incremental snapshots are created.                                                                                                                                                                                                                                                                                                          |
      |                                   |    -  Incremental snapshot restoration is based on full snapshots. Incremental snapshots are used to restore all data to the time point when they were created.                                                                                                                                                                                                                                  |
      |                                   |    -  An incremental snapshot records the changes made after the previous snapshot was created. A full snapshot backs up the data of an entire cluster. It takes a short time to create an incremental snapshot, and a long time to create a full snapshot. When restoring a snapshot to a new cluster, GaussDB(DWS) uses all snapshots between the latest full backup and the current snapshot. |
      |                                   |    -  The following versions support automated incremental policies at the schema level:                                                                                                                                                                                                                                                                                                         |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                  |
      |                                   |       -  9.1.0.100 or later                                                                                                                                                                                                                                                                                                                                                                      |
      |                                   |       -  8.3.0.110 or later 8.3.0.\ *xxx* cluster versions                                                                                                                                                                                                                                                                                                                                       |
      |                                   |       -  8.2.1.230 or later 8.2.1.2\ *xx* versions                                                                                                                                                                                                                                                                                                                                               |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                  |
      |                                   |    -  Schema-level policies, the policy level, database, and schema attributes cannot be modified after configuration.                                                                                                                                                                                                                                                                           |
      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Policy                            | You can choose either periodic or one-time snapshots.                                                                                                                                                                                                                                                                                                                                            |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                  |
      |                                   | .. note::                                                                                                                                                                                                                                                                                                                                                                                        |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                  |
      |                                   |    **One-time** can be selected only for full snapshots.                                                                                                                                                                                                                                                                                                                                         |
      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | One-time                          | You can create a full snapshot at a specified time in the future. The local time is used.                                                                                                                                                                                                                                                                                                        |
      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Periodic Policy Configurations    | You can create automated snapshots on a daily, weekly, or monthly basis:                                                                                                                                                                                                                                                                                                                         |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                  |
      |                                   | -  **Days**: Specify days for every week or every month. **Weekly** and **Monthly** cannot be selected at the same time. For **Monthly**, the specified days are applicable only to months that contain the dates. For example, if you select **29**, no automated snapshot will be created on February, 2022.                                                                                   |
      |                                   | -  **Time**: Specify the exact time on the selected days. For incremental snapshots, you can specify the start time and interval. The interval for cluster-level snapshots ranges from 4 to 24 hours, while for schema-level snapshots, it ranges from 1 to 24 hours. This setting determines the frequency at which snapshots will be taken.                                                    |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                  |
      |                                   | .. important::                                                                                                                                                                                                                                                                                                                                                                                   |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                  |
      |                                   |    NOTICE:                                                                                                                                                                                                                                                                                                                                                                                       |
      |                                   |    Incremental snapshots can be set only to **Periodic**, as shown in the first figure below.                                                                                                                                                                                                                                                                                                    |
      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Click **OK**.

#. (Optional) To modify an automated snapshot policy, click **Modify** in the **Operation** column.

#. (Optional) To preview a policy, click **Preview Policy**. The next seven snapshots of the cluster will be displayed. If no full snapshot policy is configured for the cluster, the default policy is used, that is, a full snapshot is taken after every 15 incremental snapshots.

   .. important::

      Implementation of the same policy varies according to operations in the cluster. For example:

      -  If the automated snapshot function is disabled, the configured snapshot policy will not appear on the snapshot preview page.
      -  If the fine-grained snapshot function is disabled in the snapshot list, the schema-level automatic policy will not be shown on the preview page.
      -  The policy preview time is for your reference only. The cluster triggers a snapshot within one hour before and after the preset time.
      -  The next automated snapshots after cluster scale-out, upgrade, resize, and media modification are full snapshots by default.
      -  If a periodic policy is used for a cluster, no automatic backup is allowed within 4 hours after the last automated snapshot is complete.
      -  In the event of conflicting triggering times between multiple policies, the following priority order applies: cluster-level > schema-level, one-time > periodic, and full > incremental.
      -  You can use any backup, full or incremental, to restore the full data of a resource.
      -  When both the schema-level automatic incremental policy and the cluster-level full or incremental policies are in use, incremental schema snapshots will automatically be converted to full snapshots when certain conditions are met.

.. |image1| image:: /_static/images/en-us_image_0000002168066248.png
.. |image2| image:: /_static/images/en-us_image_0000002203427229.png
