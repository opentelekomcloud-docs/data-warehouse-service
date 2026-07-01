:original_name: dws_01_0028.html

.. _dws_01_0028:

Creating and Managing a DWS Snapshot
====================================

A manual snapshot can be created at any time. It will be retained until it is deleted from the DWS console. Manual snapshots are full backup data, which takes a long time to create. You can create a cluster snapshot.

-  :ref:`Creating a Cluster Snapshot <en-us_topic_0000002329489586__en-us_topic_0000001422799449_section151481400504>`: A cluster snapshot is a complete backup that records point-in-time configuration data and service data of a DWS cluster.
-  :ref:`Creating a Schema Snapshot <en-us_topic_0000002329489586__en-us_topic_0000001422799449_section18820143351010>`: A schema snapshot is a backup of specific schemas in a DWS cluster at a specific point in time.

Notes and Constraints
---------------------

-  Manually created snapshots can be backed up to OBS or NFS.
-  To create a snapshot of a cluster, ensure that the cluster is in **Available**, **To be restarted**, or **Unbalanced** state. In cluster versions earlier than 8.1.3.101, you can also create a snapshot of a cluster in **Read-only** state.
-  To create a schema snapshot, ensure that the cluster is in **Available** or **Unbalanced** state.
-  If a snapshot is being created for a cluster, the cluster cannot be restarted, scaled, its password cannot be reset, and its configurations cannot be modified. To ensure the integrity of snapshot data, do not write data during snapshot creation.
-  If the snapshot size is much greater than that of the data stored in the cluster, the data is possibly labeled with a deletion tag, but is not cleared and reclaimed. In this case, clear the data and recreate a snapshot. For details, see :ref:`How Can I Clear and Reclaim the DWS Storage Space? <dws_03_0033>`
-  Schema-level fine-grained snapshots cannot back up comments and permissions of tables.

.. _en-us_topic_0000002329489586__en-us_topic_0000001422799449_section151481400504:

Creating a Cluster Snapshot
---------------------------

#. Log in to the DWS console.

#. In the navigation pane on the left, choose **Dedicated Clusters** > **Management** > **Snapshots**. Alternatively, in the cluster list, click the name of the target cluster to switch to the **Cluster Information** page, and then choose **Snapshots** from the navigation pane.

#. Click **Create Snapshot** in the upper right corner. Alternatively, on the **Clusters** page, locate a cluster, click **More** in the **Operation** column, and select **Create Snapshot**.

#. On the **Create Snapshot** page, set the parameters listed in :ref:`Table 1 <en-us_topic_0000002329489586__en-us_topic_0000001422799449_table1830517518165>`.

   .. _en-us_topic_0000002329489586__en-us_topic_0000001422799449_table1830517518165:

   .. table:: **Table 1** Snapshot parameters

      +----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter            | Description                                                                                                                                                                                           |
      +======================+=======================================================================================================================================================================================================+
      | Cluster Name         | Select a DWS cluster.                                                                                                                                                                                 |
      +----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Snapshot Name        | Enter a snapshot name. The snapshot name must be 4 to 64 characters in length and start with a letter. It is case-insensitive and can contain only letters, digits, hyphens (-), and underscores (_). |
      +----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Snapshot Level       | Select **cluster**.                                                                                                                                                                                   |
      +----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Snapshot Description | Enter the snapshot description. This parameter is optional. The snapshot description must be 0 to 256 characters long and cannot include special characters like !<>'=&"                              |
      +----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Confirm the information and click **Create**.

   The task status of the cluster for which you are creating a snapshot is **Creating snapshot**. The status of the snapshot that is being created is **Creating**. After the snapshot is created, it is in the **Available** state.

Prerequisites for Creating a Schema Snapshot
--------------------------------------------

Manually enable the fine-grained snapshot.

#. In the navigation pane on the left, choose **Dedicated Clusters** > **Management** > **Snapshots**. Alternatively, in the cluster list, click the name of the target cluster to switch to the **Cluster Information** page, and then choose **Snapshots** from the navigation pane.
#. Click **Create Snapshot** in the upper right corner. Alternatively, choose **More** > **Create Snapshot** in the **Operation** column.
#. Click |image1| next to **Snapshot Level** and click **Configure**.
#. On the **Snapshots** page, enable **Fine-grained Snapshot**. After enabling fine-grained snapshot, you can restore tables from automatic or manual snapshots.

.. _en-us_topic_0000002329489586__en-us_topic_0000001422799449_section18820143351010:

Creating a Schema Snapshot
--------------------------

#. Log in to the DWS console.

#. In the navigation pane on the left, choose **Dedicated Clusters** > **Management** > **Snapshots**. Alternatively, in the cluster list, click the name of the target cluster to switch to the **Cluster Information** page, and then choose **Snapshots** from the navigation pane.

#. Click **Create Snapshot** in the upper right corner. Alternatively, on the **Clusters** page, locate a cluster, click **More** in the **Operation** column, and select **Create Snapshot**.

#. On the **Create Snapshot** page, set parameters based on :ref:`Table 1 <en-us_topic_0000002329489586__en-us_topic_0000001422799449_table1830517518165>`. Select **schema** for **Snapshot Level**.

#. Specify the snapshots to be backed up.

   -  Select a database from the **Database** drop-down list. Schemas in different databases cannot be backed up at a time.
   -  In the schema list, select the schemas to be backed up (Up to 50 schemas can be backed up at a time). To search for a schema, enter its name in the search box in the upper right corner of the list, and click |image2|. DWS supports fuzzy search.

#. Confirm the information and click **Create**.

   The task status of the cluster for which you are creating a snapshot is **Creating snapshot**. The status of the snapshot that is being created is **Creating**. After the snapshot is created, it is in the **Available** state.

Stopping a Manually Created Snapshot
------------------------------------

#. Log in to the DWS console.
#. In the navigation pane on the left, choose **Dedicated Clusters** > **Management** > **Snapshots**. Alternatively, in the cluster list, click the name of the target cluster to switch to the **Cluster Information** page, and then choose **Snapshots** from the navigation pane. All snapshots are displayed by default.
#. In the **Operation** column of a snapshot that is being created, and click **Cancel Snapshot Creation**.
#. In the dialog box that is displayed, click **Yes** to stop the snapshot. The snapshot state will change to **Unavailable**.

   -  This feature is supported only in cluster version 8.1.3.200 and later.
   -  If the snapshot creation is about to complete, the command for stopping the snapshot will not take effect, and the snapshot will be created.
   -  Only the snapshots in the **Creating** state can be stopped. A snapshot creation task that just started or is about to complete cannot be stopped.

Deleting a Manually Created Snapshot
------------------------------------

#. Log in to the DWS console.
#. In the navigation pane on the left, choose **Dedicated Clusters** > **Management** > **Snapshots**. Alternatively, in the cluster list, click the name of the target cluster to switch to the **Cluster Information** page, and then choose **Snapshots** from the navigation pane. All snapshots are displayed by default.
#. In the **Operation** column of the snapshot that you want to delete, click **More** and select **Delete**.

   .. note::

      You can only delete snapshots that were manually created.

#. Confirm the information, enter **DELETE**, and click **OK** to delete the snapshot. Deleted snapshots cannot be recovered. Exercise caution when performing this operation.

.. |image1| image:: /_static/images/en-us_image_0000002329928960.png
.. |image2| image:: /_static/images/en-us_image_0000002329769160.jpg
