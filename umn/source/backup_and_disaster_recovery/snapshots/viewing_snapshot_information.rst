:original_name: dws_01_0021.html

.. _dws_01_0021:

Viewing Snapshot Information
============================

This section describes how to view snapshot information on the **Snapshots** page.


Viewing Snapshot Information
----------------------------

#. Log in to the GaussDB(DWS) management console.

#. In the navigation pane on the left, choose **Snapshots**.

   In the snapshot list, all snapshots are displayed by default. Click |image1| next to the snapshot name to check the snapshot details.

   |image2|

#. You can view the **Snapshot Name**, **Snapshot Status**, **Cluster Name**, **Backup Mode**, **Snapshot Type**, **Storage Medium**, and creation time of snapshots.

   You can also enter a snapshot name or cluster name in the upper right corner of the snapshot list and click |image3| to search for the specified snapshot. GaussDB(DWS) supports fuzzy search.

   :ref:`Table 1 <en-us_topic_0000001466754510__en-us_topic_0000001307409978_en-us_topic_0000001180320191_table3259774163926>` describes snapshot status.

   .. _en-us_topic_0000001466754510__en-us_topic_0000001307409978_en-us_topic_0000001180320191_table3259774163926:

   .. table:: **Table 1** Snapshot status

      +-----------------+---------------------------------------------------------------+
      | Status          | Description                                                   |
      +=================+===============================================================+
      | **Available**   | Indicates that the existing snapshot works properly.          |
      +-----------------+---------------------------------------------------------------+
      | **Creating**    | Indicates that a snapshot is being created.                   |
      +-----------------+---------------------------------------------------------------+
      | **Unavailable** | Indicates that the existing snapshot cannot provide services. |
      +-----------------+---------------------------------------------------------------+

   The following table describes the backup modes.

   .. table:: **Table 2** Backup modes

      +---------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Type          | Description                                                                                                                                                                                                                                                  |
      +===============+==============================================================================================================================================================================================================================================================+
      | **Manual**    | Indicates the snapshot that you manually create through the GaussDB(DWS) management console or using APIs. You can delete the snapshots that are manually created.                                                                                           |
      +---------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | **Automated** | Indicates the snapshot that is automatically created after the automated snapshot backup policy is enabled. You cannot delete the snapshots that are automatically created. The system automatically deletes the snapshots whose retention duration expires. |
      +---------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   The following table describes the snapshot types.

   .. table:: **Table 3** Type

      =========== ======================================
      Type        Description
      =========== ======================================
      Full        The snapshot is a full backup.
      Incremental The snapshot is an incremental backup.
      =========== ======================================

   The following table describes the snapshot media.

   .. table:: **Table 4** Storage media

      +----------------+------------------------------------------------------------------------------------------+
      | Storage Medium | Description                                                                              |
      +================+==========================================================================================+
      | OBS            | The created snapshot is an OBS snapshot and the backup data is stored on the OBS server. |
      +----------------+------------------------------------------------------------------------------------------+

.. |image1| image:: /_static/images/en-us_image_0000001517754405.jpg
.. |image2| image:: /_static/images/en-us_image_0000001466914334.png
.. |image3| image:: /_static/images/en-us_image_0000001466754710.jpg
