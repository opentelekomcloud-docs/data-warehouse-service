:original_name: dws_04_1106.html

.. _dws_04_1106:

PGXC_MEMORY_DEBUG_INFO
======================

**PGXC_MEMORY_DEBUG_INFO** displays memory error information for each node in the current cluster when executing jobs, making it easy to locate memory error issues. When an error message "memory is temporarily unavailable" is prompted during statement execution, this view can be used to query memory error information for all nodes, which is the same as the memory error information displayed in the log. This view is supported only by clusters of version 8.3.0 or later.

.. important::

   This view only displays the most recent cluster information for errors, and repeated error information will be overwritten. If the same query requests memory multiple times and errors occur, the information will not be updated.

.. table:: **Table 1** PGXC_MEMORY_DEBUG_INFO columns

   +-----------------------+--------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   | Column                | Type                     | Description                                                                                                                         |
   +=======================+==========================+=====================================================================================================================================+
   | node_name             | Text                     | Instance name, including CNs and DNs.                                                                                               |
   +-----------------------+--------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   | query_id              | Bigint                   | ID of the query that is currently requesting memory.                                                                                |
   +-----------------------+--------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   | memory_info           | Text                     | Current instance's memory usage, including:                                                                                         |
   |                       |                          |                                                                                                                                     |
   |                       |                          | -  **process_used_memory**: memory size used by the GaussDB(DWS) process.                                                           |
   |                       |                          | -  **max_dynamic_memory**: maximum dynamic memory.                                                                                  |
   |                       |                          | -  **dynamic_used_memory**: used dynamic memory.                                                                                    |
   |                       |                          | -  **dynamic_peak_memory**: dynamic peak value of memory.                                                                           |
   |                       |                          | -  **dynamic_used_shrctx**: maximum dynamic shared memory context.                                                                  |
   |                       |                          | -  **dynamic_peak_shrctx**: dynamic peak value of shared memory context.                                                            |
   |                       |                          | -  **shared_used_memory**: used shared memory.                                                                                      |
   |                       |                          | -  **cstore_used_memory**: memory size used for column store.                                                                       |
   |                       |                          | -  **comm_used_memory**: memory size used by the communication library.                                                             |
   |                       |                          | -  **comm_peak_memory**: peak value of memory used by the communication library.                                                    |
   |                       |                          | -  **other_used_memory**: memory size used by other components.                                                                     |
   |                       |                          | -  **topsql_used_memory**: memory size used by topsql.                                                                              |
   |                       |                          | -  **large_storage_memory**: memory size used for column-store compression and decompression.                                       |
   |                       |                          | -  **os_totalmem**: total memory size of the operating system.                                                                      |
   |                       |                          | -  **os_freeemem**: remaining memory size of the operating system.                                                                  |
   +-----------------------+--------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   | summary               | Text                     | The total estimated memory consumption and actual memory consumption of jobs on the instance.                                       |
   +-----------------------+--------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   | abnormal_query        | Text                     | Thread ID and query ID with abnormal memory usage, including two cases:                                                             |
   |                       |                          |                                                                                                                                     |
   |                       |                          | #. Session with the maximum current memory usage.                                                                                   |
   |                       |                          | #. Session with the largest difference between estimated memory and actual memory usage.                                            |
   +-----------------------+--------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   | abnormal_memory       | Text                     | Memory block with the highest usage, including the maximum shared memory context usage and the maximum common memory context usage. |
   +-----------------------+--------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   | top_thread            | Text                     | Information on the top three threads with the highest memory usage:                                                                 |
   |                       |                          |                                                                                                                                     |
   |                       |                          | **context name**: memory block currently in use.                                                                                    |
   |                       |                          |                                                                                                                                     |
   |                       |                          | **contextlevel**: context level.                                                                                                    |
   |                       |                          |                                                                                                                                     |
   |                       |                          | **sessType**: type of the top-level context node.                                                                                   |
   |                       |                          |                                                                                                                                     |
   |                       |                          | **totalsize[274,13,260]MB**: total memory, released memory, and used memory size of the current memory context, in MB.              |
   +-----------------------+--------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   | create_time           | Timestamp with time zone | Time when the memory shortage error occurred.                                                                                       |
   +-----------------------+--------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
