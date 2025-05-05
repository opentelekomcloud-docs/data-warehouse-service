:original_name: dws_04_0694.html

.. _dws_04_0694:

GS_SESSION_MEMORY_STATISTICS
============================

**GS_SESSION_MEMORY_STATISTICS** displays load management information about memory usage of ongoing complex jobs executed by the current user.

.. table:: **Table 1** GS_SESSION_MEMORY_STATISTICS columns

   +-----------------------+--------------------------+-------------------------------------------------------------------------------------------+
   | Column                | Type                     | Description                                                                               |
   +=======================+==========================+===========================================================================================+
   | datid                 | OID                      | OID of the database the backend is connected to.                                          |
   +-----------------------+--------------------------+-------------------------------------------------------------------------------------------+
   | usename               | Name                     | Username logged in to the backend.                                                        |
   +-----------------------+--------------------------+-------------------------------------------------------------------------------------------+
   | pid                   | Bigint                   | Backend thread ID.                                                                        |
   +-----------------------+--------------------------+-------------------------------------------------------------------------------------------+
   | start_time            | Timestamp with time zone | Start time of statement execution.                                                        |
   +-----------------------+--------------------------+-------------------------------------------------------------------------------------------+
   | min_peak_memory       | Integer                  | Minimum memory peak of a statement across all DNs, in MB                                  |
   +-----------------------+--------------------------+-------------------------------------------------------------------------------------------+
   | max_peak_memory       | Integer                  | Maximum memory peak of a statement across all DNs, in MB                                  |
   +-----------------------+--------------------------+-------------------------------------------------------------------------------------------+
   | spill_info            | Text                     | Spill information for the statement on all DNs. The options are:                          |
   |                       |                          |                                                                                           |
   |                       |                          | **None**: The statement has not been spilled to disks on any DNs.                         |
   |                       |                          |                                                                                           |
   |                       |                          | **All**: The statement has been spilled to disks on all DNs.                              |
   |                       |                          |                                                                                           |
   |                       |                          | **[**\ *a*\ **:**\ *b*\ **]**: The statement has been spilled to disks on *a* of *b* DNs. |
   +-----------------------+--------------------------+-------------------------------------------------------------------------------------------+
   | query                 | Text                     | Statement currently being executed.                                                       |
   +-----------------------+--------------------------+-------------------------------------------------------------------------------------------+
   | node_group            | Text                     | Logical cluster of the user running the statement.                                        |
   +-----------------------+--------------------------+-------------------------------------------------------------------------------------------+
