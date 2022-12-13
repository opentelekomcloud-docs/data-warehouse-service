:original_name: dws_01_10131.html

.. _dws_01_10131:

Automated Snapshot Overview
===========================

Automated snapshots adopt differential incremental backups. The automated snapshot created for the first time is a full backup (base version), and then the system creates full backups at a specified interval. Incremental backups are generated between two full backups. The incremental backup records change based on the previous backup.

During snapshot restoration, GaussDB(DWS) uses all backups between the latest full backup and the current incremental backup to restore the cluster. This can prevent data loss.

If the retention period of an incremental snapshot exceeds the maximum retention period, GaussDB(DWS) does not delete the snapshot immediately. Instead, GaussDB(DWS) retains it until the next full backup, when the deletion of the snapshot will not hinder incremental data backup and restoration.


.. figure:: /_static/images/en-us_image_0000001232285254.png
   :alt: **Figure 1** Snapshot backup process

   **Figure 1** Snapshot backup process

Automated snapshots are enabled by default when you create a cluster. If automated snapshots are enabled for a cluster, GaussDB(DWS) periodically takes snapshots of that cluster based on the time and interval you set, usually every eight hours. You can configure one or more automated snapshot policies for the cluster as required. For details, see :ref:`Configuring an Automated Snapshot Policy <dws_01_0089>`.

The retention period of an automated snapshot can be set to 1 to 31 days. The default retention period is 3 days. The system deletes the snapshot at the end of the retention period. If you want to keep an automated snapshot for a longer period, you can create a copy of it as a manual snapshot. The automated snapshot is retained until the end of the retention period, whrereas the corresponding manual snapshot is retained until you manually delete it. For details about how to replicate an automated snapshot, see :ref:`Copying Automated Snapshots <dws_01_0085>`.

.. caution::

   If you disable the automated snapshot function for an existing cluster, all its automated snapshots will be deleted. However, manual snapshots will not be deleted.
