:original_name: dws_04_0682.html

.. _dws_04_0682:

GLOBAL_REDO_STAT
================

**GLOBAL_REDO_STAT** displays the total statistics of XLOG redo operations on all nodes in a cluster. Except the **avgiotim** column (indicating the average redo write time of all nodes), the names of the other columns in this view are the same as those in the :ref:`PV_REDO_STAT <dws_04_0856>` view. The respective meanings of the other columns are the sum of the values of the same columns in the **PV_REDO_STAT** view on each node.

.. table:: **Table 1** GLOBAL_REDO_STAT columns

   ========= ====== ==================================================
   Column    Type   Description
   ========= ====== ==================================================
   phywrts   Bigint Total number of physical writes on all nodes
   phyblkwrt Bigint Total number of physical write blocks on all nodes
   writetim  Bigint Total physical write time of all nodes
   avgiotim  Bigint Average redo write time of all nodes
   lstiotim  Bigint Sum of the last write time of all nodes
   miniotim  Bigint Sum of the minimum write time of all nodes
   maxiowtm  Bigint Sum of the maximum write time of all nodes
   ========= ====== ==================================================

.. note::

   This view is accessible only to users with system administrator rights.
