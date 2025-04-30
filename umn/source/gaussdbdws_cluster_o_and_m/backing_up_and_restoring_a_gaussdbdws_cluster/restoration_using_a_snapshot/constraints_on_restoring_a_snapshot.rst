:original_name: dws_01_1029.html

.. _dws_01_1029:

Constraints on Restoring a Snapshot
===================================

Cluster-Level Snapshot Restoration
----------------------------------

Cluster-level restoration consists of two steps:

#. Data restoration: Restores data in the backup set to the data directory of each primary DN/CN instance in parallel.
#. Rebuilding the standby DN: After the primary DN is restored, standby DNs are rebuilt with full data in parallel.

.. note::

   -  The restoration process takes 1.5 to 2 times longer than the backup process.
   -  After a cluster-level restoration, the parameters will be identical to those during the backup. The new cluster must have the same specifications as the original cluster. If any changes were made to the specifications of the original cluster, the new cluster must still match the specifications prior to the changes. If the specifications of the new cluster are smaller, the restoration may fail.
