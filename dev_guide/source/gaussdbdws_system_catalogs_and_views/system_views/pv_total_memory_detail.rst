:original_name: dws_04_0855.html

.. _dws_04_0855:

PV_TOTAL_MEMORY_DETAIL
======================

**PV_TOTAL_MEMORY_DETAIL** displays statistics about memory usage of the current database node in the unit of MB.

.. table:: **Table 1** PV_TOTAL_MEMORY_DETAIL columns

   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------+
   | Name                  | Type                  | Description                                                                                |
   +=======================+=======================+============================================================================================+
   | nodename              | Text                  | Node name                                                                                  |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------+
   | memorytype            | Text                  | Memory type. Its value can be:                                                             |
   |                       |                       |                                                                                            |
   |                       |                       | -  **max_process_memory**: memory used by a GaussDB(DWS) cluster instance                  |
   |                       |                       | -  **process_used_memory**: memory used by a GaussDB(DWS) process                          |
   |                       |                       | -  **max_dynamic_memory**: maximum dynamic memory                                          |
   |                       |                       | -  **dynamic_used_memory**: used dynamic memory                                            |
   |                       |                       | -  **dynamic_peak_memory**: dynamic peak value of the memory                               |
   |                       |                       | -  **dynamic_used_shrctx**: maximum dynamic shared memory context                          |
   |                       |                       | -  **dynamic_peak_shrctx**: dynamic peak value of the shared memory context                |
   |                       |                       | -  **max_shared_memory**: maximum shared memory                                            |
   |                       |                       | -  **shared_used_memory**: used shared memory                                              |
   |                       |                       | -  **max_cstore_memory**: maximum memory allowed for column store                          |
   |                       |                       | -  **cstore_used_memory**: memory used for column store                                    |
   |                       |                       | -  **max_sctpcomm_memory**: maximum memory allowed for the communication library           |
   |                       |                       | -  **sctpcomm_used_memory**: memory used for the communication library                     |
   |                       |                       | -  **sctpcomm_peak_memory**: memory peak of the communication library                      |
   |                       |                       | -  **other_used_memory**: other used memory                                                |
   |                       |                       | -  **gpu_max_dynamic_memory**: maximum GPU memory                                          |
   |                       |                       | -  **gpu_dynamic_used_memory**: sum of the available GPU memory and temporary GPU memory   |
   |                       |                       | -  **gpu_dynamic_peak_memory**: maximum memory used for GPU                                |
   |                       |                       | -  **pooler_conn_memory**: memory used for pooler connections                              |
   |                       |                       | -  **pooler_freeconn_memory**: memory used for idle pooler connections                     |
   |                       |                       | -  **storage_compress_memory**: memory used for column-store compression and decompression |
   |                       |                       | -  **udf_reserved_memory**: memory reserved for the **UDF Worker** process                 |
   |                       |                       | -  **mmap_used_memory**: memory used for **mmap**                                          |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------+
   | memorymbytes          | Integer               | Size of allocated memory-typed memory                                                      |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------+
