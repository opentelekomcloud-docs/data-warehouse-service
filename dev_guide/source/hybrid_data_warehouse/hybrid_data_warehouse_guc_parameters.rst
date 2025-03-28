:original_name: dws_04_1028.html

.. _dws_04_1028:

Hybrid Data Warehouse GUC Parameters
====================================

autovacuum
----------

**Parameter description**: Specifies whether to start the automatic cleanup process (**autovacuum**).

**Type**: SIGHUP

**Value range**: Boolean

-  **on** indicates the database automatic cleanup process is enabled.
-  **off** indicates that the database automatic cleanup process is disabled.

**Default value**: **on**

autovacuum_compaction_rows_limit
--------------------------------

**Parameter description**: Specifies the threshold of a small CU. A CU whose number of live tuples is less than the value of this parameter is considered as a small CU. This parameter is supported only by clusters of version 8.2.1.200 or later.

**Type**: USERSET

**Value range**: an integer ranging from -1 to 5000

**Default value**: **-1**

autovacuum_compaction_time_limit
--------------------------------

**Parameter description**: Specifies the interval for clearing small CUs. Small CUs are merged at a specified interval. This parameter is supported only by clusters of version 8.2.1.200 or later.

**Type**: SIGHUP

**Value range**: an integer ranging from 0 to 10080. The unit is minute.

**Default value**: **0**

autovacuum_max_workers
----------------------

**Parameter description**: Specifies the maximum number of autovacuum worker threads that can run at the same time. The upper limit of this parameter is related to the values of **max_connections** and **job_queue_processes**.

**Type**: SIGHUP

**Value range**: an integer

-  The minimum value is **0**, indicating that autovacuum is not automatically performed.
-  The theoretical maximum value is **262143**, and the actual maximum value dynamically changes. Formula: 262143 - **max_inner_tool_connections** - **max_connections** - **job_queue_processes** - **auxiliary threads** - **Number of autovacuum launcher threads** - 1. The number of auxiliary threads and the number of autovacuum launcher threads are specified by two macros. Their default values in the current version are **20** and **2**, respectively.

**Default value**: **4**

autovacuum_max_workers_hstore
-----------------------------

**Parameter description**: Specifies the maximum number of concurrent automatic cleanup threads used for hstore tables in **autovacuum_max_workers**.

**Type**: SIGHUP

**Value range**: an integer

**Default value**: **1**

.. note::

   To use HStore tables, set the following parameters, or the HStore performance will deteriorate severely. The recommended settings are as follows:

   **autovacuum_max_workers_hstore=3, autovacuum_max_workers=6, autovacuum=true**

enable_hstore_lightupdate
-------------------------

**Parameter description**: Specifies whether to enable lightweight UPDATE for an HStore table. (When an UPDATE operation is performed on an HStore table, the system automatically determines whether lightweight UPDATE is required.)

**Type**: SIGHUP

**Value range**: Boolean

-  **on** indicates that lightweight UPDATE is enabled for hstore tables.
-  **off** indicates that lightweight UPDATE is disabled for hstore tables.

**Default value**: **off**

enable_hstore_merge_keepgtm
---------------------------

**Parameter description**: Specifies whether the MERGE in the autovacuum operation on column-store and hstore tables occupies slots in the GTM.

**Type**: SIGHUP

**Value range**: Boolean

-  **true** indicates that it occupies slots in the GTM.
-  **false** indicates that it does not occupy slots in the GTM.

**Default value**: **true**

hstore_buffer_size
------------------

**Parameter description**: Specifies the number of HStore CU slots. The slots are used to store the update chain of each CU, which significantly improves the update and query efficiency.

To prevent excessive memory usage, the system calculates a slot value based on the memory size, compares the slot value with the value of this parameter, and uses the smaller value of the two.

**Type**: POSTMASTER

**Value range**: an integer ranging from 100 to 100000

**Default value**: **true**

gtm_option
----------

**Parameter description**: Specifies the GTM running mode in GaussDB(DWS). This parameter is supported by version 8.2.1 or later clusters.

-  GTM mode: In this mode, the GTM manages running transactions and allocates XIDs and CSNs in a unified manner.
-  GTM-Lite mode: The GTM is only responsible for XID allocation and CSN update, and is no longer responsible for global transaction management. The GTM-Lite mode applies to TP scenarios with high concurrency and short queries. It can improve query performance while ensuring transaction consistency.
-  GTM-Free mode: For details, see the description of **enable_gtm_free**.

**Type**: POSTMASTER

**Value range**: enumerated values

-  **gtm** or **0**: The GTM mode is enabled.
-  **gtm-lite** or **1**: The GTM-Lite mode is enabled.
-  **gtm-free** or **2**: The GTM-Free mode starts.

**Default value**: **gtm**

.. important::

   #. Both GaussDB(DWS) and GTM have the **gtm_option** parameter with the same meaning. For GTM and GTM-Lite, the same mode must be set on GaussDB(DWS) and GTM. Otherwise, service errors may occur.
   #. The GTM-Free mode can be enabled by setting **enable_gtm_free** to **on** or **gtm_option** to **gtm-free**.
   #. To set the non-GTM-Free modes, set **enable_gtm_free** to **off**.
   #. The GTM-Free mode takes effect only in hybrid cloud and ESL scenarios.

defer_xid_cleanup_time
----------------------

**Parameter description**: Specifies the global OldestXmin maintenance period in GTM-Lite mode in the hybrid data warehouse. In each maintenance period, the CCN or FCN collects and delivers the values of global **OldestXmin**. This parameter is supported by version 8.2.1 or later clusters.

This parameter takes effect only in GTM-Lite mode. You are advised not to modify this parameter.

**Type**: SIGHUP

**Value range**: an integer ranging from 1 to INT_MAX. The unit is ms.

**Default value**: **5,000**.
