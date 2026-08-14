:original_name: dws_04_0843.html

.. _dws_04_0843:

PGXC_WORKLOAD_SQL_COUNT
=======================

**PGXC_WORKLOAD_SQL_COUNT** displays statistics on the number of SQL statements executed in workload Cgroups on all CNs in a cluster, including the number of **SELECT**, **UPDATE**, **INSERT**, and **DELETE** statements and the number of DDL, DML, and DCL statements. Only the system administrator or the preset role **gs_role_read_all_stats** can access this view.

.. table:: **Table 1** **PGXC_WORKLOAD_SQL_COUNT** columns

   ============ ====== ================================
   Column       Type   Description
   ============ ====== ================================
   node_name    Name   Node name.
   workload     Name   Workload Cgroup name.
   select_count Bigint Number of **SELECT** statements.
   update_count Bigint Number of **UPDATE** statements.
   insert_count Bigint Number of **INSERT** statements.
   delete_count Bigint Number of **DELETE** statements.
   ddl_count    Bigint Number of **DDL** statements.
   dml_count    Bigint Number of **DML** statements.
   dcl_count    Bigint Number of **DCL** statements.
   ============ ====== ================================
