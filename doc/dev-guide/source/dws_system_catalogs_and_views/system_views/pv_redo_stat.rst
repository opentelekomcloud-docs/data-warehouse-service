:original_name: dws_04_0856.html

.. _dws_04_0856:

PV_REDO_STAT
============

**PV_REDO_STAT** displays statistics on redoing Xlogs on the current node.

.. table:: **Table 1** PV_REDO_STAT columns

   ========= ====== ==================================
   Name      Type   Description
   ========= ====== ==================================
   phywrts   Bigint Number of physical writes.
   phyblkwrt Bigint Number of physical blocks written.
   writetim  Bigint Time taken for physical writes.
   avgiotim  Bigint Average time taken per write.
   lstiotim  Bigint Time taken for the last write.
   miniotim  Bigint Minimum time taken for a write.
   maxiowtm  Bigint Maximum time taken for a write.
   ========= ====== ==================================
