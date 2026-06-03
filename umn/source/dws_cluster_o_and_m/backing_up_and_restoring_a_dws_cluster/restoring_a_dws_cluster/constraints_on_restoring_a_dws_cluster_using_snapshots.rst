:original_name: dws_01_00097.html

.. _dws_01_00097:

Constraints on Restoring a DWS Cluster Using Snapshots
======================================================

Cluster Snapshot Restoration
----------------------------

-  Cluster snapshot restoration includes restoring data and rebuilding the standby DNs. Data restoration means restoring data in the backup set to the data directory of each primary CN and DN instance in parallel. After the primary DN is restored, standby DNs are rebuilt with full data in parallel. The restoration process is 1.5 to 2 times longer than the backup process.
-  By default, the new cluster created during restoration has the same specifications and node quantity as the original cluster. If any changes were made to the specifications of the original cluster, the new cluster must still match the specifications prior to the changes. If the specifications of the new cluster are smaller, the restoration may fail.

-  Currently, you can only use the snapshots stored in OBS to restore data to a new cluster.
-  The resources required for restoring data to a new cluster do not exceed your available resource quota.
-  Logical clusters and resource pools cannot be restored to a new cluster or the current cluster.

-  Snapshots can be restored to the current cluster only when the cluster version is 8.1.3.200 or later.
-  Only a snapshot in the **Available** state can be used for restoration.
