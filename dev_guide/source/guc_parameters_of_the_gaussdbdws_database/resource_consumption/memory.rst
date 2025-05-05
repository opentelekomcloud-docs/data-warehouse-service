:original_name: dws_04_0893.html

.. _dws_04_0893:

Memory
======

This section describes memory parameters.

.. important::

   Parameters described in this section take effect only after the database service restarts.

.. _en-us_topic_0000001811610373__sadc1e0e8c1c246a4a6cad3967deebaad:

max_process_memory
------------------

**Parameter description**: Specifies the maximum physical memory of a database node.

**Type**: SIGHUP

**Value range**: an integer ranging from 2 x 1024 x 1024 to INT_MAX/2. The unit is KB.

**Default value**: Determined based on non-secondary DNs. If multiple DNs are deployed on a server, the value is (Physical memory size) x 0.8/(1 + Number of primary DNs). If a single DN is deployed on a server, the value is (Physical memory size) x 0.6. If the calculation result is less than 2 GB, the value is 2 GB by default. The default size of the secondary DN is 12 GB.

**Setting suggestions:**

-  On DNs, the value of this parameter is determined based on the physical system memory and the number of DNs deployed on a single node. If multiple DNs are deployed on a server, the calculation formula for the **max_process_memory** value is as follows: (Physical memory size - **vm.min_free_kbytes**) x 0.8/(n + Number of primary DNs). If only one DN is deployed on a server, the calculation formula for the **max_process_memory** value is (Physical memory size - **vm.min_free_kbytes**) x 0.6. This parameter aims to ensure system reliability, preventing node OOM caused by increasing memory usage. **vm.min_free_kbytes** indicates OS memory reserved for kernels to receive and send data. Its value is at least 5% of the total memory. That is, **max_process_memory** = Physical memory x 0.8/ (n + Number of primary DNs). If the cluster scale (number of nodes in the cluster) is smaller than 256, n=1; if the cluster scale is larger than 256 and smaller than 512, n=2; if the cluster scale is larger than 512, n=3.
-  You are not advised to set this parameter to the minimum threshold.
-  Set this parameter on CNs to the same value as that on DNs.
-  RAM is the maximum memory allocated to the cluster.
-  In GaussDB(DWS) 8.2.0 and later versions, the initial value of **max_process_memory** is increased to improve memory resource utilization. However, in an unbalanced cluster where a server has two primary DNs running, using the initial value of **max_process_memory** may cause OOM. In 8.2.0 and later versions, the **max_process_memory** parameter is changed to the SIGHUP type and can be manually adjusted. The **max_process_memory_auto_adjust** parameter is added. If a cluster is unbalanced, its CM will dynamically adjust **max_process_memory** based on the cluster status. The value of **max_process_memory** is (Physical memory - **vm.min_free_kbytes**) x 0.8/Number of primary DNs.
-  In GaussDB(DWS) 8.2.1 or later, the application scope of dynamically adjusting the value of **max_process_memory** is expanded from clusters where each server has only one DN to all cluster deployment modes.

   -  If **max_process_memory_auto_adjust** is set to **on**, the value of **max_process_memory** is dynamically adjusted between the upper limit and the lower limit. The lower limit is calculated as follows: (Physical memory size) x 0.8/(1 + Number of primary DNs). The upper limit is specified by the GUC parameter **max_process_memory_balanced**. (For details about how to set **max_process_memory_balanced**, contact technical support.)
   -  When the cluster works in load balancing mode, the upper limit of **max_process_memory** is used to improve the overall memory usage of the node. Compared with earlier versions, the memory usage is improved.
   -  When the cluster is not in load balancing mode, the lower limit of **max_process_memory** is used. The overall memory usage of the node is the same as that in versions earlier than 8.2.1.
   -  In upgrade scenarios, to ensure forward compatibility, the system does not set **max_process_memory_balanced**, and **max_process_memory** uses the value set before the upgrade by default.

max_process_memory_auto_adjust
------------------------------

**Parameter description**: Specifies whether to enable automatic adjustment for **max_process_memory** parameter. (This parameter is supported only by cluster versions 8.2.0 and later.) In a cluster where each server only has one DN, if this function is enabled, the CM dynamically adjusts the value of **max_process_memory** on the corresponding DN during an active/standby switchover.

**Type**: SIGHUP

**Value range**: Boolean

**Default value**: **on**

**Suggestion**: Set this parameter to **on**. For a cluster where each server only has one DN, the initial value of **max_process_memory** is increased in 8.2.0 and later versions to improve memory resource utilization. However, after a primary/standby switchover, there will be two primary DNs running on the same server. Using the initial value of **max_process_memory** in this case may cause OOM, and you need to let the CM dynamically adjust the value.

.. _en-us_topic_0000001811610373__s9292cfbf38fa4b17b93e9a47330da753:

shared_buffers
--------------

**Parameter description**: Specifies the size of shared memory used by GaussDB(DWS). If this parameter is set to a large value, GaussDB(DWS) may require more System V shared memory than the default setting.

**Type**: POSTMASTER

**Value range**: an integer ranging from 128 to INT_MAX. The unit is 8 KB.

Changing the value of **BLCKSZ** will result in a change in the minimum value of the **shared_buffers**.

**Default value**: The value of this parameter for CNs is half of that for DNs, which is calculated using the formula **POWER(2,ROUND(LOG(2,max_process_memory/18),0))**. If the maximum value allowed by the OS is smaller than 32 MB, this parameter will be automatically changed to the maximum value allowed by the OS during database initialization.

**Setting suggestions:**

You are advised to set this parameter for DNs to a value greater than that for CNs, because GaussDB(DWS) pushes its most queries down to DNs.

It is recommended that **shared_buffers** be set to a value less than 40% of the memory. Set it to a large value for row-store tables and a small value for column-store tables. For column-store tables: shared_buffers = (Memory of a single server/Number of DNs on the single server) x 0.4 x 0.25

If you want to increase the value of **shared_buffers**, you also need to increase the value of **checkpoint_segments**, because a longer period of time is required to write a large amount of new or changed data.

bulk_write_ring_size
--------------------

**Parameter description**: Specifies the size of the ring buffer used for data parallel import.

**Type**: USERSET

**Value range**: an integer ranging from 16384 to INT_MAX. The unit is KB.

**Default value**: **2 GB**

**Setting suggestions**: Increase the value of this parameter on DNs if a huge amount of data is to be imported.

buffer_ring_ratio
-----------------

**Parameter description**: ring buffer threshold for parallel data export

**Type**: USERSET

**Value range**: integer in the range 1-1000

**Default value**: 250

.. note::

   -  The default value indicates that the threshold is 250/1000 (a quarter) of **shared_buffers**.
   -  The minimum value is 1/1000 of the value of **shared_buffers**.
   -  The maximum value is the value of **shared_buffers**.

**Setting suggestions**: If the cache hit ratio is not as expected during export, you are advised to configure this parameter on DNs.

enable_cstore_ring_buffer
-------------------------

**Parameter description**: Specifies whether to enable column-store RingBuffer. This parameter is supported only by cluster versions 8.2.0 and later.

**Type**: USERSET

**Value range**: Boolean

**Default value**: **off**

**Suggestion**: If workloads have been running for a period of time, a large amount of frequently queried data has been stored in the CStoreBuffer, and you want to query large tables that are rarely accessed, you are advised to enable this function before the query and disable it after the query.

temp_buffers
------------

**Parameter description**: Specifies the maximum size of local temporary buffers used by each database session.

**Type**: USERSET

**Value range**: an integer ranging from 800 to INT_MAX/2. The unit is 8 KB.

**Default value**: **8 MB**

.. note::

   -  This parameter can be modified only before the first use of temporary tables within each session. Subsequent attempts to change the value of this parameter will not take effect on that session.
   -  Based on the value of **temp_buffers**, a session allocates temporary buffers as required. The cost of setting a large value in sessions that do not require many temporary buffers is only a buffer descriptor. If a buffer is used, 8192 bytes will be consumed for it.

.. _en-us_topic_0000001811610373__s7f44489cfdce4bbea287150fb7333b9e:

max_prepared_transactions
-------------------------

**Parameter description**: Specifies the maximum number of transactions that can stay in the **prepared** state simultaneously. If this parameter is set to a large value, GaussDB(DWS) may require more System V shared memory than the default setting.

When GaussDB(DWS) is deployed as an HA system, set this parameter on the standby server to the same value or a value greater than that on the primary server. Otherwise, queries will fail on the standby server.

**Type**: POSTMASTER

**Value range**: an integer ranging from 0 to 536870911. The value of CN set to **0** indicates that the prepared transaction feature is disabled.

**Default value**: **800** for CNs and **5000** for DNs

.. note::

   Set this parameter to a value greater than or equal to that of :ref:`max_connections <en-us_topic_0000001764491936__s2d671f584b5647a19255e7c6a3d116aa>` to avoid failures in preparation.

.. _en-us_topic_0000001811610373__s7be4202f202f4ccc8ecee5816cf7b2ab:

work_mem
--------

**Parameter description**: Specifies the memory capacity to be used by internal sort operations and Hash tables before writing to temporary disk files. Sort operations are used for **ORDER BY**, **DISTINCT**, and merge joins. Hash tables are required for Hash joins as well as Hash-based aggregations and **IN** subqueries.

For a complex query, several sort or Hash operations may be running in parallel; each operation will be allowed to use as much memory as this value specifies. If the memory is insufficient, data is written into temporary files. In addition, several running sessions could be performing such operations concurrently. Therefore, the total memory used may be many times the value of **work_mem**.

**Type**: USERSET

**Value range**: an integer ranging from 64 to INT_MAX. The unit is KB.

**Default value**: 512 MB for small-scale memory and 2 GB for large-scale memory (If :ref:`max_process_memory <en-us_topic_0000001811610373__sadc1e0e8c1c246a4a6cad3967deebaad>` is greater than or equal to 30 GB, it is large-scale memory. Otherwise, it is small-scale memory.)

**Setting suggestions:**

If the physical memory specified by **work_mem** is insufficient, additional operator calculation data will be written into temporary tables based on query characteristics and the degree of parallelism. This reduces performance by five to ten times, and prolongs the query response time from seconds to minutes.

-  In complex serial query scenarios, each query requires five to ten associated operations. Set **work_mem** using the following formula: **work_mem** = 50% of the memory/10.
-  In simple serial query scenarios, each query requires two to five associated operations. Set **work_mem** using the following formula: **work_mem** = 50% of the memory/5.
-  For concurrent queries, use the formula: **work_mem** = **work_mem** in serialized scenario/Number of concurrent SQL statements.

   .. important::

      Once memory adaptation is enabled, there is no need to use **work_mem** to optimize operator memory usage after collecting statistics. The system generates a plan for each statement and estimates the memory usage of each operator and the entire statement based on the current workload. The system then schedules the queue based on the workload and the overall memory usage of the statement, which can result in statement queuing in high-concurrency scenarios.

query_mem
---------

**Parameter description**: Specifies the memory used by query. If the value of **query_mem** is greater than 0, the optimizer adjusts the estimated query memory to this value when generating an execution plan.

**Type**: USERSET

**Value range**: **0** or an integer greater than 32 MB. The default unit is KB. If the value is set to a negative value or less than 32 MB, the default value **0** is used. In this case, the optimizer does not adjust the estimated query memory.

**Default value**: **0**

query_max_mem
-------------

**Parameter description**: Specifies the maximum memory that can be used by query. If the value of **query_max_mem** is greater than 0, when generating an execution plan, the optimizer uses this value to set the available memory for operators. If job memory usage exceeds the value of this parameter, an error is reported and the job exits.

**Type**: USERSET

**Value range**: **0** or an integer greater than 32 MB. The default unit is KB. If the value is less than 32 MB, the system automatically sets this parameter to the default value **0**. In this case, the optimizer does not limit the memory usage of jobs.

**Default value**: **0**

agg_max_mem
-----------

**Parameter description**: Specifies the maximum memory that can be used by the Agg operator when the number of aggregation columns exceeds 5. This parameter takes effect only if the value of **agg_max_mem** is greater than 0. (This parameter is supported only in 8.1.3.200 and later cluster versions.)

**Type**: USERSET

**Value range**: **0** or an integer greater than 32 MB. The default unit is KB. If the value is less than 32 MB, the system automatically sets this parameter to the default value **0**. In this case, the memory usage of the Agg operator is not limited based on the value.

**Default value**:

-  If the current cluster is upgraded from an earlier version to 8.1.3 or later, the value in the earlier version is inherited. The default value is **INT_MAX**.
-  If the current cluster version is 8.1.3 or later, the default value is **2GB**.

enable_rowagg_memory_control
----------------------------

**Parameter description**: Specifies the upper limit of the memory used by the row-store agg operator.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that the memory usage limit of the row-store agg operator is enabled. Setting this parameter to **on** can avoid OOM caused by the row-store agg operator, but may affect the agg performance.
-  **off** indicates that the memory usage limit of the row-store agg operator is disabled. If this parameter is set to **off**, the system memory may be unavailable.

**Default value**: **on**

.. _en-us_topic_0000001811610373__sfbfa78b6871442cb85a84a425335ce38:

maintenance_work_mem
--------------------

**Parameter description:** Specifies the maximum size of memory to be used for maintenance operations, such as **VACUUM**, **CREATE INDEX**, and **ALTER TABLE ADD FOREIGN KEY**. This parameter may affect the execution efficiency of VACUUM, VACUUM FULL, CLUSTER, and CREATE INDEX.

**Type**: USERSET

**Value range**: an integer ranging from 1024 to INT_MAX. The unit is KB.

**Default value**: 512 MB for small-scale memory and 2 GB for large-scale memory (If :ref:`max_process_memory <en-us_topic_0000001811610373__sadc1e0e8c1c246a4a6cad3967deebaad>` is greater than or equal to 30 GB, it is large-scale memory. Otherwise, it is small-scale memory.)

**Setting suggestions:**

-  You are advised to set this parameter to the same value of :ref:`work_mem <en-us_topic_0000001811610373__s7be4202f202f4ccc8ecee5816cf7b2ab>` so that database dump can be cleared or restored more quickly. In a database session, only one maintenance operation can be performed at a time. Maintenance is usually performed when there are not much sessions.
-  When the :ref:`Automatic Cleanup <dws_04_0923>` process is running, up to :ref:`autovacuum_max_workers <en-us_topic_0000001764650468__s502d4304994d4da5bd3cda661aab27ac>` times of this memory may be allocated. Set **maintenance_work_mem** to a value equal to or larger than the value of :ref:`work_mem <en-us_topic_0000001811610373__s7be4202f202f4ccc8ecee5816cf7b2ab>`.
-  If a large amount of data needs to be processed in the cluster, increase the value of this parameter in sessions.

psort_work_mem
--------------

**Parameter description**: Specifies the memory used for internal sort operations on column-store tables before they are written into temporary disk files. This parameter can be used for inserting tables having a partial cluster key or index, creating a table index, and deleting or updating a table.

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

**Take the following into consideration when setting this parameter:**

-  The ideal value of this parameter is the maximum stack size enforced by the kernel (value of **ulimit -s**).
-  Setting this parameter to a value larger than the actual kernel limit means that a running recursive function may crash an individual backend process. In an OS where GaussDB(DWS) can check the kernel limit, such as the SLES, GaussDB(DWS) will prevent this parameter from being set to a value greater than the kernel limit.
-  Since not all the OSs provide this function, you are advised to set a specific value for this parameter.

**Value range**: an integer ranging from 100 to INT_MAX. The unit is KB.

**Default value**: **2 MB**

.. note::

   **2 MB** is a small value and will not incur system breakdown in general, but may lead to execution failures of complex functions.

cstore_buffers
--------------

**Parameter description**: Specifies the size of the shared buffer used by ORC, Parquet, or CarbonData data of column-store tables and OBS or HDFS column-store foreign tables.

**Type**: POSTMASTER

**Value range**: an integer ranging from 16384 to INT_MAX. The unit is KB.

**Default value**: The value of this parameter for CNs is 32 MB, while that for DNs is calculated using the formula **POWER(2,ROUND(LOG(2,max_process_memory/18),0))**.

**Setting suggestions:**

Column-store tables use the shared buffer specified by **cstore_buffers** instead of that specified by :ref:`shared_buffers <en-us_topic_0000001811610373__s9292cfbf38fa4b17b93e9a47330da753>`. When column-store tables are mainly used, reduce the value of **shared_buffers** and increase that of **cstore_buffers**.

Use **cstore_buffers** to specify the cache of ORC, Parquet, or CarbonData metadata and data for OBS or HDFS foreign tables. The metadata cache size should be 1/4 of **cstore_buffers** and not exceed 2 GB. The remaining cache is shared by column-store data and foreign table column-store data.

dfs_max_memory
--------------

**Parameter description**: Specifies the maximum memory that can be occupied during ORC export. If the memory is insufficient when a wide table is exported, increase the value of this parameter and try again. This parameter is supported only by clusters of version 8.3.0 or later.

**Type**: USERSET

**Value range**: an integer ranging from 131072 to 10485760. The unit is KB.

**Default value**: 262144 KB (256 MB)

schedule_splits_threshold
-------------------------

**Parameter description**: Specifies the maximum number of files that can be stored in memory when you schedule an HDFS foreign table. If the number is exceeded, all files in the list will be spilled to disk for scheduling.

**Type**: USERSET

**Value range**: an integer ranging from 1 to INT_MAX

**Default value**: **60000**

bulk_read_ring_size
-------------------

**Parameter description**: Specifies the ring buffer size used for data parallel export.

**Type**: USERSET

**Value range**: an integer ranging from 256 to INT_MAX. The unit is KB.

**Default value**: **16 MB**

check_cu_size_threshold
-----------------------

**Parameter description**: When inserting data into a column-store table, if the amount of data already inserted in a CU exceeds the value of this parameter, row-level size verification will be performed to prevent the creation of uncompressed CUs larger than 1 GB.

**Type**: USERSET

**Value range**: an integer ranging from 0 to 1048576. The unit is MB.

**Default value:** 1 GB

.. important::

   If row-level size verification fails multiple times, you are advised to temporarily set the parameter to **0** at the session level.

memory_spread_strategy
----------------------

**Parameter description**: Specifies the DN memory expansion policy of a customized resource pool. You are advised to set this parameter for services with sufficient memory. After this parameter is set, the query performance is improved. The maximum memory can be the same as that of the default resource pool. However, there may be errors caused by insufficient memory in some service scenarios if the memory is small. This parameter is supported only by clusters of version 8.1.3 or later.

**Type**: USERSET

**Value range**: enumerated values

-  **none**: indicates that the memory is not expanded.
-  **negative**: indicates that the memory and operators are expanded based on the estimated usage.
-  **crazy**: indicates that the memory is directly expanded, which is equivalent to the memory expansion policy of the default resource pool. However, there may be errors caused by insufficient memory in some service scenarios if the memory is small.

**Default value**: **none**

async_io_acc_max_memory
-----------------------

**Parameter description**: Specifies the maximum memory that can be used for asynchronous read/write acceleration in a single task thread. This parameter is supported only by clusters of version 9.0.0 or later.

**Type**: USERSET

**Value range**: an integer ranging from 4096 to **INT_MAX/2**, in KB.

**Default value**: **128MB**
