:original_name: dws_04_0712.html

.. _dws_04_0712:

GS_STAT_DB_CU
=============

**GS_STAT_DB_CU** displsys CU hits in a database and in each node in a cluster. You can clear it using **gs_stat_reset()**.

.. table:: **Table 1** GS_STAT_DB_CU columns

   ============= ======= ======================================
   Name          Type    Description
   ============= ======= ======================================
   node_name1    text    Node name
   db_name       text    Database name
   mem_hit       integer Number of memory hits
   hdd_sync_read integer Number of hard disk synchronous reads
   hdd_asyn_read integer Number of hard disk asynchronous reads
   ============= ======= ======================================
