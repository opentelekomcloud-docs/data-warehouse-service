:original_name: dws_04_0711.html

.. _dws_04_0711:

GS_WORKLOAD_TRANSACTION
=======================

GS_WORKLOAD_TRANSACTION provides transaction information about workload cgroups on a single CN. The database records the number of times that each workload Cgroup commits and rolls back transactions and the response time of transaction commitment and rollback, in microseconds.

.. table:: **Table 1** GS_WORKLOAD_TRANSACTION columns

   ================ ====== =====================
   Column           Type   Description
   ================ ====== =====================
   workload         Name   Workload Cgroup name
   commit_counter   Bigint Number of the commits
   rollback_counter Bigint Number of rollbacks
   resp_min         Bigint Minimum response time
   resp_max         Bigint Maximum response time
   resp_avg         Bigint Average response time
   resp_total       Bigint Total response time
   ================ ====== =====================
