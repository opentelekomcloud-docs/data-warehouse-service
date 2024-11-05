:original_name: dws_04_0845.html

.. _dws_04_0845:

PGXC_WORKLOAD_TRANSACTION
=========================

PGXC_WORKLOAD_TRANSACTION provides transaction information about workload cgroups on all CNs. Only the system administrator or the preset role **gs_role_read_all_stats** can access this view. This view is valid only when the real-time resource monitoring function is enabled, that is, when :ref:`enable_resource_track <en-us_topic_0000001510522653__s9530ecdd2b0d4a98b67b66e32bf8e5d0>` is **on**.

.. table:: **Table 1** PGXC_WORKLOAD_TRANSACTION columns

   ================ ====== ================================
   Name             Type   Description
   ================ ====== ================================
   node_name        name   Node name
   workload         name   Workload Cgroup name
   commit_counter   bigint Number of the commit times
   rollback_counter bigint Number of rollbacks
   resp_min         bigint Minimum response time (unit: μs)
   resp_max         bigint Maximum response time (unit: μs)
   resp_avg         bigint Average response time (unit: μs)
   resp_total       bigint Total response time (unit: μs)
   ================ ====== ================================
