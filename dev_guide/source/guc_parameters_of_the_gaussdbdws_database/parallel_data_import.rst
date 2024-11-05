:original_name: dws_04_0899.html

.. _dws_04_0899:

Parallel Data Import
====================

GaussDB(DWS) provides a parallel data import function that enables a large amount of data to be imported in a fast and efficient manner. This section describes parameters for importing data in parallel in GaussDB(DWS).

.. _en-us_topic_0000001460722804__sd65ae1e8226f4611819e91ce8b6a35fb:

raise_errors_if_no_files
------------------------

**Parameter description**: Specifies whether distinguish between the problems "the number of imported file records is empty" and "the imported file does not exist". If this parameter is set to **true** and the problem "the imported file does not exist" occurs, GaussDB(DWS) will report the error message "file does not exist".

**Type**: SUSET

**Value range**: Boolean

-  **on** indicates the messages of "the number of imported file records is empty" and "the imported file does not exist" are distinguished when files are imported.
-  **off** indicates the messages of "the number of imported file records is empty" and "the imported file does not exist" are not distinguished when files are imported.

**Default value**: **off**

.. _en-us_topic_0000001460722804__s004b2931955e4e549caeb98b2f2723af:

partition_max_cache_size
------------------------

**Parameter description**: To optimize the inserting of column-store partitioned tables in batches, data is cached during the inserting process and then written to the disk in batches. You can use **partition_max_cache_size** to specify the size of the data buffer. If the value is too large, much memory will be consumed. If it is too small, the performance of inserting column-store partitioned tables in batches will deteriorate.

**Type**: USERSET

**Value range**: 4096 to INT_MAX/2. The minimum unit is KB.

**Default value**: **2 GB**

.. _en-us_topic_0000001460722804__section89951118396:

partition_mem_batch
-------------------

**Parameter description**: To optimize the performance of batch insert into column-store partitioned tables, data is cached during the inserting process and then written to the disk in batches. If **partition_max_cache_size** is configured, **partition_mem_batch** can be used to specify the number of caches. If this parameter is set to a large value, the available cache of each partition will be small, and the performance of batch insert into column-store partitioned tables will deteriorate. If this parameter is set to a small value, the available cache of each partition will be large, consuming much system memory.

**Type**: USERSET

**Value range**: 1 to 65535

**Default value**: **256**

gds_debug_mod
-------------

**Parameter description**: Specifies whether to enable the debug function of Gauss Data Service (GDS). This parameter is used to better locate and analyze GDS faults. After the debug function is enabled, types of packets received or sent by GDS, peer end of GDS during command interaction, and other interaction information about GDS are written into the logs of corresponding nodes. In this way, state switching on the GaussDB state machine and the current state are recorded. If this function is enabled, additional log I/O resources will be consumed, affecting log performance and validity. You are advised to enable this function only when locating GDS faults.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that the GDS debug function is enabled.
-  **off** indicates that the GDS debug function is disabled.

**Default value**: **off**

max_copy_data_display
---------------------

**Parameter description**: GUC control added for the length of the **rawrecord** field in the copy error table, in the text type. The maximum value is 1 GB minus 8203 bytes (that is, 1073733621 bytes). This parameter is supported only by clusters of versions 8.2.1.100 or later.

When this parameter is set, it indicates the maximum number of characters that can be displayed. If the number of characters exceeds the maximum, an ellipsis (...) is displayed at the end.

**Type**: USERSET

**Value range**: 0 to 1073733616

**Default value**: **1024**
