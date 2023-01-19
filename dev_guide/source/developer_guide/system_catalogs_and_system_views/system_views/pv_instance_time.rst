:original_name: dws_04_0849.html

.. _dws_04_0849:

PV_INSTANCE_TIME
================

**PV_INSTANCE_TIME** collects statistics on the running time of processes and the time consumed in each execution phase, in microseconds.

**PV_INSTANCE_TIME** records time consumption information of the current node. The time consumption information is classified into the following types:

-  **DB_TIME**: effective time spent by jobs in multi-core scenarios
-  **CPU_TIME**: CPU time spent
-  **EXECUTION_TIME**: time spent within executors
-  **PARSE_TIME**: time spent on parsing SQL statements
-  **PLAN_TIME**: time spent on generating plans
-  **REWRITE_TIME**: time spent on rewriting SQL statements
-  **PL_EXECUTION_TIME**: execution time of the PL/pgSQL stored procedure
-  **PL_COMPILATION_TIME**: compilation time of the PL/pgSQL stored procedure
-  **NET_SEND_TIME**: time spent on the network
-  **DATA_IO_TIME**: I/O time spent

.. table:: **Table 1** PV_INSTANCE_TIME columns

   ========= ======= ======================
   Name      Type    Description
   ========= ======= ======================
   stat_id   integer Type ID
   stat_name text    Running time type name
   value     bigint  Running time value
   ========= ======= ======================
