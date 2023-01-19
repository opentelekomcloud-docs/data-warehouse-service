:original_name: dws_01_0089.html

.. _dws_01_0089:

Configuring an Automated Snapshot Policy
========================================

You can select a snapshot type and set one or more automated snapshot policies for a cluster. After an automated snapshot policy is enabled, the system automatically creates snapshots based on the preset time, period, and snapshot type according to the policy.

Perform the following steps to configure an automated snapshot policy.

Procedure
---------

#. Log in to the GaussDB(DWS) management console.

#. In the navigation pane on the left, choose **Clusters**.

#. Click the name of a cluster.

#. Click the **Snapshots** tab page and click **Policy List**. All the policies of the current cluster are displayed on the **Policy List** page. Toggle on **Snapshot Policy**.

   -  |image1| indicates that the policy is enabled (default). The default retention period is three days.
   -  |image2| indicates that the policy is disabled. If this policy is disabled, historical automated snapshots will be automatically deleted.


   .. figure:: /_static/images/en-us_image_0000001276984957.png
      :alt: **Figure 1** Policy list

      **Figure 1** Policy list

#. After this function is enabled, you can set the retention mode for automated snapshots. For more information, see :ref:`Table 1 <en-us_topic_0000001231278872__table11860173413712>`.

   .. _en-us_topic_0000001231278872__table11860173413712:

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

#. After automated snapshot is enabled, you can configure its parameters. For more information, see :ref:`Table 2 <en-us_topic_0000001231278872__table1355651818416>`.

   .. note::

      The snapshot creation time is UTC, which may be different from your local time.

   -  If the snapshot type is set to **Full**, you can choose either **Periodic** or **One-time**, as shown in the following figures.

      -  **Periodic**: Specify the days for every week/month and the exact time on the days.

         |image3|

      -  **One-time**: Specify a day and the exact time on the day.

      |image4|

   -  Incremental snapshots can be set only to **Periodic**, as shown in the first figure below.

      -  When configuring a periodic incremental snapshot policy, you can specify the days for every week/month and the exact time on the days. You can also specify the start time and interval for the snapshots.

      |image5|

      |image6|

      .. warning::

         Choosing the days in red (29/30/31) may skip some monthly backups.

   .. _en-us_topic_0000001231278872__table1355651818416:

   .. table:: **Table 2** Parameter description

      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                                                                                                                                                                                                                    |
      +===================================+================================================================================================================================================================================================================================================================================================================+
      | Name                              | The policy name must be unique, consist of 4 to 92 characters, and start with a letter. It is case-insensitive and can contain only letters, digits, hyphens (-), and underscores (_).                                                                                                                         |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Type                              | You can choose either full or incremental snapshots.                                                                                                                                                                                                                                                           |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Policy                            | You can choose either periodic or one-time snapshots.                                                                                                                                                                                                                                                          |
      |                                   |                                                                                                                                                                                                                                                                                                                |
      |                                   | .. note::                                                                                                                                                                                                                                                                                                      |
      |                                   |                                                                                                                                                                                                                                                                                                                |
      |                                   |    You can select One-off Snapshot only when Snapshot Type is set to Full.                                                                                                                                                                                                                                     |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | One-time                          | You can create a full snapshot at a specified time in the future. The UTC time is used.                                                                                                                                                                                                                        |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Periodic Policy Configurations    | You can create automated snapshots on a daily, weekly, or monthly basis:                                                                                                                                                                                                                                       |
      |                                   |                                                                                                                                                                                                                                                                                                                |
      |                                   | -  **Days**: Specify days for every week or every month. **Weekly** and **Monthly** cannot be selected at the same time. For **Monthly**, the specified days are applicable only to months that contain the dates. For example, if you select **29**, no automated snapshot will be created on February, 2022. |
      |                                   | -  **Time**: Specify the exact time on the selected days. For incremental snapshots, you can specify the start time and interval. The interval can be 4 to 24 hours, indicating that a snapshot is created at an interval of 4 to 24 hours.                                                                    |
      |                                   |                                                                                                                                                                                                                                                                                                                |
      |                                   | .. important::                                                                                                                                                                                                                                                                                                 |
      |                                   |                                                                                                                                                                                                                                                                                                                |
      |                                   |    NOTICE:                                                                                                                                                                                                                                                                                                     |
      |                                   |    If the incremental data is large and the execution period is long, the backup will be slow. In this case, increase the backup frequency.                                                                                                                                                                    |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Click **OK**.

   .. note::

      A maximum of three snapshot policies can be set for a cluster.

#. (Optional) To modify an automated snapshot policy, click **Modify** in the **Operation** column.

   |image7|

#. (Optional) To preview a policy, click **Preview Policy**. The next seven snapshots of the cluster will be displayed. If no full snapshot policy is configured for the cluster, the default policy is used, that is, a full snapshot is taken after every 14 incremental snapshots.

   |image8|

   .. important::

      Implementation of the same policy varies according to operations in the cluster. For example:

      -  The policy preview time is for your reference only. The cluster triggers a snapshot within one hour before or after the preset time.
      -  The next automated snapshots after cluster scale-out, upgrade, resize, and media modification are full snapshots by default.
      -  If a periodic policy is used for a cluster, no automated backup is allowed within 4 hours after the last automated snapshot is complete.
      -  If the snapshot start time of multiple policies conflicts, the priorities of the policies are as follows: one-time > periodic > full > incremental.
      -  You can use any backup, full or incremental, to restore the full data of a resource.

.. |image1| image:: /_static/images/en-us_image_0000001276999057.png
.. |image2| image:: /_static/images/en-us_image_0000001277279565.png
.. |image3| image:: /_static/images/en-us_image_0000001232559208.png
.. |image4| image:: /_static/images/en-us_image_0000001277158833.png
.. |image5| image:: /_static/images/en-us_image_0000001232399264.png
.. |image6| image:: /_static/images/en-us_image_0000001232719136.png
.. |image7| image:: /_static/images/en-us_image_0000001277270017.png
.. |image8| image:: /_static/images/en-us_image_0000001232389880.png
