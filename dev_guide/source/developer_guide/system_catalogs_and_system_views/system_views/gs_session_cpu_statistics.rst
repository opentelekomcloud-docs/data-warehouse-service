:original_name: dws_04_0693.html

.. _dws_04_0693:

GS_SESSION_CPU_STATISTICS
=========================

**GS_SESSION_CPU_STATISTICS** displays load management information about CPU usage of ongoing complex jobs executed by the current user.

.. table:: **Table 1** GS_SESSION_CPU_STATISTICS columns

   +----------------+--------------------------+-------------------------------------------------------------------+
   | Name           | Type                     | Description                                                       |
   +================+==========================+===================================================================+
   | datid          | oid                      | OID of the database this backend is connected to                  |
   +----------------+--------------------------+-------------------------------------------------------------------+
   | usename        | name                     | Name of the user logging in to the backend                        |
   +----------------+--------------------------+-------------------------------------------------------------------+
   | pid            | bigint                   | ID of a backend process                                           |
   +----------------+--------------------------+-------------------------------------------------------------------+
   | start_time     | timestamp with time zone | Time when the statement starts to be executed                     |
   +----------------+--------------------------+-------------------------------------------------------------------+
   | min_cpu_time   | bigint                   | Minimum CPU time of the statement across all DNs. The unit is ms. |
   +----------------+--------------------------+-------------------------------------------------------------------+
   | max_cpu_time   | bigint                   | Maximum CPU time of the statement across all DNs. The unit is ms. |
   +----------------+--------------------------+-------------------------------------------------------------------+
   | total_cpu_time | bigint                   | Total CPU time of the statement across all DNs. The unit is ms.   |
   +----------------+--------------------------+-------------------------------------------------------------------+
   | query          | text                     | Statement that is being executed                                  |
   +----------------+--------------------------+-------------------------------------------------------------------+
   | node_group     | text                     | Logical cluster of the user running the statement                 |
   +----------------+--------------------------+-------------------------------------------------------------------+
