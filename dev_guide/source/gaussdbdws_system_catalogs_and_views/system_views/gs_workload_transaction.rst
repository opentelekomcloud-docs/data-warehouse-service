:original_name: dws_04_0711.html

.. _dws_04_0711:

GS_WORKLOAD_TRANSACTION
=======================

GS_WORKLOAD_TRANSACTION provides transaction information about workload cgroups on a single CN. The database records the number of times that each workload Cgroup commits and rolls back transactions and the response time of transaction commitment and rollback, in microseconds.

.. table:: **Table 1** GS_WORKLOAD_TRANSACTION columns

   ================ ====== ==========================
   Name             Type   Description
   ================ ====== ==========================
   workload         name   Workload Cgroup name
   commit_counter   bigint Number of the commit times
   rollback_counter bigint Number of rollbacks
   resp_min         bigint Minimum response time
   resp_max         bigint Maximum response time
   resp_avg         bigint Average response time
   resp_total       bigint Total response time
   ================ ====== ==========================
