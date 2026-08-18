:original_name: dws_04_0693.html

.. _dws_04_0693:

GS_SESSION_CPU_STATISTICS
=========================

**GS_SESSION_CPU_STATISTICS** displays load management information about CPU usage of ongoing complex jobs executed by the current user.

.. table:: **Table 1** GS_SESSION_CPU_STATISTICS columns

   +----------------+--------------------------+-----------------------------------------------------------------+
   | Column         | Type                     | Description                                                     |
   +================+==========================+=================================================================+
   | datid          | OID                      | OID of the database the backend is connected to.                |
   +----------------+--------------------------+-----------------------------------------------------------------+
   | usename        | Name                     | Username logged in to the backend.                              |
   +----------------+--------------------------+-----------------------------------------------------------------+
   | pid            | Bigint                   | Backend thread ID.                                              |
   +----------------+--------------------------+-----------------------------------------------------------------+
   | start_time     | Timestamp with time zone | Start time of statement execution.                              |
   +----------------+--------------------------+-----------------------------------------------------------------+
   | min_cpu_time   | Bigint                   | Minimum CPU time of a statement across all DNs. The unit is ms. |
   +----------------+--------------------------+-----------------------------------------------------------------+
   | max_cpu_time   | Bigint                   | Maximum CPU time of a statement across all DNs. The unit is ms. |
   +----------------+--------------------------+-----------------------------------------------------------------+
   | total_cpu_time | Bigint                   | Total CPU time of a statement across all DNs. The unit is ms.   |
   +----------------+--------------------------+-----------------------------------------------------------------+
   | query          | Text                     | Statement currently being executed.                             |
   +----------------+--------------------------+-----------------------------------------------------------------+
   | node_group     | Text                     | Logical cluster of the user running the statement.              |
   +----------------+--------------------------+-----------------------------------------------------------------+
