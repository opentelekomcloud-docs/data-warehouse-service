:original_name: dws_04_0856.html

.. _dws_04_0856:

PV_REDO_STAT
============

**PV_REDO_STAT** displays statistics on redoing Xlogs on the current node.

.. table:: **Table 1** PV_REDO_STAT columns

   ========= ====== ================================
   Name      Type   Description
   ========= ====== ================================
   phywrts   bigint Number of physical writes
   phyblkwrt bigint Number of physical write blocks
   writetim  bigint Time consumed by physical writes
   avgiotim  bigint Average time for each write
   lstiotim  bigint Last write time
   miniotim  bigint Minimum write time
   maxiowtm  bigint Maximum write time
   ========= ====== ================================
