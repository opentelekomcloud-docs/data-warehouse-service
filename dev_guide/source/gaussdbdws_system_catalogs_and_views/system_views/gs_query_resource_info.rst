:original_name: dws_04_1409.html

.. _dws_04_1409:

GS_QUERY_RESOURCE_INFO
======================

The **GS_QUERY_RESOURCE_INFO** view displays the resource information about all running jobs on the current DN. This parameter is supported only by clusters of version 9.1.0 or later.

.. note::

   This view can be queried only on DNs. It is used only for O&M operations to locate faults. You are advised not to use this function.

.. table:: **Table 1** GS_QUERY_RESOURCE_INFO

   +-------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Column      | Type   | Description                                                                                                                                                    |
   +=============+========+================================================================================================================================================================+
   | node_name   | Text   | Instance name, which contains only DNs                                                                                                                         |
   +-------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | user_id     | OID    | User ID.                                                                                                                                                       |
   +-------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | queryid     | Bigint | Internal query ID used for statement execution.                                                                                                                |
   +-------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | used_mem    | Int    | Memory used by the statement on the current DN. The unit is MB.                                                                                                |
   +-------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | cpu_time    | Bigint | CPU time of a statement on the current DN. The unit is ms.                                                                                                     |
   +-------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | used_cpu    | Double | Number of CPUs used by the statement on the current DN.                                                                                                        |
   +-------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | spill_size  | Bigint | Amount of data spilled to disks on the current DN. The default value is **0**. The unit is MB.                                                                 |
   +-------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | read_bytes  | Bigint | Number of logical read bytes used by the statement on the current DN. The unit is KB.                                                                          |
   +-------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | write_bytes | Bigint | Number of logical write bytes used by the statement on the current DN. The unit is KB.                                                                         |
   +-------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | read_count  | Bigint | Number of logical reads used by the statement on the current DN.                                                                                               |
   +-------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | write_count | Bigint | Number of logical writes used by the statement on the current DN.                                                                                              |
   +-------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | read_speed  | Int    | Logical read rate used by the statement on the current DN. The unit is KB/s.                                                                                   |
   +-------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | write_speed | Int    | Logical write rate used by the statement on the current DN. The unit is KB/s.                                                                                  |
   +-------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | curr_iops   | Int    | I/O operations per second of the statement on the current DN. It is recorded as a count in a column-store table and as a count of 10,000 in a row-store table. |
   +-------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | send_pkg    | Bigint | Total number of communication packages sent by a statement across all DNs.                                                                                     |
   +-------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | recv_pkg    | Bigint | Total number of communication packages received by a statement across all DNs.                                                                                 |
   +-------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | send_bytes  | Bigint | Total sent data of the statement stream, in byte.                                                                                                              |
   +-------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | recv_bytes  | Bigint | Total received data of the statement stream, in byte.                                                                                                          |
   +-------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | send_speed  | Int    | Network sending rate of the statement on the current DN. The unit is KB/s.                                                                                     |
   +-------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | recv_speed  | Int    | Network receiving rate of the statement on the current DN. The unit is KB/s.                                                                                   |
   +-------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
