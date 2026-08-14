:original_name: dws_04_0713.html

.. _dws_04_0713:

GS_STAT_SESSION_CU
==================

**GS_STAT_SESSION_CU** displays the CU hit rate of running sessions on each node in a cluster. This data about a session is cleared when you exit this session or restart the cluster.

.. table:: **Table 1** GS_STAT_SESSION_CU columns

   ============= ======= =================================
   Column        Type    Description
   ============= ======= =================================
   node_name1    Text    Node name
   mem_hit       Integer Number of memory hits
   hdd_sync_read Integer Number of disk synchronous reads
   hdd_asyn_read Integer Number of disk asynchronous reads
   ============= ======= =================================
