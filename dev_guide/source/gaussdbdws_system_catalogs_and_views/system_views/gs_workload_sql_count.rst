:original_name: dws_04_0709.html

.. _dws_04_0709:

GS_WORKLOAD_SQL_COUNT
=====================

**GS_WORKLOAD_SQL_COUNT** displays statistics on the number of SQL statements executed in workload Cgroups on the current node, including the number of **SELECT**, **UPDATE**, **INSERT**, and **DELETE** statements and the number of DDL, DML, and DCL statements.

.. table:: **Table 1** **GS_WORKLOAD_SQL_COUNT** columns

   ============ ====== ===============================
   Column       Type   Description
   ============ ====== ===============================
   workload     Name   Workload Cgroup name
   select_count Bigint Number of **SELECT** statements
   update_count Bigint Number of **UPDATE** statements
   insert_count Bigint Number of **INSERT** statements
   delete_count Bigint Number of **DELETE** statements
   ddl_count    Bigint Number of **DDL** statements
   dml_count    Bigint Number of **DML** statements
   dcl_count    Bigint Number of **DCL** statements
   ============ ====== ===============================
