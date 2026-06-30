:original_name: dws_01_0021.html

.. _dws_01_0021:

Viewing Snapshots of a DWS Cluster
==================================

This section describes how to view snapshot information on the **Snapshots** page.

Viewing Snapshot Information
----------------------------

#. Log in to the DWS console.

#. In the navigation pane on the left, choose **Dedicated Clusters** > **Management** > **Snapshots**. In the snapshot list, all snapshots are displayed by default.

#. You can view the **Snapshot Name**, **Snapshot Status**, **Cluster Name**, **Backup Mode**, **Snapshot Type**, **Storage Media**, and creation time of snapshots.

   You can also enter a snapshot name or cluster name in the upper right corner of the snapshot list and click |image1| to search for the specified snapshot. DWS supports fuzzy search.

   :ref:`Table 1 <en-us_topic_0000002329489594__en-us_topic_0000001422959269_en-us_topic_0000001307409978_en-us_topic_0000001180320191_table3259774163926>` describes snapshot status.

   .. _en-us_topic_0000002329489594__en-us_topic_0000001422959269_en-us_topic_0000001307409978_en-us_topic_0000001180320191_table3259774163926:

   .. table:: **Table 1** Snapshot status

      =========== ===============================
      Status      Description
      =========== ===============================
      Available   The snapshot works properly.
      Creating    The snapshot is being created.
      Restoring   The snapshot is being restored.
      Unavailable The snapshot is unavailable.
      =========== ===============================

   :ref:`Table 2 <en-us_topic_0000002329489594__en-us_topic_0000001422959269_en-us_topic_0000001307409978_en-us_topic_0000001180320191_table875924217540>` lists the backup modes.

   .. _en-us_topic_0000002329489594__en-us_topic_0000001422959269_en-us_topic_0000001307409978_en-us_topic_0000001180320191_table875924217540:

   .. table:: **Table 2** Backup modes

      +-----------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Type      | Description                                                                                                                                                                                                                          |
      +===========+======================================================================================================================================================================================================================================+
      | Manual    | Snapshot that you manually create through the DWS console or using APIs. You can delete the snapshot.                                                                                                                                |
      +-----------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Automated | The snapshot was automatically created by the system when automated snapshot backup is enabled for the cluster. The system automatically deletes the automated snapshots after the retention period expires. You cannot delete them. |
      +-----------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   :ref:`Table 3 <en-us_topic_0000002329489594__en-us_topic_0000001422959269_en-us_topic_0000001307409978_table5780051267>` lists the snapshot types.

   .. _en-us_topic_0000002329489594__en-us_topic_0000001422959269_en-us_topic_0000001307409978_table5780051267:

   .. table:: **Table 3** Snapshot types

      =========== ======================================
      Type        Description
      =========== ======================================
      Full        The snapshot is a full backup.
      Incremental The snapshot is an incremental backup.
      =========== ======================================

   :ref:`Table 4 <en-us_topic_0000002329489594__en-us_topic_0000001422959269_en-us_topic_0000001307409978_en-us_topic_0000001180320191_table1793135164913>` describes the storage media of the snapshot.

   .. _en-us_topic_0000002329489594__en-us_topic_0000001422959269_en-us_topic_0000001307409978_en-us_topic_0000001180320191_table1793135164913:

   .. table:: **Table 4** Storage media

      +---------------+------------------------------------------------------------------------------------------+
      | Storage Media | Description                                                                              |
      +===============+==========================================================================================+
      | OBS           | The created snapshot is an OBS snapshot and the backup data is stored on the OBS server. |
      +---------------+------------------------------------------------------------------------------------------+
      | NFS           | The created snapshot is an NFS snapshot and the backup data is stored on the NFS server. |
      +---------------+------------------------------------------------------------------------------------------+

.. |image1| image:: /_static/images/en-us_image_0000002363770209.jpg
