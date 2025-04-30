:original_name: dws_04_0815.html

.. _dws_04_0815:

PGXC_REDO_STAT
==============

**PGXC_REDO_STAT** displays statistics on redoing Xlogs of each node in the cluster. All columns except **node_name** are the same as those in the :ref:`PV_REDO_STAT <dws_04_0856>` view. Only the system administrator or the preset role **gs_role_read_all_stats** can access this view.

.. table:: **Table 1** PGXC_REDO_STAT columns

   ========= ====== =================================
   Column    Type   Description
   ========= ====== =================================
   node_name Text   Node name
   phywrts   Bigint Number of physical writes
   phyblkwrt Bigint Number of physical blocks written
   writetim  Bigint Time taken for physical writes
   avgiotim  Bigint Average time taken per write
   lstiotim  Bigint Time taken for the last write
   miniotim  Bigint Minimum time taken for a write
   maxiowtm  Bigint Maximum time taken for a write
   ========= ====== =================================
