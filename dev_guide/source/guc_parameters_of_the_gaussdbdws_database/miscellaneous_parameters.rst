:original_name: dws_04_0945.html

.. _dws_04_0945:

Miscellaneous Parameters
========================

enable_cluster_resize
---------------------

**Parameter description**: Indicates whether the current session is for scaling or redistributing data. It should only be used for these specific sessions and not set for other service sessions.

**Type**: SUSET

**Value range**: Boolean

-  **on** indicates that the current session is for scaling or redistributing data, and allows the execution of specific SQL statements for redistribution.
-  **off** indicates that the current session is not for scaling or redistributing data, and does not allow the execution of specific SQL statements for redistribution.

**Default value**: **off**

.. note::

   This parameter is used for internal O&M. Do not set it to **on** unless absolutely necessary.

dfs_partition_directory_length
------------------------------

**Parameter description**: Specifies the largest directory name length for the partition directory of a table partitioned by VALUE in the HDFS.

**Type**: USERSET

**Value range**: 92 to 7999

**Default value**: **512**

enable_hadoop_env
-----------------

**Parameter description**: Sets whether local row- and column-store tables can be created in a database while the Hadoop feature is used. In the GaussDB(DWS) cluster, it is set to **off** by default to support local row- and column- based storage and cross-cluster access to Hadoop. You are not advised to change the value of this parameter.

**Type**: USERSET

**Value range**: Boolean

-  **on** or **true**, indicating that local row- and column-store tables cannot be created in a database while the Hadoop feature is used.
-  **off** or **false**, indicating that local row- and column-based tables can be created in a database while the Hadoop feature is used.

**Default value**: **off**

enable_upgrade_merge_lock_mode
------------------------------

**Parameter description**: If this parameter is set to **on**, the delta merge operation internally increases the lock level, and errors can be avoided when update and delete operations are performed at the same time.

**Type**: USERSET

**Value range**: Boolean

-  If this parameter is set to **on**, the delta merge operation internally increases the lock level. In this way, when any two of the **DELTAMERGE**, **UPDATE**, and **DELETE** operations are concurrently performed, an operation can be performed only after the previous one is complete.
-  If this parameter is set to **off**, and any two of the **DELTAMERGE**, **UPDATE**, and **DELETE** operations are concurrently performed to data in a row in the delta table of the HDFS table, errors will be reported during the later operation, and the operation will stop.

**Default value**: **off**

job_queue_processes
-------------------

**Parameter description**: Specifies the number of jobs that can be concurrently executed.

**Type**: POSTMASTER

**Value range**: 0 to 1000

**Functions**:

-  Setting **job_queue_processes** to **0** indicates that the scheduled task function is disabled and that no job will be executed. (Enabling scheduled tasks may affect the system performance. At sites where this function is not required, you are advised to disable it.)
-  Setting **job_queue_processes** to a value that is greater than **0** indicates that the scheduled task function is enabled and this value is the maximum number of tasks that can be concurrently processed.

After the scheduled task function is enabled, the **job_scheduler** thread at a scheduled interval polls the **pg_jobs** system catalog. The scheduled task check is performed every second by default.

Too many concurrent tasks consume many system resources, so you need to set the number of concurrent tasks to be processed. If the current number of concurrent tasks reaches **job_queue_processes** and some of them expire, these tasks will be postponed to the next polling period. Therefore, you are advised to set the polling interval (the **interval** parameter of the **submit** API) based on the execution duration of each task to avoid the problem that tasks in the next polling period cannot be properly processed because overlong task execution time.

Note: If the number of parallel jobs is large and the value is too small, these jobs will wait in queues. However, a large parameter value leads to large resource consumption. You are advised to set this parameter to **100** and change it based on the system resource condition.

**Default value**: **10**

ngram_gram_size
---------------

**Parameter description**: Specifies the length of the ngram parser segmentation.

**Type**: USERSET

**Value range**: an integer ranging from 1 to 4

**Default value**: **2**

ngram_grapsymbol_ignore
-----------------------

**Parameter description**: Specifies whether the ngram parser ignores graphical characters.

**Type**: USERSET

**Value range**: Boolean

-  **on**: Ignores graphical characters.
-  **off**: Does not ignore graphical characters.

**Default value**: **off**

ngram_punctuation_ignore
------------------------

**Parameter description**: Specifies whether the ngram parser ignores punctuations.

**Type**: USERSET

**Value range**: Boolean

-  **on**: Ignores punctuations.
-  **off**: Does not ignore punctuations.

**Default value**: **on**

zhparser_multi_duality
----------------------

**Parameter description**: Specifies whether Zhparser aggregates segments in long words with duality.

**Type**: USERSET

**Value range**: Boolean

-  **on**: Aggregates segments in long words with duality.
-  **off**: Does not aggregate segments in long words with duality.

**Default value**: **off**

zhparser_multi_short
--------------------

**Parameter description**: Specifies whether Zhparser executes long words composite divide.

**Type**: USERSET

**Value range**: Boolean

-  **on**: Performs compound segmentation for long words.
-  **off**: Does not perform compound segmentation for long words.

**Default value**: **on**

zhparser_multi_zall
-------------------

**Parameter description**: Specifies whether Zhparser displays all single words individually.

**Type**: USERSET

**Value range**: Boolean

-  **on**: Displays all single words separately.
-  **off**: Does not display all single words separately.

**Default value**: **off**

zhparser_multi_zmain
--------------------

**Parameter description**: Specifies whether Zhparser displays important single words separately.

**Type**: USERSET

**Value range**: Boolean

-  **on**: Displays important single words separately.
-  **off**: Does not display important single words separately.

**Default value**: **off**

zhparser_punctuation_ignore
---------------------------

**Parameter description**: Specifies whether the Zhparser segmentation result ignores special characters including punctuations (\\r and \\n will not be ignored).

**Type**: USERSET

**Value range**: Boolean

-  **on**: Ignores all the special characters including punctuations.
-  **off**: Does not ignore all the special characters including punctuations.

**Default value**: **on**

zhparser_seg_with_duality
-------------------------

**Parameter description**: Specifies whether Zhparser aggregates segments in long words with duality.

**Type**: USERSET

**Value range**: Boolean

-  **on**: Aggregates segments in long words with duality.
-  **off**: Does not aggregate segments in long words with duality.

**Default value**: **off**

.. _en-us_topic_0000001811490665__section13787157164412:

acceleration_with_compute_pool
------------------------------

**Parameter description**: Specifies whether to use the computing resource pool for acceleration when OBS is queried.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that the query covering OBS is accelerated based on the cost when the computing resource pool is available.
-  **off** indicates that no query is accelerated using the computing resource pool.

**Default value**: **off**

redact_compat_options
---------------------

**Parameter description**: Specifies the compatibility option for calculation using masked data. This parameter is supported only by clusters of version 8.1.3.310 or later.

**Type**: USERSET

**Value range**: a string

-  **none** indicates that compatibility options are specified.
-  **disable_comparison_operator_mask** indicates that comparison operators that do not expose raw data can bypass the data masking check and generate the actual calculation result.

**Default value**: **none**

table_skewness_warning_threshold
--------------------------------

**Parameter description**: Specifies the threshold for triggering a table skew alarm.

**Type**: SUSET

**Value range**: a floating point number ranging from 0 to 1

**Default value**: **1**

table_skewness_warning_rows
---------------------------

**Parameter description**: Specifies the minimum number of rows for triggering a table skew alarm.

**Type**: SUSET

**Value range**: an integer ranging from **0** to **INT_MAX**

**Default value**: **100000**

enable_view_update
------------------

**Parameter description**: Enables the view update function or not.

**Type**: POSTMASTER

**Value range**: Boolean

-  **on** indicates that the view update function is enabled.
-  **off** indicates that the view update function is disabled.

**Default value**: **off**

view_independent
----------------

**Parameter description**: Decouples views from tables, functions, and synonyms or not. After the base table is restored, automatic association and re-creation are supported.

**Type**: SIGHUP

**Value range**: Boolean

-  **on** indicates that the view decoupling function is enabled. Tables, functions, synonyms, and other views on which views depend can be deleted separately (except temporary tables and temporary views). Associated views are reserved but unavailable.
-  **off** indicates that the view decoupling function is disabled. Tables, functions, synonyms, and other views on which views depend cannot be deleted separately. You can only delete them in the cascade mode.

**Default value**: **off**

assign_abort_xid
----------------

**Parameter description**: Determines the transaction to be aborted based on the specified XID in a query.

**Type**: USERSET

**Value range**: a character string with the specified XID

.. caution::

   This parameter is used only for quick restoration if a user deletes data by mistake (DELETE operation). Do not use this parameter in other scenarios. Otherwise, visible transaction errors may occur.

default_distribution_mode
-------------------------

**Parameter description**: Specifies the default distribution mode of a table. This feature is supported only in 8.1.2 or later.

**Type**: USERSET

**Value range**: enumerated values

-  **roundrobin**: If the distribution mode is not specified during table creation, the default distribution mode is selected according to the following rules:

   #. If the primary key or unique constraint is included during table creation, hash distribution is selected. The distribution column is the column corresponding to the primary key or unique constraint.
   #. If the primary key or unique constraint is not included during table creation, round-robin distribution is selected.

-  **hash**: If the distribution mode is not specified during table creation, the default distribution mode is selected according to the following rules:

   #. If the primary key or unique constraint is included during table creation, hash distribution is selected. The distribution column is the column corresponding to the primary key or unique constraint.
   #. If the primary key or unique constraint is not included during table creation but there are columns whose data types can be used as distribution columns, hash distribution is selected. The distribution column is the first column whose data type can be used as a distribution column.
   #. If the primary key or unique constraint is not included during table creation and no column whose data type can be used as a distribution column exists, round-robin distribution is selected.

**Default value**: **roundrobin**

.. note::

   The default value of this parameter is **roundrobin** for a new GaussDB(DWS) 8.1.2 cluster and is **hash** for an upgrade to GaussDB(DWS) 8.1.2.

object_mtime_record_mode
------------------------

**Parameter description**: Sets the update action of the **mtime** column in the **PG_OBJECT** system catalog.

**Type**: SIGHUP

**Value range**: a string

-  de\ **fault**: **ALTER**, **COMMENT**, **GRANT/REVOKE**, and **TRUNCATE** operations update the **mtime** column by default.
-  **disable**: The **mtime** column is not updated.
-  **disable_acl**: **GRANT** or **REVOKE** operation does not update the **mtime** column.
-  **disable_truncate**: **TRUNCATE** operations do not update the **mtime** column.
-  **disable_partition**: Partition **ALTER** operations do not update the **mtime** column.

**Default value**: **default**

max_volatile_tables
-------------------

**Parameter description**: Specifies the maximum number of volatile tables created for each session, including volatile tables and their auxiliary tables. This parameter is supported by clusters of version 8.2.0 or later.

**Type**: USERSET

**Value range**: an integer ranging from 0 to INT_MAX

**Default value**: **300**

query_cache_refresh_time
------------------------

**Parameter description**: Specifies the cache refresh interval for queries for which the **enable_accelerate_select** parameter takes effect. This parameter is supported only by clusters of version 8.3.0 or later.

**Type**: USERSET

**Value range**: a floating point number ranging from 0 to 10000.0, in seconds

**Default value**: **60.0**

vector_engine_strategy
----------------------

**Parameter description**: Specifies the vectorization enhancement policy. This parameter is supported only by clusters of version 8.3.0 or later.

**Type**: USERSET

**Value range**: enumerated values

-  **force** specifies that the vectorization-enhanced plan is forcibly rolled back to the row storage plan when there are scenarios that do not support vectorization.
-  **improve** specifies that vectorization enhancement is enabled even when there are scenarios that do not support vectorization.

**Default value**: **improve**

default_temptable_type
----------------------

**Parameter description**: Specifies the type of temporary table created when **CREATE TABLE** is used to create a temporary table without specifying the table type before **TEMP** or **TEMPORARY**. This parameter is supported only by clusters of version 9.1.0 or later.

**Type**: USERSET

**Value range**: enumerated values

-  **local**: creates a local temporary table when the type is not specified.
-  **volatile**: creates a volatile temporary table when the type is not specified.

**Default value**: **local**

pgxc_node_readonly
------------------

**Parameter description**: Specifies whether a CN or DN is an elastic or classic DN. This parameter is supported only by clusters of version 9.1.0 or later.

**Type**: SUSET

**Value range**: Boolean

-  **on** indicates that the CN or DN is an elastic node.
-  **off** indicates that the CN or DN is a classic node.

**Default value**: **off**

hudi_sync_max_commits
---------------------

**Parameter description**: Specifies the maximum number of commits for a single synchronization task in Hudi. This parameter is supported only by clusters of version 9.1.0.100 or later.

**Type**: SIGHUP

**Value range**: an integer ranging from -1 to INT_MAX

-  **-1** indicates no limit.
-  **0** indicates no limit.
-  Any other value indicates the maximum number of commits.

**Default value**: **-1**

foreign_table_default_rw_options
--------------------------------

**Parameter description**: Specifies the default permissions when creating a foreign table without specifying them. This parameter is supported only by clusters of version 9.0.3 or later.

**Type**: USERSET

**Value range**: a string

-  **READ_ONLY** indicates the read-only permission.
-  **WRITE_ONLY** indicates the write-only permission.
-  **READ_WRITE** indicates the read-write permission.

**Default value**: **READ_ONLY**
