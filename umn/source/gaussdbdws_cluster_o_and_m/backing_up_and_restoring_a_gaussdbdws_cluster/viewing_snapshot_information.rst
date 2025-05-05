:original_name: dws_01_0021.html

.. _dws_01_0021:

Viewing Snapshot Information
============================

This section describes how to view snapshot information on the **Snapshots** page.


Viewing Snapshot Information
----------------------------

#. Log in to the GaussDB(DWS) console.

#. In the navigation pane, choose **Management** > **Snapshot**.

   In the snapshot list, all snapshots are displayed by default.

#. .. _en-us_topic_0000002203426641__en-us_topic_0000001307409978_en-us_topic_0000001180320191_li25916796163327:

   You can view the **Snapshot Name**, **Snapshot Status**, **Cluster Name**, **Backup Mode**, **Snapshot Type**, **Storage Media**, **Snapshot Level**, and creation time of snapshots.

   You can also enter a snapshot name or cluster name in the upper right corner of the snapshot list and click |image1| to search for the specified snapshot. GaussDB(DWS) supports fuzzy search.

   :ref:`Table 1 <en-us_topic_0000002203426641__en-us_topic_0000001307409978_en-us_topic_0000001180320191_table3259774163926>` describes snapshot status.

   .. _en-us_topic_0000002203426641__en-us_topic_0000001307409978_en-us_topic_0000001180320191_table3259774163926:

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

   :ref:`Table 2 <en-us_topic_0000002203426641__en-us_topic_0000001307409978_en-us_topic_0000001180320191_table875924217540>` lists the backup modes.

   .. _en-us_topic_0000002203426641__en-us_topic_0000001307409978_en-us_topic_0000001180320191_table875924217540:

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
      | NFS            | The created snapshot is an NFS snapshot and the backup data is stored on the NFS server. |
      +----------------+------------------------------------------------------------------------------------------+

   The following table describes the snapshot levels.

   .. table:: **Table 5** Snapshot levels

      +----------------+-----------------------------------------------------------------------------------------------+
      | Snapshot Level | Description                                                                                   |
      +================+===============================================================================================+
      | cluster        | A backup of all the configurations and service data of a cluster at a specific point in time. |
      +----------------+-----------------------------------------------------------------------------------------------+
      | schema         | A backup of all the service data of a schema at a specific point in time.                     |
      +----------------+-----------------------------------------------------------------------------------------------+

Querying Snapshot Information by Table Name
-------------------------------------------

**Prerequisites**

Only the snapshots that are created after the fine-grained snapshot function is enabled support fine-grained search.

.. note::

   Only cluster versions 8.2.1.230 and later support snapshot query by table name.

**Procedure**

#. Log in to the GaussDB(DWS) console.

#. Choose **Dedicated Clusters** > **Clusters** in the navigation pane.

#. In the cluster list, click the name of the target cluster. The **Cluster Information** page is displayed.

#. In the navigation pane, choose **Snapshots**. Click the **Snapshots** tab and enable the fine-grained snapshot function. Click the advanced search button on the right.

   You can set a triplet consisting of a database, a schema, and a table in a fine-grained query on the snapshot information in a specified database schema table. For details about the snapshot information, see :ref:`3 <en-us_topic_0000002203426641__en-us_topic_0000001307409978_en-us_topic_0000001180320191_li25916796163327>`.

   |image2|

   .. table:: **Table 6** Triplet description

      ========== ==============
      Tuple Name Description
      ========== ==============
      database   Database name.
      schema     Schema name.
      table      Table name.
      ========== ==============

.. |image1| image:: /_static/images/en-us_image_0000002203312533.jpg
.. |image2| image:: /_static/images/en-us_image_0000002203312529.png
