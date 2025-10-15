:original_name: dws_04_0845.html

.. _dws_04_0845:

PGXC_WORKLOAD_TRANSACTION
=========================

PGXC_WORKLOAD_TRANSACTION provides transaction information about workload cgroups on all CNs. Only the system administrator or the preset role **gs_role_read_all_stats** can access this view. This view is valid only when the real-time resource monitoring function is enabled, that is, when :ref:`enable_resource_track <en-us_topic_0000001811490709__s9530ecdd2b0d4a98b67b66e32bf8e5d0>` is **on**.

.. table:: **Table 1** PGXC_WORKLOAD_TRANSACTION columns

   ================ ====== =======================================
   Column           Type   Description
   ================ ====== =======================================
   node_name        Name   Node name.
   workload         Name   Workload Cgroup name.
   commit_counter   Bigint Number of the commits.
   rollback_counter Bigint Number of rollbacks.
   resp_min         Bigint Minimum response time, in microseconds.
   resp_max         Bigint Maximum response time, in microseconds.
   resp_avg         Bigint Average response time, in microseconds.
   resp_total       Bigint Total response time, in microseconds.
   ================ ====== =======================================
