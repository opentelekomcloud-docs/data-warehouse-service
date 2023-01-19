:original_name: dws_04_0843.html

.. _dws_04_0843:

PGXC_WORKLOAD_SQL_COUNT
=======================

**PGXC_WORKLOAD_SQL_COUNT** displays statistics on the number of SQL statements executed in workload Cgroups on all CNs in a cluster, including the number of **SELECT**, **UPDATE**, **INSERT**, and **DELETE** statements and the number of DDL, DML, and DCL statements. It is accessible only to users with system administrator rights.

.. table:: **Table 1** **PGXC_WORKLOAD_SQL_COUNT** columns

   ============ ====== ===============================
   Name         Type   Description
   ============ ====== ===============================
   node_name    name   Node name
   workload     name   Workload Cgroup name
   select_count bigint Number of **SELECT** statements
   update_count bigint Number of **UPDATE** statements
   insert_count bigint Number of **INSERT** statements
   delete_count bigint Number of **DELETE** statements
   ddl_count    bigint Number of **DDL** statements
   dml_count    bigint Number of **DML** statements
   dcl_count    bigint Number of **DCL** statements
   ============ ====== ===============================
