:original_name: dws_01_0151.html

.. _dws_01_0151:

DWS Snapshot Overview
=====================

A snapshot is a full or incremental backup of a DWS cluster at a specific point in time. It records the current database data and cluster information, including the number of nodes, node specifications, and database administrator name. You can back up and restore clusters using snapshots. Snapshots can be created manually or automatically. For details, see :ref:`Creating and Managing a DWS Snapshot <dws_01_0028>` and :ref:`Configuring and Managing DWS Automated Snapshots <dws_01_0089>`. For details about how to restore a snapshot, see :ref:`Restoring a DWS Cluster <dws_01_1015>`. **Backing up the cluster is essential for maintaining data reliability, especially when the service provider cannot restore data through upstream re-import. This helps prevent data loss caused by human or other factors.**

The snapshot backup and restoration rates are listed below. The rates are obtained from the test environment with local SSDs as the backup media. The rates are for reference only. The actual rate depends on your disk, network, and bandwidth resources.

-  Backup rate: 200 MB/s/DN
-  Restoration rate: 125 MB/s/DN

Notes and Constraints
---------------------

-  The cluster versions that support schema-level snapshots are listed below. If the current console interface does not support this feature, contact technical support.

   -  9.1.0.100 or later
   -  8.3.0.110 or later 8.3.0.\ *xxx*
   -  8.2.1.230 or later 8.2.1.2\ *xx*

-  OBS snapshot storage space

   -  The cluster storage is provided by DWS free of charge. Cluster storage = Storage space per node x Number of nodes

-  The dependency of the snapshot service is as follows:

   -  The snapshot management function depends on OBS or NFS.
   -  If the backup device is an NFS backup media, the NFS backup media must be mounted to the high-performance SFS Turbo. For details, see :ref:`11.1.3.2 Automatic Snapshot Policy <en-us_topic_0000002329649414__en-us_topic_0000001423119273_section069713349183>`.
   -  Only the snapshots stored in OBS can be used to restore data to a new cluster.

-  The new DWS cluster created based on the snapshot must have the same configurations as the original cluster. That is, the number and specifications of nodes, memory, and disks in the new cluster must be the same as those in the original cluster.
-  If you create a cluster based on a snapshot without modifying parameters, the parameters of the new cluster will be the same as those of the snapshot.
-  During snapshot creation, do not perform the **VACUUM FULL** operation, or the cluster may become read-only.
-  Snapshot creation affects disk I/O performance. You are advised to create snapshots during off-peak hours.
-  During the snapshot creation, some intermediate files are retained, which occupy extra disk space. Therefore, create snapshots in off-peak hours and ensure that the disk capacity usage is less than 70%.
