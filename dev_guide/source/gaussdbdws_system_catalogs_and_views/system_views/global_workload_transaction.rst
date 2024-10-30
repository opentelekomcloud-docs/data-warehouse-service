:original_name: dws_04_0687.html

.. _dws_04_0687:

GLOBAL_WORKLOAD_TRANSACTION
===========================

**GLOBAL_WORKLOAD_TRANSACTION** provides the total transaction information about workload Cgroups on all CNs in the cluster. This view is accessible only to users with system administrator rights. It is valid only when the real-time resource monitoring function is enabled, that is, **enable_resource_track** is **on**.

.. table:: **Table 1** GLOBAL_WORKLOAD_TRANSACTION columns

   ================ ====== ===========================================
   Name             Type   Description
   ================ ====== ===========================================
   workload         name   Workload Cgroup name
   commit_counter   bigint Total number of submission times on each CN
   rollback_counter bigint Total number of rollback times on each CN
   resp_min         bigint Minimum response time of the cluster
   resp_max         bigint Maximum response time of the cluster
   resp_avg         bigint Average response time on each CN
   resp_total       bigint Total response time on each CN
   ================ ====== ===========================================
