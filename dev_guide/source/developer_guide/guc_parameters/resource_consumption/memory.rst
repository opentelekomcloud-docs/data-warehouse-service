:original_name: dws_04_0893.html

.. _dws_04_0893:

Memory
======

This section describes memory parameters.

.. important::

   Parameters described in this section take effect only after the database service restarts.

enable_memory_limit
-------------------

**Parameter description**: Specifies whether to enable the logical memory management module.

**Type**: POSTMASTER

**Value range**: Boolean

-  **on** indicates the logic memory management module is enabled.
-  **off** indicates the logic memory management module is disabled.

**Default value**: **on**

.. important::

   If the result of **max_process_memory** - **shared_buffer** - **cstore_buffers** is less than 2 GB, GaussDB(DWS) forcibly sets **enable_memory_limit** to **off**.

.. _en-us_topic_0000001145694783__sadc1e0e8c1c246a4a6cad3967deebaad:

max_process_memory
------------------

**Parameter description**: Specifies the maximum physical memory of a database node.

**Type**: POSTMASTER

**Value range**: an integer ranging from 2 x 1024 x 1024 to INT_MAX/2. The unit is KB.

**Default value**: The value is automatically adapted by non-secondary DNs. The formula is (Physical memory size) x 0.6/(1 + Number of primary DNs). If the result is less than 2 GB, 2 GB is used by default. The default size of the secondary DN is 12 GB.

**Setting suggestions:**

On DNs, the value of this parameter is determined based on the physical system memory and the number of DNs deployed on a single node. Parameter value = (Physical memory - **vm.min_free_kbytes**) x 0.7/(n + Number of primary DNs). This parameter aims to ensure system reliability, preventing node OOM caused by increasing memory usage. **vm.min_free_kbytes** indicates OS memory reserved for kernels to receive and send data. Its value is at least 5% of the total memory. That is, **max_process_memory** = Physical memory x 0.665/(n + Number of primary DNs). If the cluster scale (number of nodes in the cluster) is smaller than 256, n=1; if the cluster scale is larger than 256 and smaller than 512, n=2; if the cluster scale is larger than 512, n=3.

Set this parameter on CNs to the same value as that on DNs.

RAM is the maximum memory allocated to the cluster.

.. _en-us_topic_0000001145694783__s9292cfbf38fa4b17b93e9a47330da753:

shared_buffers
--------------

**Parameter description**: Specifies the size of shared memory used by GaussDB(DWS). If this parameter is set to a large value, GaussDB(DWS) may require more System V shared memory than the default setting.

**Type**: POSTMASTER

**Value range**: an integer ranging from 128 to INT_MAX. The unit is 8 KB.

Changing the value of **BLCKSZ** will result in a change in the minimum value of the **shared_buffers**.

**Default value**: **512 MB** for CNs and **1 GB** for DNs. If the maximum value allowed by the OS is smaller than 32 MB, this parameter will be automatically changed to the maximum value allowed by the OS during database initialization.

**Setting suggestions:**

Set this parameter for DNs to a value greater than that for CNs, because GaussDB(DWS) pushes most of its queries down to DNs.

It is recommended that **shared_buffers** be set to a value less than 40% of the memory. Set it to a large value for row-store tables and a small value for column-store tables. For column-store tables: shared_buffers = (Memory of a single server/Number of DNs on the single server) x 0.4 x 0.25

If you want to increase the value of **shared_buffers**, you also need to increase the value of :ref:`checkpoint_segments <en-us_topic_0000001098655058__sd7daa0171e7245968cf6a44d968e1ad5>`, because a longer period of time is required to write a large amount of new or changed data.

bulk_write_ring_size
--------------------

**Parameter description**: Specifies the size of the ring buffer used for data parallel import.

**Type**: USERSET

**Value range**: an integer ranging from 16384 to INT_MAX. The unit is KB.

**Default value**: **2 GB**

**Setting suggestions**: Increase the value of this parameter on DNs if a large amount of data is to be imported.

temp_buffers
------------

**Parameter description**: Specifies the maximum size of local temporary buffers used by each database session.

**Type**: USERSET

**Value range**: an integer ranging from 800 to INT_MAX/2. The unit is KB.

**Default value**: **8 MB**

.. note::

   -  This parameter can be modified only before the first use of temporary tables within each session. Subsequent attempts to change the value of this parameter will not take effect on that session.
   -  Based on the value of **temp_buffers**, a session allocates temporary buffers as required. The cost of setting a large value in sessions that do not require many temporary buffers is only a buffer descriptor. If a buffer is used, 8192 bytes will be consumed for it.

.. _en-us_topic_0000001145694783__s7f44489cfdce4bbea287150fb7333b9e:

max_prepared_transactions
-------------------------

**Parameter description**: Specifies the maximum number of transactions that can stay in the **prepared** state simultaneously. If this parameter is set to a large value, GaussDB(DWS) may require more System V shared memory than the default setting.

When GaussDB(DWS) is deployed as an HA system, set this parameter on the standby server to the same value or a value greater than that on the primary server. Otherwise, queries will fail on the standby server.

**Type**: POSTMASTER

**Value range**: an integer ranging from 0 to 536870911. **800** indicates that the prepared transaction feature is disabled.

**Default value**: **800**

.. note::

   Set this parameter to a value greater than or equal to that of :ref:`max_connections <en-us_topic_0000001099134530__s2d671f584b5647a19255e7c6a3d116aa>` to avoid failures in preparation.

.. _en-us_topic_0000001145694783__s7be4202f202f4ccc8ecee5816cf7b2ab:

work_mem
--------

**Parameter description**: Specifies the memory used for internal sort operations and hash tables before data is written into temporary disk files. Sort operations are used for **ORDER BY**, **DISTINCT**, and merge joins. Hash tables are required for Hash joins as well as Hash-based aggregations and **IN** subqueries.

For a complex query, several sort or Hash operations may be running in parallel; each operation will be allowed to use as much memory as this value specifies. If the memory is insufficient, data is written into temporary files. In addition, several running sessions could be performing such operations concurrently. Therefore, the total memory used may be many times the value of **work_mem**.

**Type**: USERSET

**Value range**: an integer ranging from 64 to INT_MAX. The unit is KB.

**Default value**: **64 MB**

**Setting suggestions**:

If the physical memory specified by **work_mem** is insufficient, additional operator calculation data will be written into temporary tables based on query characteristics and the degree of parallelism. This reduces performance by five to ten times, and prolongs the query response time from seconds to minutes.

-  In complex serial query scenarios, each query requires five to ten associated operations. Set **work_mem** using the following formula: **work_mem** = 50% of the memory/10.
-  In simple serial query scenarios, each query requires two to five associated operations. Set **work_mem** using the following formula: **work_mem** = 50% of the memory/5.
-  For concurrent queries, use the formula: **work_mem** = **work_mem** in serialized scenario/Number of concurrent SQL statements.

query_mem
---------

**Parameter description**: Specifies the memory used by query. If the value of **query_mem** is greater than 0, the optimizer adjusts the estimated query memory to this value when generating an execution plan.

**Type**: USERSET

**Value range**: 0 or an integer greater than 32. The default unit is KB. If the value is set to a negative value or less than 32 MB, the default value **0** is used. In this case, the optimizer does not adjust the estimated query memory.

**Default value**: **0**

query_max_mem
-------------

**Parameter description**: Specifies the maximum memory that can be used by query. If the value of **query_max_mem** is greater than 0, an error is reported when the query memory usage exceeds the value.

**Type**: USERSET

**Value range**: 0 or an integer greater than 32 MB. The default unit is KB. If the value is set to a negative value or less than 32 MB, the default value **0** is used. In this case, the query memory will not be limited based on the value.

**Default value**: **0**

maintenance_work_mem
--------------------

**Parameter description:** Specifies the maximum size of memory to be used for maintenance operations, such as **VACUUM**, **CREATE INDEX**, and **ALTER TABLE ADD FOREIGN KEY**. This parameter may affect the execution efficiency of **VACUUM**, **VACUUM FULL**, **CLUSTER**, and **CREATE INDEX**.

**Type**: USERSET

**Value range**: an integer ranging from 1024 to INT_MAX. The unit is KB.

**Default value**: **128 MB**

**Setting suggestions**:

-  The value of this parameter must be greater than that of :ref:`work_mem <en-us_topic_0000001145694783__s7be4202f202f4ccc8ecee5816cf7b2ab>` so database dump files can be more quickly cleared or restored. In a database session, only one maintenance operation can be performed at a time. Maintenance is usually performed when there are not much sessions.
-  When the :ref:`Automatic Cleanup <dws_04_0923>` process is running, up to :ref:`autovacuum_max_workers <en-us_topic_0000001145814611__s502d4304994d4da5bd3cda661aab27ac>` times of this memory may be allocated. Set **maintenance_work_mem** to a value equal to or larger than the value of :ref:`work_mem <en-us_topic_0000001145694783__s7be4202f202f4ccc8ecee5816cf7b2ab>`.
-  If a large amount of data needs to be processed in the cluster, increase the value of this parameter in sessions.

psort_work_mem
--------------

**Parameter description**: Specifies the memory used for internal sort operations on column-store tables before data is written into temporary disk files. This parameter can be used for inserting tables with a partial cluster key or index, creating a table index, and deleting or updating a table.

**Type**: USERSET

.. important::

   Multiple running sessions may perform partial sorting on a table at the same time. Therefore, the total memory usage may be several times of the **psort_work_mem** value.

**Value range**: an integer ranging from 64 to INT_MAX. The unit is KB.

**Default value**: **512 MB**

max_loaded_cudesc
-----------------

**Parameter description**: Specifies the number of loaded CuDescs per column when a column-store table is scanned. Increasing the value will improve the query performance and increase the memory usage, particularly when there are many columns in the column tables.

**Type**: USERSET

**Value range**: an integer ranging from 100 to INT_MAX/2

**Default value**: **1024**

.. important::

   When the value of **max_loaded_cudesc** is set to a large value, the memory may be insufficient.

max_stack_depth
---------------

**Parameter description**: Specifies the maximum safe depth of GaussDB(DWS) execution stack. The safety margin is required because the stack depth is not checked in every routine in the server, but only in key potentially-recursive routines, such as expression evaluation.

**Type**: SUSET

**Configuration principles**:

-  The ideal value of this parameter is the maximum stack size enforced by the kernel (value of **ulimit -s**).
-  If this parameter is set to a value greater than the actual kernel limit, a running recursive function may crash an individual backend process. In an OS where GaussDB(DWS) can check the kernel limit, such as the SLES, GaussDB(DWS) will prevent this parameter from being set to a value greater than the kernel limit.
-  Since not all the OSs provide this function, you are advised to set this parameter to a specific value.

**Value range**: an integer ranging from 100 to INT_MAX. The unit is KB.

**Default value**: **2 MB**

.. note::

   **2 MB** is a small value and will not incur system breakdown in general, but may lead to execution failures of complex functions.

cstore_buffers
--------------

**Parameter description**: Specifies the size of the shared buffer used by ORC, Parquet, or CarbonData data of column-store tables and OBS or HDFS column-store foreign tables.

**Type**: POSTMASTER

**Value range**: an integer ranging from 16384 to INT_MAX. The unit is KB.

**Default value**: **32 MB**

**Setting suggestions**:

Column-store tables use the shared buffer specified by **cstore_buffers** instead of that specified by :ref:`shared_buffers <en-us_topic_0000001145694783__s9292cfbf38fa4b17b93e9a47330da753>`. When column-store tables are mainly used, reduce the value of **shared_buffers** and increase that of **cstore_buffers**.

Use **cstore_buffers** to specify the cache of ORC, Parquet, or CarbonData metadata and data for OBS or HDFS foreign tables. The metadata cache size should be 1/4 of **cstore_buffers** and not exceed 2 GB. The remaining cache is shared by column-store data and foreign table column-store data.

enable_orc_cache
----------------

**Parameter description**: Specifies whether to reserve 1/4 of **cstore_buffers** for storing ORC metadata when the cstore buffer is initialized.

**Type**: POSTMASTER

**Value range**: Boolean

**Default value**: **on**

-  **on** indicates that the ORC metadata is cached, which improves the query performance of the HDFS table but occupies the column-store buffer resources. As a result, the column-store performance is compromised.
-  **off** indicates that the ORC metadata is not cached.

schedule_splits_threshold
-------------------------

**Parameter description**: Specifies the maximum number of files that can be stored in memory when you schedule an HDFS foreign table. If the number is exceeded, all files in the list will be spilled to disk for scheduling.

**Type**: USERSET

**Value range**: an integer ranging from 1 to INT_MAX

**Default value**: **60000**

bulk_read_ring_size
-------------------

**Parameter description**: Specifies the size of the ring buffer used for data parallel export.

**Type**: USERSET

**Value range**: an integer ranging from 256 to INT_MAX. The unit is KB.

**Default value**: **16 MB**

check_cu_size_threshold
-----------------------

**Parameter description**: If the amount of data inserted to a CU is greater than the value of this parameter when data is inserted to a column-store table, the system starts row-level size verification to prevent the generation of a CU whose size is greater than 1 GB (non-compressed size).

**Type**: USERSET

**Value range**: an integer ranging from 0 to 1024. The unit is MB.

**Default value**: **1024 MB**
