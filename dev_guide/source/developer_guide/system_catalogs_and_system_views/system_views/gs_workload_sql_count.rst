:original_name: dws_04_0709.html

.. _dws_04_0709:

GS_WORKLOAD_SQL_COUNT
=====================

**GS_WORKLOAD_SQL_COUNT** displays statistics on the number of SQL statements executed in workload Cgroups on the current node, including the number of **SELECT**, **UPDATE**, **INSERT**, and **DELETE** statements and the number of DDL, DML, and DCL statements.

.. table:: **Table 1** **GS_WORKLOAD_SQL_COUNT** columns

   ============ ====== ===============================
   Name         Type   Description
   ============ ====== ===============================
   workload     name   Workload Cgroup name
   select_count bigint Number of **SELECT** statements
   update_count bigint Number of **UPDATE** statements
   insert_count bigint Number of **INSERT** statements
   delete_count bigint Number of **DELETE** statements
   ddl_count    bigint Number of **DDL** statements
   dml_count    bigint Number of **DML** statements
   dcl_count    bigint Number of **DCL** statements
   ============ ====== ===============================
