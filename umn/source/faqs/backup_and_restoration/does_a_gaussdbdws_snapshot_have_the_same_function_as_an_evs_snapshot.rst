:original_name: dws_03_2131.html

.. _dws_03_2131:

Does a GaussDB(DWS) Snapshot Have the Same Function as an EVS Snapshot?
=======================================================================

No.

GaussDB(DWS) snapshots are used to restore all the configurations and service data of a cluster. EVS snapshots are used to restore the service data of a data disk or system disk within a specific time period.

GaussDB(DWS) Snapshot
---------------------

A GaussDB(DWS) snapshot is a full backup and an incremental backup of a data warehouse cluster at a point in time. It records the data in the current database and cluster information, including the number of nodes, node specifications, and database administrator username. Snapshots can be created manually or automatically.

When restoring data from a snapshot to a cluster, GaussDB(DWS) creates a cluster based on the cluster information recorded in the snapshot and restores database information from the snapshot data.

For details, see "Backing Up and Restoring a GaussDB(DWS) Cluster" in the *Data Warehouse Service (DWS) User Guide*.

EVS snapshot
------------

An EVS snapshot is a complete copy or image of the disk data taken at a specific time point. Snapshot is a major disaster recovery approach, and you can completely restore data of a snapshot to the time when the snapshot was created.

You can create snapshots to rapidly save the disk data at specified time points. In addition, you can use snapshots to create new disks so that the created disks will contain the snapshot data in the beginning.

You can create snapshots to rapidly save the disk data at specified time points to implement data disaster recovery.

-  If data loss occurs, you can use a snapshot to completely restore the data to the time point when the snapshot was created.
-  You can use snapshots to create new disks so that the created disks will contain the snapshot data.

For details, see section "EVS Snapshot (OBT)" in the *Elastic Volume Service Product Description*.
