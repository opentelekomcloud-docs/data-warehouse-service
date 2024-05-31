:original_name: dws_01_0151.html

.. _dws_01_0151:

Overview
========

A snapshot is a full or incremental backup of a GaussDB(DWS) cluster at a specific point in time. It records the current database data and cluster information, including the number of nodes, node specifications, and database administrator name. Snapshots can be created manually or automatically. For details, see :ref:`Manual Snapshots <dws_01_0092>` and :ref:`Automated Snapshots <dws_01_1013>`.

If you restore a snapshot to a new cluster, GaussDB(DWS) creates a new cluster based on the cluster information recorded in the snapshot, and then restores data from the snapshot. For more information, see :ref:`Restoring a Snapshot to a New Cluster <dws_01_0029>`.

If you restore a snapshot to the original cluster, GaussDB(DWS) clears the existing data in the cluster, and then restores the database information from the snapshot to the cluster. For more information, see :ref:`Restoring a Snapshot to the Original Cluster <dws_01_00097>`.

The snapshot backup and restoration rates are listed below. (The rates are obtained from the test environment with local SSDs as the backup media. The rates are for reference only.) The actual rate depends on your disk, network, and bandwidth resources.)

-  Backup rate: 200 MB/s/DN
-  Restoration rate: 125 MB/s/DN

.. note::

   -  Snapshot storage space

      -  The cluster storage is provided by GaussDB(DWS) free of charge. Cluster storage = Storage space per node x Number of nodes

   -  The dependency of the snapshot service is as follows:

      -  Only the snapshots stored in OBS can be used to restore data to a new cluster.

   -  The new GaussDB(DWS) cluster created based on the snapshot must have the same configurations as the original cluster. That is, the number and specifications of nodes, memory, and disks in the new cluster must be the same as those in the original cluster.
   -  If you create a new cluster based on a snapshot without modifying parameters, the parameters of the new cluster will be the same as those of the snapshot.
   -  During snapshot creation, do not perform the VACUUM FULL operation, or the cluster may become read-only.
   -  Snapshot creation affects disk I/O performance. You are advised to create snapshots during off-peak hours.
   -  During the snapshot creation, some intermediate files are retained, which occupy extra disk space. Therefore, create snapshots in off-peak hours and ensure that the disk capacity usage is less than 70%.
