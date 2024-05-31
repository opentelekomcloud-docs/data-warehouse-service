:original_name: dws_04_0686.html

.. _dws_04_0686:

GLOBAL_WORKLOAD_SQL_ELAPSE_TIME
===============================

**GLOBAL_WORKLOAD_SQL_ELAPSE_TIME** displays statistics on the response time of SQL statements in all workload Cgroups in a cluster, including the maximum, minimum, average, and total response time of **SELECT**, **UPDATE**, **INSERT**, and **DELETE** statements. The unit is microsecond.

.. table:: **Table 1** **GLOBAL_WORKLOAD_SQL_ELAPSE_TIME** columns

   =================== ====== ===================================
   Name                Type   Description
   =================== ====== ===================================
   workload            name   Workload Cgroup name
   total_select_elapse bigint Total response time of **SELECT**
   max_select_elapse   bigint Maximum response time of **SELECT**
   min_select_elapse   bigint Minimum response time of **SELECT**
   avg_select_elapse   bigint Average response time of **SELECT**
   total_update_elapse bigint Total response time of **UPDATE**
   max_update_elapse   bigint Maximum response time of **UPDATE**
   min_update_elapse   bigint Minimum response time of **UPDATE**
   avg_update_elapse   bigint Average response time of **UPDATE**
   total_insert_elapse bigint Total response time of **INSERT**
   max_insert_elapse   bigint Maximum response time of **INSERT**
   min_insert_elapse   bigint Minimum response time of **INSERT**
   avg_insert_elapse   bigint Average response time of **INSERT**
   total_delete_elapse bigint Total response time of **DELETE**
   max_delete_elapse   bigint Maximum response time of **DELETE**
   min_delete_elapse   bigint Minimum response time of **DELETE**
   avg_delete_elapse   bigint Average response time of **DELETE**
   =================== ====== ===================================
