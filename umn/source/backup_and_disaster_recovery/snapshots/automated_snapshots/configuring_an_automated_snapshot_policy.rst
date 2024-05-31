:original_name: dws_01_0089.html

.. _dws_01_0089:

Configuring an Automated Snapshot Policy
========================================

You can select a snapshot type and set one or more automated snapshot policies for a cluster. After an automated snapshot policy is enabled, the system automatically creates snapshots based on the time, period, and snapshot type you configured.

Procedure
---------

#. Log in to the GaussDB(DWS) management console.

#. In the navigation pane on the left, choose **Clusters** > **Dedicated Clusters**.

#. In the cluster list, click the name of the target cluster. The **Cluster Information** page is displayed.

#. Click the **Snapshots** tab page and click **Policy List**. All policies of the current cluster are displayed on the **Policy List** page. Toggle on **Snapshot Policy**.

   -  |image1| indicates that the policy is enabled (default). The default retention period is three days.
   -  |image2| indicates that the automatic snapshot function is disabled.


   .. figure:: /_static/images/en-us_image_0000001711439524.png
      :alt: **Figure 1** Policy list

      **Figure 1** Policy list

#. After this function is enabled, you can set the retention mode for automated snapshots. For more information, see :ref:`Table 1 <en-us_topic_0000001658895258__en-us_topic_0000001423119261_en-us_topic_0000001360169333_en-us_topic_0000001231278872_table11860173413712>`.

   .. _en-us_topic_0000001658895258__en-us_topic_0000001423119261_en-us_topic_0000001360169333_en-us_topic_0000001231278872_table11860173413712:

   .. table:: **Table 1** Automated snapshot parameters

      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                                                                                   |
      +===================================+===============================================================================================================================================================================+
      | Retention Days                    | Retention days of the snapshots that are automatically created. The value ranges from 1 to 31 days.                                                                           |
      |                                   |                                                                                                                                                                               |
      |                                   | .. note::                                                                                                                                                                     |
      |                                   |                                                                                                                                                                               |
      |                                   |    Snapshots that are automatically created cannot be deleted manually. The system automatically deletes these snapshots when their retention duration exceeds the threshold. |
      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. After automated snapshot is enabled, you can configure its parameters. For more information, see :ref:`Table 2 <en-us_topic_0000001658895258__en-us_topic_0000001423119261_en-us_topic_0000001360169333_en-us_topic_0000001231278872_table1355651818416>`.

   .. note::

      The snapshot creation time is UTC, which may be different from your local time.

   -  If the snapshot type is set to **Full**, you can choose either **Periodic** or **One-time**, as shown in the following figures.

      -  **Periodic**: Specify the days for every week/month and the exact time on the days.

         |image3|

         .. warning::

            Choosing the days in red (29th/30th/31st) may skip some monthly backups.

      -  **One-time**: Specify a day and the exact time on the day.

      |image4|

   -  Incremental snapshots can be set only to **Periodic**, as shown in the first figure below.

      When configuring a periodic incremental snapshot policy, you can specify the days for every week/month and the exact time on the days. You can also specify the start time and interval for the snapshots.

      |image5|

   .. _en-us_topic_0000001658895258__en-us_topic_0000001423119261_en-us_topic_0000001360169333_en-us_topic_0000001231278872_table1355651818416:

   .. table:: **Table 2** Snapshot policy parameters

      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                                                                                                                                                                                                                                                                                                      |
      +===================================+==================================================================================================================================================================================================================================================================================================================================================================================================+
      | Name                              | The policy name must be unique, consist of 4 to 92 characters, and start with a letter. It is case-insensitive and can contain only letters, digits, hyphens (-), and underscores (_).                                                                                                                                                                                                           |
      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Type                              | You can choose either full or incremental snapshots.                                                                                                                                                                                                                                                                                                                                             |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                  |
      |                                   | .. note::                                                                                                                                                                                                                                                                                                                                                                                        |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                  |
      |                                   |    -  A full snapshot is created after every fifteen incremental snapshots are created.                                                                                                                                                                                                                                                                                                          |
      |                                   |    -  Incremental snapshot restoration is based on full snapshots. Incremental snapshots are used to restore all data to the time point when they were created.                                                                                                                                                                                                                                  |
      |                                   |    -  An incremental snapshot records the changes made after the previous snapshot was created. A full snapshot backs up the data of an entire cluster. It takes a short time to create an incremental snapshot, and a long time to create a full snapshot. When restoring a snapshot to a new cluster, GaussDB(DWS) uses all snapshots between the latest full backup and the current snapshot. |
      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Policy                            | You can choose either periodic or one-time snapshots.                                                                                                                                                                                                                                                                                                                                            |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                  |
      |                                   | .. note::                                                                                                                                                                                                                                                                                                                                                                                        |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                  |
      |                                   |    **One-time** can be selected only for full snapshots.                                                                                                                                                                                                                                                                                                                                         |
      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | One-time                          | You can create a full snapshot at a specified time in the future. The UTC time is used.                                                                                                                                                                                                                                                                                                          |
      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Periodic Policy Configurations    | You can create automated snapshots on a daily, weekly, or monthly basis:                                                                                                                                                                                                                                                                                                                         |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                  |
      |                                   | -  **Days**: Specify days for every week or every month. **Weekly** and **Monthly** cannot be selected at the same time. For **Monthly**, the specified days are applicable only to months that contain the dates. For example, if you select **29**, no automated snapshot will be created on February, 2022.                                                                                   |
      |                                   | -  **Time**: Specify the exact time on the selected days. For incremental snapshots, you can specify the start time and interval. The interval can be 4 to 24 hours, indicating that a snapshot is created at an interval of 4 to 24 hours.                                                                                                                                                      |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                  |
      |                                   | .. important::                                                                                                                                                                                                                                                                                                                                                                                   |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                  |
      |                                   |    NOTICE:                                                                                                                                                                                                                                                                                                                                                                                       |
      |                                   |    If the incremental data is large and the execution period is long, the backup will be slow. In this case, increase the backup frequency.                                                                                                                                                                                                                                                      |
      +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Click **OK**.

   .. note::

      A maximum of three snapshot policies can be set for a cluster.

#. (Optional) To modify an automated snapshot policy, click **Modify** in the **Operation** column.

   |image6|

#. (Optional) To preview a policy, click **Preview Policy**. The next seven snapshots of the cluster will be displayed. If no full snapshot policy is configured for the cluster, the default policy is used, that is, a full snapshot is taken after every 14 incremental snapshots.

   |image7|

   .. important::

      Implementation of the same policy varies according to operations in the cluster. For example:

      -  The policy preview time is for your reference only. The cluster triggers a snapshot within one hour before and after the preset time.
      -  The next automated snapshots after cluster scale-out, upgrade, resize, and media modification are full snapshots by default.
      -  If a periodic policy is used for a cluster, no automatic backup is allowed within 4 hours after the last automated snapshot is complete.
      -  If the time for triggering snapshots of multiple policies conflicts, the priorities of the policies are as follows: one-time > periodic > full > incremental.
      -  You can use any backup, full or incremental, to restore the full data of a resource.

.. |image1| image:: /_static/images/en-us_image_0000001759358549.png
.. |image2| image:: /_static/images/en-us_image_0000001711599016.png
.. |image3| image:: /_static/images/en-us_image_0000001711599064.png
.. |image4| image:: /_static/images/en-us_image_0000001711439584.png
.. |image5| image:: /_static/images/en-us_image_0000001759518473.png
.. |image6| image:: /_static/images/en-us_image_0000001759358617.png
.. |image7| image:: /_static/images/en-us_image_0000001711599084.png
