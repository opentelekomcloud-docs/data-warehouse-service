:original_name: dws_04_0714.html

.. _dws_04_0714:

GS_TOTAL_NODEGROUP_MEMORY_DETAIL
================================

**GS_TOTAL_NODEGROUP_MEMORY_DETAIL** displays statistics about memory usage of the logical cluster that the current database belongs to in the unit of MB.

.. table:: **Table 1** GS_TOTAL_NODEGROUP_MEMORY_DETAIL columns

   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------+
   | Column                | Type                  | Description                                                                                                                            |
   +=======================+=======================+========================================================================================================================================+
   | ngname                | Text                  | Logical cluster name                                                                                                                   |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------+
   | memorytype            | Text                  | Memory type. The value can be:                                                                                                         |
   |                       |                       |                                                                                                                                        |
   |                       |                       | -  **ng_total_memory**: total memory of the logical cluster                                                                            |
   |                       |                       | -  **ng_used_memory**: memory usage of the logical cluster                                                                             |
   |                       |                       | -  **ng_estimate_memory**: estimated memory usage of the logical cluster                                                               |
   |                       |                       | -  **ng_foreignrp_memsize**: total memory of the external resource pool of the logical cluster                                         |
   |                       |                       | -  **ng_foreignrp_usedsize**: memory usage of the external resource pool of the logical instance                                       |
   |                       |                       | -  **ng_foreignrp_peaksize**: peak memory usage of the external resource pool of the logical cluster                                   |
   |                       |                       | -  **ng_foreignrp_mempct**: percentage of the external resource pool of the logical cluster to the total memory of the logical cluster |
   |                       |                       | -  **ng_foreignrp_estmsize**: estimated memory usage of the external resource pool of the logical cluster                              |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------+
   | memorymbytes          | Integer               | Size of allocated memory-typed memory                                                                                                  |
   +-----------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------+
