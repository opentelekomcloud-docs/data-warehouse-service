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

autovacuum_max_workers
----------------------

**Parameter description**: Specifies the maximum number of autovacuum worker threads that can run at the same time. The upper limit of this parameter is related to the values of **max_connections** and **job_queue_processes**.

**Type**: SIGHUP

**Value range**: an integer

-  The minimum value is **0**, indicating that autovacuum is not automatically performed.
-  The theoretical maximum value is **262143**, and the actual maximum value dynamically changes. Formula: 262143 - **max_inner_tool_connections** - **max_connections** - **job_queue_processes** - **auxiliary threads** - **Number of autovacuum launcher threads** - 1. The number of auxiliary threads and the number of autovacuum launcher threads are specified by two macros. Their default values in the current version are **20** and **2**, respectively.

**Default value**: **3**

autovacuum_max_workers_hstore
-----------------------------

**Parameter description**: Specifies the maximum number of concurrent automatic cleanup threads used for hstore tables in **autovacuum_max_workers**.

**Type**: SIGHUP

**Value range**: an integer

**Default value**: **0**

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

**Value range**: an integer ranging from 100 to 10,000,000

**Default value**: **100,000**
