:original_name: dws_01_1029.html

.. _dws_01_1029:

Constraints on Restoring a Snapshot
===================================

Cluster-Level Snapshot Restoration
----------------------------------

Cluster-level restoration consists of two steps:

#. Data restoration: The backup tool simultaneously restores data from the backup set to the data directory of each instance, including the primary CN and primary DN.
#. Rebuilding the standby DN: After the primary DN is restored, standby DNs are rebuilt with full data in parallel.

.. note::

   -  The restoration process takes 1.5 to 2 times longer than the backup process.
   -  The parameters after cluster-level restoration are the same as those before backup. When restoring data to a new cluster, ensure that the flavor of the new cluster is the same as that of the original cluster. If the flavor of the new cluster is smaller, the restoration may fail.
