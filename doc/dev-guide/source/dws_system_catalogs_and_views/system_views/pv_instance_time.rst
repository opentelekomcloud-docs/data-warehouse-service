:original_name: dws_04_0849.html

.. _dws_04_0849:

PV_INSTANCE_TIME
================

**PV_INSTANCE_TIME** collects statistics on the running time of processes and the time consumed in each execution phase, in microseconds.

**PV_INSTANCE_TIME** records time consumption information of the current node. The time consumption information is classified into the following types:

-  **DB_TIME**: time spent by a process on multi-core parallel computing
-  **CPU_TIME**: time spent by a process on the CPU core
-  **EXECUTION_TIME**: time spent on the executor
-  **PARSE_TIME**: time spent on parsing SQL statements
-  **PLAN_TIME**: time spent on generating plans
-  **REWRITE_TIME**: time spent on rewriting SQL statements
-  **PL_EXECUTION_TIME**: execution time of the PL/pgSQL stored procedure
-  **PL_COMPILATION_TIME**: compilation time of the PL/pgSQL stored procedure
-  **NET_SEND_TIME**: time spent on the network
-  **DATA_IO_TIME**: time spent on I/O

.. table:: **Table 1** PV_INSTANCE_TIME columns

   ========= ======= =========================
   Column    Type    Description
   ========= ======= =========================
   stat_id   Integer Type ID.
   stat_name Text    Name of the runtime type.
   value     Bigint  Runtime value.
   ========= ======= =========================
