:original_name: dws_04_0712.html

.. _dws_04_0712:

GS_STAT_DB_CU
=============

**GS_STAT_DB_CU** displays CU hits of each database in each node of a cluster. You can clear it using **gs_stat_reset()**.

.. table:: **Table 1** GS_STAT_DB_CU columns

   ============= ====== =================================
   Name          Type   Description
   ============= ====== =================================
   node_name1    text   Node name
   db_name       text   Database name
   mem_hit       bigint Number of memory hits
   hdd_sync_read bigint Number of disk synchronous reads
   hdd_asyn_read bigint Number of disk asynchronous reads
   ============= ====== =================================
