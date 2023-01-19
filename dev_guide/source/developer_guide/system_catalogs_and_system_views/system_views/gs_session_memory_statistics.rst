:original_name: dws_04_0694.html

.. _dws_04_0694:

GS_SESSION_MEMORY_STATISTICS
============================

**GS_SESSION_MEMORY_STATISTICS** displays load management information about memory usage of ongoing complex jobs executed by the current user.

.. table:: **Table 1** GS_SESSION_MEMORY_STATISTICS columns

   +-----------------------+--------------------------+---------------------------------------------------------------------------------------------------------+
   | Name                  | Type                     | Description                                                                                             |
   +=======================+==========================+=========================================================================================================+
   | datid                 | oid                      | OID of the database this backend is connected to                                                        |
   +-----------------------+--------------------------+---------------------------------------------------------------------------------------------------------+
   | usename               | name                     | Name of the user logging in to the backend                                                              |
   +-----------------------+--------------------------+---------------------------------------------------------------------------------------------------------+
   | pid                   | bigint                   | ID of a backend process                                                                                 |
   +-----------------------+--------------------------+---------------------------------------------------------------------------------------------------------+
   | start_time            | timestamp with time zone | Time when the statement starts to be executed                                                           |
   +-----------------------+--------------------------+---------------------------------------------------------------------------------------------------------+
   | min_peak_memory       | integer                  | Minimum memory peak of a statement across all DNs, in MB                                                |
   +-----------------------+--------------------------+---------------------------------------------------------------------------------------------------------+
   | max_peak_memory       | integer                  | Maximum memory peak of a statement across all DNs, in MB                                                |
   +-----------------------+--------------------------+---------------------------------------------------------------------------------------------------------+
   | spill_info            | text                     | Information about statement flushing into disks on DNs                                                  |
   |                       |                          |                                                                                                         |
   |                       |                          | **None** indicates that the statement has not been flushed to disks on any DNs.                         |
   |                       |                          |                                                                                                         |
   |                       |                          | **All** indicates that the statement has been flushed to disks on every DN.                             |
   |                       |                          |                                                                                                         |
   |                       |                          | **[**\ *a*\ **:**\ *b*\ **]** indicates that the statement has been flushed to disks on *a* of *b* DNs. |
   +-----------------------+--------------------------+---------------------------------------------------------------------------------------------------------+
   | query                 | text                     | Statement that is being executed                                                                        |
   +-----------------------+--------------------------+---------------------------------------------------------------------------------------------------------+
   | node_group            | text                     | Logical cluster of the user running the statement                                                       |
   +-----------------------+--------------------------+---------------------------------------------------------------------------------------------------------+
