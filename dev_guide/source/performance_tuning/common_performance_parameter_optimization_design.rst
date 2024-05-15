:original_name: dws_04_0970.html

.. _dws_04_0970:

Common Performance Parameter Optimization Design
================================================

To improve the cluster performance, you can use multiple methods to optimize the database, including hardware configuration, software driver upgrade, and internal parameter adjustment of the database. This section describes some common parameters and recommended configurations.

#. query_dop

   Set the user-defined query parallelism degree.

   The SMP architecture uses abundant resources to obtain time. After the plan parallelism is executed, more resources are consumed, including the CPU, memory, I/O, and network bandwidth. As the DOP grows, the resource consumption increases.

   -  When resources become a bottleneck, the SMP cannot improve the performance and may even deteriorate the performance. In the case of a resource bottleneck, you are advised to disable the SMP.
   -  If resources are sufficient, the higher the DOP, the more the performance is improved.

   The SMP DOP can be configured at a session level and you are advised to enable the SMP before executing the query that meets the requirements. After the execution is complete, disable the SMP. Otherwise, SMP may affect services in peak hours.

   You can set **query_dop** to **10** to enable the SMP in a session.

#. enable_dynamic_workload

   Enable dynamic load management. Dynamic load management refers to the automatic queue control of complex queries based on user loads in a database. This fine-tunes system parameters without manual adjustment.

   This parameter is enabled by default. Notes:

   -  A CN in the cluster is used as the Central Coordinator (CCN) for collecting and scheduling job execution. Its status will be displayed in **Central Coordinator State**. If there is no CCN, jobs will not be controlled by dynamic load management.
   -  Simple query jobs (which are estimated to require less than 32 MB memory) and non-DML statements (statements other than **INSERT**, **UPDATE**, **DELETE**, and **SELECT**) have no adaptive load restrictions. Control the upper memory limits for them on a single CN using **max_active_statements**.
   -  In adaptive load scenarios, the value cannot be increased. If you increase it, memory cannot be controlled for certain statements, such as statements that have not been analyzed.
   -  Reduce concurrency in the following scenarios, because high concurrency may lead to uncontrollable memory usage.

      -  A single tuple occupies excessive memory, for example, a base table contains a column more than 1 MB wide.
      -  A query is fully pushed down.
      -  A statement occupies a large amount of memory on the CN, for example, a statement that cannot be pushed down or a cursor withholding statement.
      -  An execution plan creates a hash table based on the hash join operator, and the table has many duplicate values and occupies a large amount of memory.
      -  UDFs are used, which occupy a large amount of memory.

   When configuring this parameter, you can set **query_dop** to **0** (adaptive). In this case, the system dynamically selects the optimal DOP between 1 and 8 for each query based on resource usage and plan characteristics. The **enable_dynamic_workload** parameter supports the dynamic memory allocation.

#. max_active_statements

   Specifies the maximum number of concurrent jobs. This parameter applies to all the jobs on one CN.

   Set the value of this parameter based on system resources, such as CPU, I/O, and memory resources, to ensure that the system resources can be fully utilized and the system will not be crashed due to excessive concurrent jobs.

   -  If this parameter is set to **-1** or **0**, the number of global concurrent jobs is not limited.
   -  In the point query scenario, you are advised to set this parameter to **100**.
   -  In an analytical query scenario, set this parameter to the number of CPU cores divided by the number of DNs. Generally, its value ranges from 5 to 8.

#. **session_timeout**

   By default, if a client is in idle state after connecting to a database, the client automatically disconnects from the database after the duration specified by the parameter.

   **Value range**: an integer ranging from 0 to 86400. The minimum unit is second (s). **0** means to disable the timeout. Generally, you are advised not to set this parameter to **0**.

#. The five parameters that affect the database memory are as follows:

   **max_process_memory**, **shared_buffers**, **cstore_buffers**, **work_mem**, and **maintenance_work_mem**

   -  max_process_memory

      **max_process_memory** is a logical memory management parameter. It is used to control the maximum available memory on a single CN or DN.

      Non-secondary DNs are automatically adapted. The formula is as follows: (Physical memory size) x 0.8/(1 + Number of primary DNs). If the result is less than 2 GB, 2 GB is used by default. The default size of the secondary DN is 12 GB.

   -  shared_buffers

      Specifies the size of the shared memory used by GaussDB(DWS). If the value of this parameter is increased, GaussDB(DWS) requires more System V shared memory than the default system setting.

      You are advised to set **shared_buffers** to a value less than 40% of the memory. It is used to scan row-store tables. Formula: **shared_buffers** = (Memory of a single server/Number of DNs on a single server) x 0.4 x 0.25

   -  cstore_buffers

      Specifies the size of the shared buffer used by column-store tables and column-store tables (ORC, Parquet, and CarbonData) of OBS and HDFS foreign tables.

      Column-store tables use the shared buffer specified by **cstore_buffers** instead of that specified by **shared_buffers**. When column-store tables are mainly used, reduce the value of **shared_buffers** and increase that of **cstore_buffers**.

      Use **cstore_buffers** to specify the cache of ORC, Parquet, or CarbonData metadata and data for OBS or HDFS foreign tables. The metadata cache size should be 1/4 of **cstore_buffers** and not exceed 2 GB. The remaining cache is shared by column-store data and foreign table column-store data.

   -  **work_mem**

      Specifies the size of the memory used by internal sequential operations and the Hash table before data is written into temporary disk files.

      Sort operations are required for **ORDER BY**, **DISTINCT**, and merge joins. Hash tables are used in hash joins, hash-based aggregation, and hash-based processing of **IN** subqueries.

      In a complex query, several sort or hash operations may run in parallel. Each operation will be allowed to use as much memory as this parameter specifies. If the memory is insufficient, data will be written into temporary files. In addition, several running sessions may be performing such operations concurrently. Therefore, the total memory used may be many times the value of **work_mem**.

      The formulas are as follows:

      For non-concurrent complex serial queries, each query requires five to ten associated operations. Configure **work_mem** using the following formula: **work_mem** = 50% of the memory/10.

      For non-concurrent simple serial queries, each query requires two to five associated operations. Configure **work_mem** using the following formula: **work_mem** = 50% of the memory/5.

      For concurrent queries, configure **work_mem** using the following formula: **work_mem** = **work_mem** for serial queries/Number of concurrent SQL statements.

   -  **maintenance_work_mem**

      Specifies the maximum size of memory used for maintenance operations, involving **VACUUM**, **CREATE INDEX**, and **ALTER TABLE ADD FOREIGN KEY**.

      Setting suggestions:

      If you set this parameter to a value greater than that of **work_mem**, database dump files can be cleaned up and restored more efficiently. In a database session, only one maintenance operation can be performed at a time. Maintenance is usually performed when there are not many sessions.

      When the automatic cleanup process is running, up to **autovacuum_max_workers** times of the memory will be allocated. In this case, set **maintenance_work_mem** to a value greater than or equal to that of **work_mem**.

#. **bulk_write_ring_size**

   Specifies the size of a ring buffer used for parallel data import.

   This parameter affects the database import performance. You are advised to increase the value of this parameter on DNs when a large amount of data is to be imported.

#. The following parameters affect the database connection:

   **max_connections** and **max_prepared_transactions**

   -  max_connections

      Specifies the maximum number of concurrent connections to the database. This parameter affects the concurrent processing capability of the cluster.

      Setting suggestions:

      Retain the default value of this parameter on CNs. Set this parameter on DNs to a value calculated using this formula: Number of CNs x Value of this parameter on a CN.

      If the value of this parameter is increased, GaussDB(DWS) may require more System V shared memory or semaphore, which may exceed the default maximum value of the OS. In this case, modify the value as needed.

   -  max_prepared_transactions

      Specifies the maximum number of transactions that can stay in the **prepared** state simultaneously. If the value of this parameter is increased, GaussDB(DWS) requires more System V shared memory than the default system setting.

   .. important::

      The value of **max_connections** is related to **max_prepared_transactions**. Before configuring **max_connections**, ensure that the value of **max_prepared_transactions** is greater than or equal to that of **max_connections**. In this way, each session has a prepared transaction in the waiting state.

#. checkpoint_completion_target

   Specifies the target for which the checkpoint is completed.

   Each checkpoint must be completed within 50% of the checkpoint interval.

   The default value is **0.5**. To improve the performance, you can change the value to 0.9.

#. data_replicate_buffer_size

   Specifies the memory used by queues when the sender sends data pages to the receiver. The value of this parameter affects the buffer size used for the replication from the primary server to the standby server.

   The default value is **128 MB**. If the server memory is 256 GB, you can increase the value to 512 MB.
