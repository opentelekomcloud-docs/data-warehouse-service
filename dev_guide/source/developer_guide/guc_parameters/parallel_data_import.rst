:original_name: dws_04_0899.html

.. _dws_04_0899:

Parallel Data Import
====================

GaussDB(DWS) provides a parallel data import function that enables a large amount of data to be imported in a fast and efficient manner. This section describes parameters for importing data in parallel in GaussDB(DWS).

.. _en-us_topic_0000001099134526__sd65ae1e8226f4611819e91ce8b6a35fb:

raise_errors_if_no_files
------------------------

**Parameter description**: Specifies whether distinguish between the problems "the number of imported file records is empty" and "the imported file does not exist". If this parameter is set to **true** and the problem "the imported file does not exist" occurs, GaussDB(DWS) will report the error message "file does not exist".

**Type**: SUSET

**Value range**: Boolean

-  **on** indicates the messages of "the number of imported file records is empty" and "the imported file does not exist" are distinguished when files are imported.
-  **off** indicates the messages of "the number of imported file records is empty" and "the imported file does not exist" are not distinguished when files are imported.

**Default value**: **off**

.. _en-us_topic_0000001099134526__s9d2ce4e6e9ea4f6a8a8df3e3a7ddadd8:

partition_mem_batch
-------------------

**Parameter description**: To optimize the inserting of column-store partitioned tables in batches, data is cached during the inserting process and then written to the disk in batches. You can use **partition_mem_batch** to specify the number of buffers. If the value is too large, much memory will be consumed. If it is too small, the performance of inserting column-store partitioned tables in batches will deteriorate.

**Type**: USERSET

**Value range**: 1 to 65535

**Default value**: **256**

.. _en-us_topic_0000001099134526__s004b2931955e4e549caeb98b2f2723af:

partition_max_cache_size
------------------------

**Parameter description**: To optimize the inserting of column-store partitioned tables in batches, data is cached during the inserting process and then written to the disk in batches. You can use **partition_max_cache_size** to specify the size of the data buffer. If the value is too large, much memory will be consumed. If it is too small, the performance of inserting column-store partitioned tables in batches will deteriorate.

**Type**: USERSET

**Value range**: 4096 to INT_MAX/2. The minimum unit is KB.

**Default value**: **2 GB**

gds_debug_mod
-------------

**Parameter description**: Specifies whether to enable the debug function of Gauss Data Service (GDS). This parameter is used to better locate and analyze GDS faults. After the debug function is enabled, types of packets received or sent by GDS, peer end of GDS during command interaction, and other interaction information about GDS are written into the logs of corresponding nodes. In this way, state switching on the GaussDB state machine and the current state are recorded. If this function is enabled, additional log I/O resources will be consumed, affecting log performance and validity. You are advised to enable this function only when locating GDS faults.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that the GDS debug function is enabled.
-  **off** indicates that the GDS debug function is disabled.

**Default value**: **off**

enable_delta_store
------------------

**Parameter description**: This parameter has been discarded. You can set this parameter to **on** for forward compatibility, but the setting will not take effect.

For details about how to enable the delta table function of column-store tables, see the table-level parameter **enable_delta** in "CREATE TABLE" in the *SQL Syntax*.

**Type**: POSTMASTER

**Value range**: Boolean

-  **on** indicates that the delta table function of column-store tables is enabled.
-  **off** indicates that the delta table function of column-store tables is disabled.

**Default value**: **off**
