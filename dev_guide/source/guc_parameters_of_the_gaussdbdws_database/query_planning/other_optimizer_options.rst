:original_name: dws_04_0912.html

.. _dws_04_0912:

Other Optimizer Options
=======================

.. _en-us_topic_0000001460562696__sa5c1527051e54fbdb6c5346d54bcbf5a:

default_statistics_target
-------------------------

**Parameter description**: Specifies the default statistics target for table columns without a column-specific target set via **ALTER TABLE SET STATISTICS**. If this parameter is set to a positive number, it indicates the number of samples of statistics information. If this parameter is set to a negative number, percentage is used to set the statistic target. The negative number converts to its corresponding percentage, for example, -5 means 5%. During sampling, the random sampling size is **default_statistics_target** x 300. For example, if the **default_statistics_target** is 100, 30,000 data records from 30,000 pages are randomly sampled.

**Type**: USERSET

**Value range**: an integer ranging from -100 to 10,000

.. important::

   -  A larger positive number than the parameter value increases the time required to do **ANALYZE**, but might improve the quality of the optimizer's estimates.
   -  Changing settings of this parameter may result in performance deterioration. If query performance deteriorates, you can:

      #. Restore to the default statistics.
      #. Use hints to optimize the query plan.

   -  If this parameter is set to a negative value, the number of samples is greater than or equal to 2% of the total data volume, and the number of records in user tables is less than 1.6 million, the time taken by running **ANALYZE** will be longer than when this parameter uses its default value.
   -  **AUTOANALYZE** does not allow you to set a sampling size for temporary table sampling. Its default value will be used for sampling.
   -  If statistics are forcibly calculated based on memory, the sampling size is limited by the **maintenance_work_mem** parameter.

**Default value**: **100**

random_function_version
-----------------------

**Parameter description**: Specifies the random function version selected by ANALYZE during data sampling. This feature is supported only in 8.1.2 or later.

**Type**: USERSET

**Value range**: enumerated values

-  The value **0** indicates that the random function provided by the C standard library is used.
-  The value **1** indicates that the optimized and enhanced random function is used.

Default value:

-  If the current cluster is upgraded from an earlier version to 8.2.0.100, the default value is **0** to ensure forward compatibility.
-  If the cluster version 8.2.0.100 is newly installed, the default value is **1**.

constraint_exclusion
--------------------

**Parameter description**: Controls the query optimizer's use of table constraints to optimize queries.

**Type**: USERSET

**Value range**: enumerated values

-  **on** indicates the constraints for all tables are examined.
-  **off**: No constraints are examined.
-  **partition** indicates that only constraints for inherited child tables and **UNION ALL** subqueries are examined.

   .. important::

      When **constraint_exclusion** is set to **on**, the optimizer compares query conditions with the table's **CHECK** constraints, and omits scanning tables for which the conditions contradict the constraints.

**Default value**: **partition**

.. note::

   Currently, this parameter is set to **on** by default to partition tables. If this parameter is set to **on**, extra planning is imposed on simple queries, which has no benefits. If you have no partitioned tables, set it to **off**.

cursor_tuple_fraction
---------------------

**Parameter description**: Specifies the optimizer's estimated fraction of a cursor's rows that are retrieved.

**Type**: USERSET

**Value range**: a floating point number ranging from 0.0 to 1.0

.. important::

   Smaller values than the default value bias the optimizer towards using **fast start** plans for cursors, which will retrieve the first few rows quickly while perhaps taking a long time to fetch all rows. Larger values put more emphasis on the total estimated time. At the maximum setting of **1.0**, cursors are planned exactly like regular queries, considering only the total estimated time and how soon the first rows might be delivered.

**Default value**: **0.1**

from_collapse_limit
-------------------

**Parameter description**: Specifies whether the optimizer merges sub-queries into upper queries based on the resulting FROM list. The optimizer merges sub-queries into upper queries if the resulting FROM list would have no more than this many items.

**Type**: USERSET

**Value range**: an integer ranging from 1 to INT_MAX

.. important::

   Smaller values reduce planning time but may lead to inferior execution plans.

**Default value**: **8**

join_collapse_limit
-------------------

**Parameter description**: Specifies whether the optimizer rewrites **JOIN** constructs (except **FULL JOIN**) into lists of **FROM** items based on the number of the items in the result list.

**Type**: USERSET

**Value range**: an integer ranging from 1 to INT_MAX

.. important::

   -  Setting this parameter to **1** prevents join reordering. As a result, the join order specified in the query will be the actual order in which the relations are joined. The query optimizer does not always choose the optimal join order. Therefore, advanced users can temporarily set this variable to **1**, and then specify the join order they desire explicitly.
   -  Smaller values reduce planning time but lead to inferior execution plans.

**Default value**: **8**

enable_from_collapse_hint
-------------------------

**Parameter description**: Specifies whether to rewrite the **FROM** list to make the hint take effect, and then rewrite it again based on the **from_collapse_limit** and **join_collapse_limit** parameters. This parameter is supported by version 8.2.0 or later clusters.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that the **FROM** list is first rewritten in hint mode.
-  **off** indicates that the **FROM** list is rewritten without difference.

.. important::

   -  If this parameter is enabled, the optimizer preferentially rewrites the **FROM** list in hint mode. However, you can learn whether a hint takes effect only after the plan is generated.
   -  If this parameter is disabled, the plan is generated in the same way as that in versions earlier than 8.2.0. That is, the plan is generated regardless of whether the table has hints.

**Default value**: **on**

plan_mode_seed
--------------

**Parameter description**: This is a commissioning parameter. Currently, it supports only OPTIMIZE_PLAN and RANDOM_PLAN. **OPTIMIZE_PLAN** indicates the optimal plan, the cost of which is estimated using the dynamic planning algorithm, and its value is **0**. **RANDOM_PLAN** indicates the plan that is randomly generated. If **plan_mode_seed** is set to **-1**, you do not need to specify the value of the seed identifier. Instead, the optimizer generates a random integer ranging from **1** to **2147483647**, and then generates a random execution plan based on this random number. If **plan_mode_seed** is set to an integer ranging from **1** to **2147483647**, you need to specify the value of the seed identifier, and the optimizer generates a random execution plan based on the seed value.

**Type**: USERSET

**Value range**: an integer ranging from -1 to 2147483647

**Default value**: **0**

.. important::

   -  If **plan_mode_seed** is set to **RANDOM_PLAN**, the optimizer generates different random execution plans, which may not be the optimal. Therefore, to guarantee the query performance, the default value **0** is recommended during upgrade, scale-out, scale-in, and O&M.
   -  If this parameter is not set to **0**, the specified hint will not be used.

enable_hdfs_predicate_pushdown
------------------------------

**Parameter description**: Specifies whether the function of pushing down predicates the native data layer is enabled.

**Type**: SUSET

**Value range**: Boolean

-  **on** indicates this function is enabled.
-  **off** indicates this function is disabled.

**Default value**: **on**

windowagg_pushdown_enhancement
------------------------------

**Parameter description**: Specifies whether to enable enhanced predicate pushdown for window functions in aggregation scenarios. (This parameter is supported by version 8.2.0 or later clusters.)

**Type**: SUSET

**Value range**: Boolean

-  **on** indicates that the predicate pushdown enhancement for window functions is enabled in aggregation scenarios.
-  **off** indicates that the predicate pushdown enhancement for window functions is disabled in aggregation scenarios.

**Default value**: **on**

implied_quality_optmode
-----------------------

**Parameter description**: Specifies how to pass conditions for the equivalent columns in a statement. (This parameter is supported by version 8.2.0 or later clusters.)

**Type**: SUSET

**Value range**: enumerated values

-  **normal** indicates forward compatibility with 8.1.3 and earlier versions, that is, the implied expression behavior is optimized.
-  **negative** indicates that the implied expression behavior is not optimized.
-  **positive** indicates that type conversion expressions are optimized in addition to the operations specified by **normal**.

**Default value:** **normal**

enable_random_datanode
----------------------

**Parameter description**: Specifies whether the function that random query about DNs in the replication table is enabled. A complete data table is stored on each DN for random retrieval to release the pressure on nodes.

**Type**: USERSET

**Value range**: Boolean

-  **on**: This function is enabled.
-  **off**: This function is disabled.

**Default value**: **on**

hashagg_table_size
------------------

**Parameter description**: Specifies the hash table size during **HASH AGG** execution.

**Type**: USERSET

**Value range**: an integer ranging from 0 to INT_MAX/2

**Default value**: **0**

.. _en-us_topic_0000001460562696__se75ab653da604c90acf654efc674c720:

enable_codegen
--------------

**Parameter description**: Specifies whether code optimization can be enabled. Currently, the code optimization uses the LLVM optimization.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates code optimization can be enabled.
-  **off** indicates code optimization cannot be enabled.

   .. important::

      Currently, the LLVM optimization only supports the vectorized executor and SQL on Hadoop features. You are advised to set this parameter to **off** in other cases.

**Default value**: **on**

codegen_strategy
----------------

**Parameter description**: Specifies the codegen optimization strategy that is used when an expression is converted to codegen-based.

**Type**: USERSET

**Value range**: enumerated values

-  **partial** indicates that you can still call the LLVM dynamic optimization strategy using the codegen framework of an expression even if functions that are not codegen-based exist in the expression.
-  **pure** indicates that the LLVM dynamic optimization strategy can be called only when all functions in an expression can be codegen-based.

   .. important::

      In the scenario where query performance reduces after the codegen function is enabled, you can set this parameter to **pure**. In other scenarios, do not change the default value **partial** of this parameter.

**Default value**: **partial**

enable_codegen_print
--------------------

**Parameter description:** Specifies whether the LLVM IR function can be printed in logs.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that the LLVM IR function can be printed in logs.
-  **off** indicates that the LLVM IR function cannot be printed in logs.

**Default value**: **off**

codegen_cost_threshold
----------------------

**Parameter description**: The LLVM compilation takes some time to generate executable machine code. Therefore, LLVM compilation is beneficial only when the actual execution cost is more than the sum of the code required for generating machine code and the optimized execution cost. This parameter specifies a threshold. If the estimated execution cost exceeds the threshold, LLVM optimization is performed.

**Type**: USERSET

**Value range**: an integer ranging from **0** to **INT_MAX**

**Default value**: **10000**

llvm_compile_expr_limit
-----------------------

**Parameter description**: This sets a limit on the number of expressions that can be compiled using LLVM. If the number of expressions exceeds the limit, only the initial ones will be compiled, and an alert will be triggered. To enable the alert, run **SET analysis_options="on(LLVM_COMPILE)"** before executing **explain performance**. This parameter is supported by clusters of version 8.2.1.220 or later.

**Type**: USERSET

**Value range**: an integer ranging from **-1** to **INT_MAX**

**Default value**: **500**

enable_constraint_optimization
------------------------------

**Parameter description**: Specifies whether the informational constraint optimization execution plan can be used for an HDFS foreign table.

**Type**: SUSET

**Value range**: Boolean

-  **on** indicates the plan can be used.
-  **off** indicates the plan cannot be used.

**Default value**: **on**

enable_bloom_filter
-------------------

**Parameter description**: Specifies whether the BloomFilter optimization is used.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates the BloomFilter optimization can be used.
-  **off** indicates the BloomFilter optimization cannot be used.

**Default value**: **on**

.. important::

   Scenario: If in a HASH JOIN, the thread of the foreign table contains HDFS tables or column-store tables, the Bloom filter is triggered.

   Constraints:

   #. Only **INNER JOIN**, **SEMI JOIN**, **RIGHT JOIN**, **RIGHT SEMI JOIN**, **RIGHT ANTI JOIN** and **RIGHT ANTI FULL JOIN** are supported.
   #. The number of rows in the internal table in the join cannot exceed 50,000.
   #. JOIN condition of the internal table: It cannot be an expression for HDFS internal or foreign tables. It can be an expression for column-store tables, but only at the non-join layer.
   #. The join condition of the foreign table must be simple column join.
   #. When the join conditions of the internal and foreign tables (HDFS) are both simple column joins, the estimated data that can be removed at the plan layer must be over 1/3.
   #. Joined columns cannot contain NULL values.
   #. Data is not spilled to disks at the JOIN layer.
   #. Data type:

      -  HDFS internal and foreign tables support SMALLINT, INTEGER, BIGINT, REAL/FLOAT4, DOUBLE PRECISION/FLOAT8, CHAR(n)/CHARACTER(n)/NCHAR(n), VARCHAR(n)/CHARACTER VARYING(n), CLOB and TEXT.
      -  Column-store tables support SMALLINT, INTEGER, BIGINT, OID, "char", CHAR(n)/CHARACTER(n)/NCHAR(n), VARCHAR(n)/CHARACTER VARYING(n), NVARCHAR2(n), CLOB, TEXT, DATE, TIME, TIMESTAMP and TIMESTAMPTZ. The collation of the character type must be **C**.

enable_extrapolation_stats
--------------------------

**Parameter description**: Specifies whether to use the extrapolation logic based on historical statistics. Using this logic may increase the accuracy of estimation for tables whose statistics have not been collected. However, there is also a possibility that the estimation is too large due to incorrect inference.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that the extrapolation logic is used for data of DATE type based on historical statistics.
-  **off** indicates that the extrapolation logic is not used for data of DATE type based on historical statistics.

**Default value**:

-  If the current cluster is upgraded from an earlier version to 8.2.0.100, the default value is **off** to ensure forward compatibility.
-  If the cluster version 8.2.0.100 is newly installed, the default value is **on**.

.. _en-us_topic_0000001460562696__section114241119217:

autoanalyze
-----------

**Parameter description**: Specifies whether to allow automatic statistics collection for a table that has no statistics or a table whose amount of data modification reaches the threshold for triggering **ANALYZE** when a plan is generated. In this case, **AUTOANALYZE** cannot be triggered for foreign tables or temporary tables with the **ON COMMIT [DELETE ROWS|DROP]** option. To collect statistics, you need to manually perform the **ANALYZE** operation. If an exception occurs in the database during the execution of autoanalyze on a table, after the database is recovered, the system may still prompt you to collect the statistics of the table when you run the statement again. In this case, manually perform the **ANALYZE** operation on the table to synchronize statistics.

.. important::

   If the amount of data modification reaches the threshold for triggering **ANALYZE**, the amount of data modification exceeds **autovacuum_analyze_threshold** + **autovacuum_analyze_scale_factor \*** *reltuples*. *reltuples* indicates the estimated number of rows in the table recorded in **pg_class**.

**Type**: SUSET

**Value range**: Boolean

-  **on** indicates that the table statistics are automatically collected.
-  **off** indicates that the table statistics are not automatically collected.

**Default value**: **on**

query_dop
---------

**Parameter description**: Specifies the user-defined degree of parallelism.

**Type**: USERSET

**Value range**: an integer ranging from -64 to 64.

[1, 64]: Fixed SMP is enabled, and the system will use the specified degree.

0: SMP adaptation function is enabled. The system dynamically selects the optimal parallelism degree [1,8] (x86 platforms) for each query based on the resource usage and query plans.

[-64, -1]: SMP adaptation is enabled, and the system will dynamically select a degree from the limited range.

.. note::

   -  For TP services that mainly involve short queries, if services cannot be optimized through lightweight CNs or statement delivery, it will take a long time to generate an SMP plan. You are advised to set **query_dop** to **1**. For AP services with complex statements, you are advised to set **query_dop** to **0**.
   -  After enabling concurrent queries, ensure you have sufficient CPU, memory, network, and I/O resources to achieve the optimal performance.
   -  To prevent performance deterioration caused by an overly large value of **query_dop**, the system calculates the maximum number of available CPU cores for a DN and uses the number as the upper limit for this parameter. If the value of **query_dop** is greater than 4 and also the upper limit, the system resets **query_dop** to the upper limit.

**Default value**: **1**

query_dop_ratio
---------------

**Parameter description**: Specifies the DOP multiple used to adjust the optimal DOP preset in the system when **query_dop** is set to **0**. That is, DOP = Preset DOP x query_dop_ratio (ranging from 1 to 64). If this parameter is set to **1**, the DOP cannot be adjusted.

**Type**: USERSET

**Value range**: a floating point number ranging from 0 to 64

**Default value**: **1**

debug_group_dop
---------------

**Parameter description**: Specifies the unified DOP parallelism degree allocated to the groups that use the Stream operator as the vertex in the generated execution plan when the value of **query_dop** is **0**. This parameter is used to manually specify the DOP for specific groups for performance optimization. Its format is **G1,D1,G2,D2,...,**, where **G1** and **G2** indicate the group IDs that can be obtained from logs and **D1** and **D2** indicate the specified DOP values and can be any positive integers.

**Type**: USERSET

**Value range**: a string

**Default value**: empty

.. important::

   This parameter is used only for internal optimization and cannot be set. You are advised to use the default value.

enable_analyze_check
--------------------

**Parameter description:** Checks whether statistics were collected about tables whose **reltuples** and **relpages** are shown as **0** in **pg_class** during plan generation. **This parameter has been discarded in clusters of version 8.1.3 or later, but is reserved for compatibility with earlier versions. The setting of this parameter does not take effect.**

**Type**: SUSET

**Value range**: Boolean

-  **on** enables the check.
-  **off** disables the check.

**Default value**: **on**

enable_sonic_hashagg
--------------------

**Parameter description**: Specifies whether to use the Hash Agg operator for column-oriented hash table design when certain constraints are met.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that the Hash Agg operator is used for column-oriented hash table design when certain constraints are met.
-  **off** indicates that the Hash Agg operator is not used for column-oriented hash table design.

.. note::

   -  If **enable_sonic_hashagg** is enabled and certain constraints are met, the Hash Agg operator will be used for column-oriented hash table design, and the memory usage of the operator can be reduced. However, in scenarios where the code generation technology (enabled by :ref:`enable_codegen <en-us_topic_0000001460562696__se75ab653da604c90acf654efc674c720>`) can significantly improve performance, the performance of the operator may deteriorate.
   -  If **enable_sonic_hashagg** is set to **on**, when certain constraints are met, the hash aggregation operator designed for column-oriented hash tables is used and its name is displayed as **Sonic Hash Aggregation** in the output of the Explain Analyze/Performance operation. When the constraints are not met, the operator name is displayed as **Hash Aggregation**.

**Default value**: **on**

enable_sonic_hashjoin
---------------------

**Parameter description**: Specifies whether to use the Hash Join operator for column-oriented hash table design when certain constraints are met.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that the Hash Join operator is used for column-oriented hash table design when certain constraints are met.
-  **off** indicates that the Hash Join operator is not used for column-oriented hash table design.

.. note::

   -  Currently, the parameter can be used only for Inner Join.
   -  If **enable_sonic_hashjoin** is enabled, the memory usage of the Hash Inner operator can be reduced. However, in scenarios where the code generation technology can significantly improve performance, the performance of the operator may deteriorate.
   -  If **enable_sonic_hashjoin** is set to **on**, when certain constraints are met, the hash join operator designed for column-oriented hash tables is used and its name is displayed as **Sonic Hash Join** in the output of the Explain Analyze/Performance operation. When the constraints are not met, the operator name is displayed as **Hash Join**.

**Default value**: **on**

enable_sonic_optspill
---------------------

**Parameter description**: Specifies whether to optimize the number of hash join or hash agg files spilled to disks in the sonic scenario. This parameter takes effect only when **enable_sonic_hashjoin** or **enable_sonic_hashagg** is enabled.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that the number of files spilled to disks is optimized.
-  **off** indicates that the number of files spilled to disks is not optimized.

.. note::

   For the hash join or hash agg operator that meets the sonic criteria, if this parameter is set to **off**, one file is spilled to disks for each column. If this parameter is set to **on** and the data types of different columns are similar, only one file (a maximum of five files) will be spilled to disks.

**Default value**: **on**

expand_hashtable_ratio
----------------------

**Parameter description**: Specifies the expansion ratio used to resize the hash table during the execution of the Hash Agg and Hash Join operators.

**Type**: USERSET

**Value range**: a floating point number of 0 or ranging from 0.5 to 10

.. note::

   -  Value **0** indicates that the hash table is adaptively expanded based on the current memory size.
   -  The value ranging from 0.5 to 10 indicates the multiple used to expand the hash table. Generally, a larger hash table delivers better performance but occupies more memory space. If the memory space is insufficient, data may be spilled to disks in advance, causing performance deterioration.

**Default value**: **0**

plan_cache_mode
---------------

**Parameter description**: Specifies the policy for generating an execution plan in the **prepare** statement.

**Type**: USERSET

**Value range**: enumerated values

-  **auto** indicates that the **custom plan** or **generic plan** is selected by default.
-  **force_generic_plan** indicates that the **generic plan** is forcibly used.
-  **force_custom_plan** indicates that the **custom plan** is forcibly used.

.. note::

   -  This parameter is valid only for the **prepare** statement. It is used when the parameterized field in the **prepare** statement has severe data skew.
   -  **custom plan** is a plan generated after you run a **prepare** statement where parameters in the execute statement is embedded in the **prepare** statement. The **custom plan** generates a plan based on specific parameters in the execute statement. This scheme generates a preferred plan based on specific parameters each time and has good execution performance. The disadvantage is that the plan needs to be regenerated before each execution, resulting in a large amount of repeated optimizer overhead.
   -  **generic plan** is a plan generated for the **prepare** statement. The plan policy binds parameters to the plan when you run the execute statement and execute the plan. The advantage of this solution is that repeated optimizer overheads can be avoided in each execution. The disadvantage is that the plan may not be optimal when data skew occurs for the bound parameter field. When some bound parameters are used, the plan execution performance is poor.

**Default value**: **auto**

wlm_query_accelerate
--------------------

**Parameter description**: Specifies whether the query needs to be accelerated when short query acceleration is enabled.

**Type**: USERSET

**Value range**: an integer ranging from **-1** to **1**

-  **-1**: indicates that short queries are controlled by the fast lane, and the long queries are controlled by the slow lane.
-  **0**: indicates that queries are not accelerated. Both short and long queries are controlled by the slow lane.
-  **1**: indicates that queries are accelerated. Both short queries and long queries are controlled by the fast lane.

**Default value**: **-1**

show_unshippable_warning
------------------------

**Parameter description**: Specifies whether to print the alarm for the statement pushdown failure to the client.

**Type**: USERSET

**Value range**: Boolean

-  **on**: Records the reason why the statement cannot be pushed down in a WARNING log and prints the log to the client.
-  **off**: Logs the reason why the statement cannot be pushed down only.

**Default value**: **off**

hashjoin_spill_strategy
-----------------------

**Parameter description**: specifies the hash join policy for spilling data to disks. This feature is supported in 8.1.2 or later.

**Type**: USERSET

**Value range**: The value is an integer ranging from 0 to 6.

-  **0**: If an inner table is too large to be fully stored in database memory, the table will be partitioned. If the table cannot be further partitioned and there is not enough memory for storing it, the system will check whether the foreign table can be stored in memory and be used to create a hash table. If the foreign table can be stored in the memory and used to create a hash table, HashJoin will be performed. Otherwise, NestLoop will be performed.
-  **1**: If an inner table is too large to be fully stored in database memory, the table will be partitioned. If the table cannot be further partitioned and there is still not enough memory for storing it, the system will check whether the foreign table can be stored in memory and be used to create a hash table. If both the inner and outer tables are large, a hash join is forcibly performed.
-  **2**: If the size of the inner table is large and cannot be partitioned after data is spilled to disks for multiple times, HashJoin will be forcibly performed.
-  **3**: If the size of the inner table is large and cannot be partitioned after data is spilled to disks for multiple times, the system attempts to place the outer table in the available memory of the database to create a hash table. If both the inner and outer tables are large, an error is reported.
-  **4**: If the size of the inner table is large and cannot be partitioned after data is spilled to disks for multiple times, an error is reported.
-  **5**: If the inner table is large and cannot be fully stored in database memory, and the foreign table can be fully stored in memory, the foreign table will be used to create a hash table and perform HashJoin. If the foreign table cannot be fully stored in memory, it will be partitioned until the inner and foreign tables cannot be further partitioned. Then, NestLoop will be performed.
-  **6**: If the inner table is large and cannot be fully stored in database memory, and the foreign table can be fully stored in memory, the foreign table will be used to create a hash table and perform HashJoin. If the foreign table cannot be fully stored in memory, it will be partitioned until the inner and foreign tables cannot be further partitioned. Then, HashJoin will be forcibly performed.

.. note::

   -  This parameter is valid only for a vectorized hash join operator.
   -  If the number of distinct values is small and the data volume is large, data may fail to be flushed to disks. As a result, the memory usage is too high and the memory is out of control. If this parameter is set to **0**, the system attempts to swap the inner and outer tables or perform a nested loop join to prevent this problem. However, a nested loop join may deteriorate performance in some scenarios. In this case, this parameter can be set to **1**, **2**, or **6** to forcibly perform HashJoin.
   -  The value **0** does not take effect for a vectorized full join, and the behavior is the same as that of the value **1**. The system attempts to create a hash table only for the outer table and does not perform a nested loop join.
   -  If the inner table is too large to be fully stored in memory, but the foreign table can be stored in memory, you are advised to set this parameter to **5** or **6** rather than **0** or **1**, directly performing Hashjoin on the foreign table without multiple rounds of partitioning and spill to disk. If a foreign table contains only a small amount of distinct data, creating a hash table using the foreign table may cause performance deterioration. In this case, you can change the value of this parameter to **0** or **1**.

**Default value**: **0**

max_streams_per_query
---------------------

**Parameter description**: Controls the number of Stream nodes in a query plan. (This parameter is supported only in 8.1.1 and later cluster versions.)

**Type**: SUSET

**Value range**: an integer ranging from -1 to 10000.

-  **-1** indicates that the number of Stream nodes in the query plan is not limited.
-  A value within the range **0** to **10000** indicates that when the number of Stream nodes in the query plan exceeds the specified value, an error is reported and the query plan will not be executed.

.. note::

   -  This parameter controls only the Stream nodes on DNs and does not control the Gather nodes on the CN.
   -  This parameter does not affect the EXPLAIN query plan, but affects EXPLAIN ANALYZE and EXPLAIN PERFORMANCE.

**Default value**: **-1**

enable_agg_limit_opt
--------------------

**Parameter description**: Specifies whether to optimize **select distinct col from table limit N**. This parameter is valid only if N is less than 16,384. **table** indicates a column-store table. This parameter is supported only by clusters of version 8.2.0.101 or later.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that the optimization is enabled. After this function is enabled, query results are from different DNs, and you do not need to create a full hash table on each DN, significantly improving query performance.

-  **off** indicates that the optimization is disabled.

**Default value**: **off**

stream_ctescan_pred_threshold
-----------------------------

**Parameter description**: minimum number of filter criteria contained in a CTE when **enable_stream_ctescan** is set to **on** and the CTE contains only a single table filtering condition. If the value is greater than or equal to the value of this parameter, the share scan mode is used. If the value is less than the value of this parameter, the inline mode is used. This parameter is supported only by clusters of version 8.2.1 or later.

**Type**: SUSET

**Value range**: an integer ranging from **0** to **INT_MAX**

**Default value**: **2**

stream_ctescan_max_estimate_mem
-------------------------------

**Parameter description**: maximum estimated memory value of the CTE when **enable_stream_ctescan** is set to **on**. This parameter must be used together with **stream_ctescan_refcount_threshold**. If the estimated memory is greater than the value of **stream_ctescan_max_estimate_mem** and the number of references is less than the value of **stream_ctescan_refcount_threshold**, the inline mode is used. Otherwise, the sharescan mode is used. This parameter is supported only by clusters of version 8.2.1 or later.

**Type**: SUSET

**Value range**: an integer ranging from 32 x 1024 (32 MB) to INT_MAX, in KB.

**Default value**: **256 MB**

stream_ctescan_refcount_threshold
---------------------------------

**Parameter description**: maximum number of times that the CTE can be referenced when **enable_stream_ctescan** is set to **on**. This parameter must be used together with **stream_ctescan_max_estimate_mem**. If the estimated memory is greater than the value of **stream_ctescan_max_estimate_mem** and the number of references is less than the value of **stream_ctescan_refcount_threshold**, the inline mode is used. Otherwise, the sharescan mode is used. This parameter is supported only by clusters of version 8.2.1 or later.

**Type**: SUSET

**Value range**: an integer ranging from 0 to INT_MAX

**Default value**: 4

.. note::

   This parameter takes effect only when the value is greater than 0. When the value is 0, only **stream_ctescan_max_estimate_mem** is used to control the inline behavior.

inlist_rough_check_threshold
----------------------------

**Parameter description**: Specifies the maximum number of values in the **IN** condition when **enable_csqual_pushdown** is enabled and the filter criterion is **IN** for rough check pushdown. If the number of values in the **IN** filter condition exceeds the value of this parameter, the maximum and minimum values in the **IN** filter condition are used for pushdown. This parameter is supported only by clusters of version 8.2.0.101 or later.

**Type**: SUSET

**Value range**: an integer ranging from 0 to 10000

**Default value**: **100**

.. note::

   If the **IN** condition is executed on the only distribution column of a table, values can be filtered on DNs. In this case, the maximum number of values in the **IN** condition is **inlist_rough_check_threshold** multiplied by the number of DNs.

enable_array_optimization
-------------------------

**Parameter description**: whether to split the Array type generated by the IN, ANY, or ALL condition into common expressions for execution. This parameter will support multiple optimizations such as vectorized execution, rough check pruning, and partition pruning. This parameter is supported only by clusters of version 8.2.1 or later.

**Type**: SUSET

**Value range**: Boolean

-  **on** indicates that expressions of the Array type are split for optimization.
-  **off** indicates that expressions of the Array type are not split for optimization.

**Default value**: **on**

max_skew_num
------------

**Parameter description**: controls the number of skew values allowed by the optimizer for redistribution optimization. This parameter is supported only by clusters of version 8.2.1 or later.

**Type**: SUSET

**Value range**: an integer ranging from 0 to INT_MAX

**Default value**: **10**
