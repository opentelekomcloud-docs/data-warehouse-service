:original_name: dws_04_0687.html

.. _dws_04_0687:

GLOBAL_WORKLOAD_TRANSACTION
===========================

**GLOBAL_WORKLOAD_TRANSACTION** provides the total transaction information about workload Cgroups on all CNs in the cluster. This view is accessible only to users with system administrator rights. It is valid only when the real-time resource monitoring function is enabled, that is, **enable_resource_track** is **on**.

.. table:: **Table 1** GLOBAL_WORKLOAD_TRANSACTION columns

   ================ ====== ===========================================
   Column           Type   Description
   ================ ====== ===========================================
   workload         Name   Workload Cgroup name
   commit_counter   Bigint Total number of submission times on each CN
   rollback_counter Bigint Total number of rollback times on each CN
   resp_min         Bigint Minimum response time of the cluster
   resp_max         Bigint Maximum response time of the cluster
   resp_avg         Bigint Average response time on each CN
   resp_total       Bigint Total response time on each CN
   ================ ====== ===========================================
