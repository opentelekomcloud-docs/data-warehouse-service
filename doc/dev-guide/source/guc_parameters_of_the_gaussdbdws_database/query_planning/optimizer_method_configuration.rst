:original_name: dws_04_0909.html

.. _dws_04_0909:

Optimizer Method Configuration
==============================

These configuration parameters provide a crude method of influencing the query plans chosen by the query optimizer. If the default plan chosen by the optimizer for a particular query is not optimal, a temporary solution is to use one of these configuration parameters to force the optimizer to choose a different plan. Better ways include adjusting the optimizer cost constants, manually running **ANALYZE**, increasing the value of the :ref:`default_statistics_target <en-us_topic_0000001764650460__sa5c1527051e54fbdb6c5346d54bcbf5a>` configuration parameter, and adding the statistics collected in a specific column using **ALTER TABLE SET STATISTICS**.

enable_bitmapscan
-----------------

**Parameter description**: Controls whether the query optimizer uses the bitmap-scan plan type.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates it is enabled.
-  **off** indicates it is disabled.

**Default value**: **on**

enable_hashagg
--------------

**Parameter description**: Controls whether the query optimizer uses the Hash aggregation plan type.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates it is enabled.
-  **off** indicates it is disabled.

**Default value**: **on**

enable_mixedagg
---------------

**Parameter description**: Controls whether the query optimizer uses the Mixed Agg plan type.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that a Mixed Agg query plan is generated for the Grouping Sets statement (including Rollup or Cube) that meets certain conditions.
-  **off** indicates it is disabled.

**Default value**: **on**

.. important::

   -  The default value of this parameter is **on** in a newly installed cluster of 9.1.0.200 or later. In an upgrade scenario, the default value of this parameter is retained for forward compatibility.

   -  The Mixed Agg query plan can be used to improve the performance of statements dealing with a large amount of data (the data volume of a single DN table is greater than 100 GB).

      Mixed Agg is not supported in the following scenarios:

      -  The data type of the columns in the **GROUP BY** clause do not support hashing.
      -  The aggregate function uses **DISTINCT** for deduplication or **ORDER BY** for sorting.
      -  The **GROUPING SETS** clause does not contain empty groups.

enable_hashjoin
---------------

**Parameter description**: Controls whether the query optimizer uses the Hash-join plan type.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates it is enabled.
-  **off** indicates it is disabled.

**Default value**: **on**

enable_indexscan
----------------

**Parameter description**: Controls whether the query optimizer uses the index-scan plan type.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates it is enabled.
-  **off** indicates it is disabled.

**Default value**: **on**

enable_indexonlyscan
--------------------

**Parameter description**: Controls whether the query optimizer uses the index-only-scan plan type.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates it is enabled.
-  **off** indicates it is disabled.

**Default value**: **on**

enable_material
---------------

**Parameter description**: Controls whether the query optimizer uses materialization. It is impossible to suppress materialization entirely, but setting this parameter to **off** prevents the optimizer from inserting materialized nodes.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates it is enabled.
-  **off** indicates it is disabled.

**Default value**: **on**

enable_mergejoin
----------------

**Parameter description**: Controls whether the query optimizer uses the merge-join plan type.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates it is enabled.
-  **off** indicates it is disabled.

**Default value**: **off**

enable_nestloop
---------------

**Parameter description**: Controls whether the query optimizer uses the nested-loop join plan type to fully scan internal tables. It is impossible to suppress nested-loop joins entirely, but setting this parameter to **off** allows the optimizer to choose other methods if available.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates it is enabled.
-  **off** indicates it is disabled.

**Default value**: **off**

enable_index_nestloop
---------------------

**Parameter description**: Controls whether the query optimizer uses the nested-loop join plan type to scan the parameterized indexes of internal tables.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates the query optimizer uses the nested-loop join plan type.
-  **off** indicates the query optimizer does not use the nested-loop join plan type.

**Default value**: The default value for a newly installed cluster is **on**. If the cluster is upgraded from R8C10, the forward compatibility is retained. If the version is upgraded from R7C10 or an earlier version, the default value is **off**.

left_join_estimation_enhancement
--------------------------------

**Parameter description**: Specifies whether to use the optimized estimated number of rows for left join. This parameter is supported only by clusters of version 8.3.0.100 or later.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that the optimized value is used.
-  **off** indicates it is disabled.

**Default value**: **off**

enable_seqscan
--------------

**Parameter description**: Controls whether the query optimizer uses the sequential scan plan type. It is impossible to suppress sequential scans entirely, but setting this variable to **off** allows the optimizer to preferentially choose other methods if available.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates it is enabled.
-  **off** indicates it is disabled.

**Default value**: **on**

enable_sort
-----------

**Parameter description**: Controls whether the query optimizer uses the sort method. It is impossible to suppress explicit sorts entirely, but setting this variable to **off** allows the optimizer to preferentially choose other methods if available.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates it is enabled.
-  **off** indicates it is disabled.

**Default value**: **on**

max_opt_sort_rows
-----------------

**Parameter description**: Specifies the maximum number of optimized limit+offset rows in an ORDER BY clause. This parameter is supported only by clusters of version 8.3.0 or later.

**Type**: USERSET

**Value range**: an integer ranging from **0** to **INT_MAX**

-  If the value is **0**, the parameter does not take effect.
-  If this parameter is set to any other value, the optimization takes effect when the number of limit+offset rows in the ORDER BY clause is less than the value of this parameter. If the number of limit+offset rows in the order by clause is greater than the value of this parameter, the optimization does not take effect. After the optimization, the time required is reduced, but the memory usage may increase.

**Default value**: **0**

enable_tidscan
--------------

**Parameter description**: Controls whether the query optimizer uses the Tuple ID (TID) scan plan type.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates it is enabled.
-  **off** indicates it is disabled.

**Default value**: **on**

enable_kill_query
-----------------

**Parameter description**: In CASCADE mode, when a user is deleted, all the objects belonging to the user are deleted. This parameter specifies whether the queries of the objects belonging to the user can be unlocked when the user is deleted.

**Type**: SUSET

**Value range**: Boolean

-  **on** indicates the unlocking is allowed.
-  **off** indicates the unlocking is not allowed.

**Default value**: **off**

enforce_oracle_behavior
-----------------------

**Parameter description**: Controls the rule matching modes of regular expressions.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that the ORACLE matching rule is used.
-  **off** indicates that the POSIX matching rule is used.

**Default value**: **on**

enable_stream_concurrent_update
-------------------------------

**Parameter description**: Controls the use of **stream** in concurrent updates. This parameter is restricted by the :ref:`enable_stream_operator <en-us_topic_0000001811609661__se97ba22fff0144d784f5363903a1f584>` parameter.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that the optimizer can generate stream plans for the **UPDATE** statement.
-  **off** indicates that the optimizer can generate only non-stream plans for the **UPDATE** statement.

**Default value**: **on**

enable_stream_ctescan
---------------------

**Parameter description**: Specifies whether a stream plan supports **ctescan**.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that **ctescan** is supported for the stream plan.
-  **off** indicates that **ctescan** is not supported for the stream plan.

**Default value**: **off**

.. _en-us_topic_0000001811609661__se97ba22fff0144d784f5363903a1f584:

enable_stream_operator
----------------------

**Parameter description:** Controls whether the query optimizer uses streams.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates it is enabled.
-  **off** indicates it is disabled.

**Default value**: **on**

enable_stream_recursive
-----------------------

**Parameter description**: Specifies whether to push **WITH RECURSIVE** join queries to DNs for processing.

**Type**: USERSET

**Value range**: Boolean

-  **on**: **WITH RECURSIVE** join queries will be pushed down to DNs.
-  **off**: **WITH RECURSIVE** join queries will not be pushed down to DNs.

**Default value**: **on**

enable_value_redistribute
-------------------------

**Parameter description**: Specifies whether to generate value redistribute plans. In 8.2.0 and later cluster versions, this parameter takes effect for **rank**, **dense_rank**, and **row_number** without the **PARTITION BY** clause.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that value redistribute plans are generated.
-  **off** indicates that no value redistribute plans are generated.

**Default value**: **on**

max_recursive_times
-------------------

**Parameter description**: Specifies the maximum number of **WITH RECURSIVE** iterations.

**Type**: USERSET

**Value range**: an integer ranging from 0 to INT_MAX

**Default value**: **200**

enable_vector_engine
--------------------

**Parameter description**: Controls whether the query optimizer uses the vectorized executor.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates it is enabled.
-  **off** indicates it is disabled.

**Default value**: **on**

enable_broadcast
----------------

**Parameter description**: Controls whether the query optimizer uses the broadcast distribution method when it evaluates the cost of stream.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates it is enabled.
-  **off** indicates it is disabled.

**Default value**: **on**

enable_redistribute
-------------------

**Parameter description**: Controls whether the query optimizer uses the local redistribute or split redistribute distribution method when estimating the cost of streams. This parameter is supported only by clusters of version 8.3.0 or later.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that either of the distribution methods is used.
-  **off** indicates that none of the distribution methods is used.

**Default value**: **on**

enable_change_hjcost
--------------------

**Parameter description**: Specifies whether the optimizer excludes internal table running costs when selecting the Hash Join cost path. If it is set to **on**, tables with a few records and high running costs are more possible to be selected.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates it is enabled.
-  **off** indicates it is disabled.

**Default value**: **off**

enable_fstream
--------------

**Parameter description**: Controls whether the query optimizer uses streams when it delivers statements. This parameter is only used for external HDFS tables.

This parameter has been discarded. To reserve forward compatibility, set this parameter to **on**, but the setting does not make a difference.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates it is enabled.
-  **off** indicates it is disabled.

**Default value**: **off**

enable_hashfilter
-----------------

**Parameter description**: Controls whether hashfilters can be generated for plans that contain replication tables (including dual and constant tables). This parameter is supported by clusters of version 8.2.0 or later.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that hashfilters can be generated.
-  **off** indicates that no hashfilters can be generated.

**Default value**: **on**

best_agg_plan
-------------

**Parameter description**: The query optimizer generates three plans for the aggregate operation under the stream:

#. hashagg+gather(redistribute)+hashagg
#. redistribute+hashagg(+gather)
#. hashagg+redistribute+hashagg(+gather).

This parameter is used to control the query optimizer to generate which type of hashagg plans.

**Type**: USERSET

**Value range**: an integer ranging from 0 to 3.

-  When the value is set to **1**, the first plan is forcibly generated.
-  When the value is set to **2** and if the **group by** column can be redistributed, the second plan is forcibly generated. Otherwise, the first plan is generated.
-  When the value is set to **3** and if the **group by** column can be redistributed, the third plan is generated. Otherwise, the first plan is generated.
-  When the value is set to **0**, the query optimizer chooses the most optimal plan based on the estimated costs of the three plans above.

**Default value**: **0**

turbo_engine_version
--------------------

**Parameter description**: For tables with the turbo storage format specified during table creation (by setting the **enable_turbo_store** parameter to **on** in the table properties), and when the query does not involve merge join or sort agg operators, the executor can use the turbo execution engine, which can significantly improve performance.

**Type**: USERSET

**Value range**: an integer ranging from 0 to 3.

-  The value **0** indicates that the turbo execution engine is disabled.
-  The value **1** indicates that the turbo execution engine is only used for single-table aggregate queries.
-  The value **2** indicates that the turbo execution engine is only used for single-table aggregate or multi-table join queries.
-  The value **3** indicates that the turbo execution engine can be used to accelerate most commonly used operators, except for operators such as merge join and sort agg. When the data volume is large and **turbo_engine_version** is set to **3**, the occurrence of merge join and sort agg operators is relatively rare, so turbo execution engine acceleration can be achieved for almost SQL statements.

**Default value**: **0**

.. important::

   You are advised not to enable the turbo execution engine in cross-VW scenarios.

enable_bucket_stream_opt
------------------------

**Parameter description**: Specifies whether to use the **bucket agg** and **bucket join** policies for level-2 partitioned tables or 3.0 hash distributed tables. It speeds up SQL statement execution by avoiding local data redistribution or broadcast. This is supported only by clusters of version 9.1.0.200 or later.

**Type**: USERSET

**Value range**: Boolean

-  **true**: The optimizer uses the **bucket agg** and **bucket join** execution policies to generate plans when the conditions for the policy to be applied are met. If this optimization policy is used, "Bucket Stream: true" is displayed at the end of the **EXPLAIN** statement.
-  **false**: The optimizer does not use the **bucket agg** and **bucket join** execution policies to generate plans.

**Default value**: **true**

.. important::

   -  The default value of this parameter is **true** in a newly installed cluster of 9.1.0.200 or later. In an upgrade scenario, the default value of this parameter is retained for forward compatibility.

   -  The **bucket agg** and **bucket join** execution policies take effect only when the current query has 16 or fewer available CPUs and meets one of the following conditions:

      1. The distribution column for level-2 partitions must match the **secondary_part_column** of these partitions. It is recommended that the number of level-2 partitions be the number of DNs multiplied by 12. Supported multiples include 4, 6, 8, 12, and 16.

      2. Tables in version 3.0 must use hash distribution, with the number of buckets or DNs exceeding 10.

   -  If the local stream cost in the plan is low, the query may not select the **bucket agg** and **bucket join** policies.

spill_compression
-----------------

**Parameter description**: Specifies the compression algorithm used when the executor operator runs out of memory and needs to spill data to disk. This is supported only by clusters of version 9.1.0.100 or later.

**Type**: USERSET

**Value range**: enumerated values

-  **'lz4'** indicates that the lz4 compression algorithm is used, which provides better performance for scenarios with smaller spill volumes, but requires more storage space.
-  **'zstd'** indicates that the zstd compression algorithm is used, which provides better performance for scenarios with larger spill volumes where I/O is the main bottleneck, and requires approximately 2/3 of the storage space used by lz4.

**Default value**: **'lz4'**

index_selectivity_cost
----------------------

**Parameter description**: Controls the cost calculation of cbtree when scanning column-store table indexes (for selectivity > 0.001). This parameter is only supported by clusters of version 8.2.1.100 or later.

**Type**: USERSET

**Value range**: a floating point number, which can be -1 or ranges from 0 to 1000.

-  If this parameter is set to **0**, the index selection rate is not affected by the threshold 0.001.
-  If the value is **-1**, the value is impacted by **disable_cost**.
-  When it is set to other values, the value is the coefficient for cbtree cost calculation.

**Default value**: **-1**

index_cost_limit
----------------

**Parameter description**: threshold for disabling the cost calculation of cbtree during column-store table index scanning. This parameter is supported only by clusters of version 8.2.1.100 or later.

**Type**: USERSET

**Value range**: an integer ranging from 0 to 2147483647

-  If the value is **0**, the parameter does not take effect.
-  If this parameter is set to other values and the number of rows in a table is less than the value of this parameter, the table is not affected by the index selection rate threshold 0.001.

**Default value**: **0**

volatile_shipping_version
-------------------------

**Parameter description**: Controls the execution scope of volatile functions to be pushed down.

**Type**: USERSET

**Value range**: **0**, **1**, **2**, **3**

-  When set to **3**, it extends the support for pushing down InlineCTE when it is only referenced once, on top of the support provided by a value **2**. It also extends the support for pushing down the use of volatile functions in UPSERT operations involving replicated tables.
-  When the value is **2**, pushdown can be performed when VOLATILE functions are contained in the target column of the copied CTE result.
-  If this parameter is set to **1**, the **nextval**, **uuid_generate_v1**, **sys_guid**, and **uuid** functions can be completely pushed down if they are in the target column of a statement.
-  If this parameter is set to **0**, random functions can be completely pushed down. The **nextval** and **uuid_generate_v1** functions can be pushed down only if **INSERT** contains simple query statements.

**Default value**: **3**

agg_redistribute_enhancement
----------------------------

**Parameter description**: When the aggregate operation is performed, which contains multiple **group by** columns and all of the columns are not in the distribution column, you need to select one **group by** column for redistribution. This parameter controls the policy of selecting a redistribution column.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates the column that can be redistributed and evaluates the most distinct value for redistribution.
-  **off** indicates the first column that can be redistributed for redistribution.

**Default value**: **off**

enable_valuepartition_pruning
-----------------------------

**Parameter description**: Specifies whether the DFS partitioned table is dynamically or statically optimized.

**Type**: USERSET

**Value range:** Boolean

-  **on** indicates that the DFS partitioned table is dynamically or statically optimized.
-  **off** indicates that the DFS partitioned table is not dynamically or statically optimized.

**Default value**: **on**

.. _en-us_topic_0000001811609661__section746841514523:

expected_computing_nodegroup
----------------------------

**Parameter description**: Specifies a computing Node Group or the way to choose such a group. The Node Group mechanism is now for internal use only. You do not need to set it.

During join or aggregation operations, a Node Group can be selected in four modes. In each mode, the specified candidate computing Node Groups are listed for the optimizer to select an appropriate one for the current operator.

**Type**: USERSET

**Value range**: a string

-  **optimal**: The list of candidate computing Node Groups consists of the Node Group where the operator's operation objects are located and the DNs in the Node Groups on which the current user has the COMPUTE permission.
-  **query**: The list of candidate computing Node Groups consists of the Node Group where the operator's operation objects are located and the DNs in the Node Groups where base tables involved in the query are located.
-  **bind**: If the current session user is a logical cluster user, the candidate computing Node Group is the Node Group of the logical cluster associated with the current user. If the session user is not a logical cluster user, the candidate computing Node Group selection rule is the same as that when this parameter is set to **query**.
-  Node Group name:

   -  If :ref:`enable_nodegroup_debug <en-us_topic_0000001811609661__section1426622145210>` is set to **off**, the list of candidate computing Node Groups consists of the Node Group where the operator's operation objects are located and the specified Node Group.
   -  If :ref:`enable_nodegroup_debug <en-us_topic_0000001811609661__section1426622145210>` is set to **on**, the specified Node Group is used as the candidate Node Group.

**Default value**: **bind**

.. _en-us_topic_0000001811609661__section1426622145210:

enable_nodegroup_debug
----------------------

**Parameter description**: Specifies whether the optimizer assigns computing workloads to a specific Node Group when multiple Node Groups exist in an environment. The Node Group mechanism is now for internal use only. You do not need to set it.

This parameter takes effect only when :ref:`expected_computing_nodegroup <en-us_topic_0000001811609661__section746841514523>` is set to a specific Node Group.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that computing workloads are assigned to the Node Group specified by **expected_computing_nodegroup**.
-  **off** indicates no Node Group is specified to compute.

**Default value**: **off**

stream_multiple
---------------

**Parameter description**: Specifies the weight used for optimizer to calculate the final cost of stream operators.

The base stream cost is multiplied by this weight to make the final cost.

**Type**: USERSET

**Value range**: a floating point number ranging from 0 to 10000

**Default value**: **1**

.. important::

   This parameter is applicable only to Redistribute and Broadcast streams.

qrw_inlist2join_optmode
-----------------------

**Parameter description**: Specifies whether enable inlist-to-join (inlist2join) query rewriting.

**Type**: USERSET

**Value range**: a string

-  **disable**: inlist2join disabled
-  **cost_base**: cost-based inlist2join query rewriting
-  **rule_base**: forcible rule-based inlist2join query rewriting
-  A positive integer: threshold of Inlist2join query rewriting. If the number of elements in the list is greater than the threshold, the rewriting is performed.

**Default value**: **disable**

enable_inlist_hashing
---------------------

**Parameter description**: Specifies whether to use inlist hash optimization. This parameter is supported only by clusters of version 9.1.0 or later.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that inlist hash optimization is enabled.
-  **off** indicates that inlist hash optimization is disabled.

**Default value**: **on**

setop_optmode
-------------

**Parameter description**: Specifies whether to perform deduplication on the query branch statements of a set operation (**UNION**/**EXCEPT**/**INTERSECT**) without the **ALL** option.

**Type**: USERSET

**Value range**: enumerated values

-  **disable**: The query branch does not perform deduplication.
-  **force**: The query branch forcibly performs deduplication.
-  **cost**: The optimizer evaluates the costs of query branches with and without deduplication and selects the execution mode with the lower cost.

**Default value**: **cost**

.. important::

   -  The default value of this parameter is **cost** in a newly installed cluster of 9.1.0.200 or later. In an upgrade scenario, the default value of this parameter is retained for forward compatibility.
   -  This parameter takes effect only if the execution plan of a SQL statement meets the following conditions:

      -  The **UNION**, **EXCEPT**, and **INTERSECT** operations in the SQL statement do not contain the **ALL** option.
      -  Data redistribution has been performed on the query branches where the set operation is to be performed.

.. _en-us_topic_0000001811609661__section1211182712176:

skew_option
-----------

**Parameter description**: Specifies whether an optimization policy is used

**Type**: USERSET

**Value range**: a string

-  **off**: policy disabled
-  **normal**: radical policy. All possible skews are optimized.
-  **lazy**: conservative policy. Uncertain skews are ignored.

**Default value:** **normal**

enable_expr_skew_optimization
-----------------------------

**Parameter description**: Specifies whether to use expression statistics in the skew optimization policy. This is supported only by clusters of version 9.1.0.100 or later.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that expression statistics are used to determine whether data skew occurs in the skew optimization policy.
-  **off** indicates that expression statistics are not used to determine whether data skew occurs in the skew optimization policy.

**Default value**: **on**

prefer_hashjoin_path
--------------------

**Parameter description**: whether to preferentially generate hashjoin paths so that other paths with high costs can be pre-pruned to shorten the overall plan generation time. This parameter is supported only by clusters of version 8.2.1 or later.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that the optimization of generating hash join paths in advance is enabled.
-  **off** indicates that the optimization of generating hash join paths in advance is disabled.

**Default value**: **on**

enable_hashfilter_test
----------------------

**Parameter description**: whether to add hash filters to columns for base table scan to check whether the results meet expectations. In addition, this parameter determines whether to check the DN accuracy when data is inserted (that is, whether the current data should be inserted into the current DN).

**Type**: USERSET

**Value range**: Boolean

-  **on** adds a hash filter for the distribution column to the base table scan and performs accurate DN verification during data insertion.
-  **off** does not add a hash filter for the distribution column to the base table scan and does not perform DN verification during data insertion.

**Default value**: **on**

.. important::

   -  This parameter is valid only for tables distributed in hash mode.
   -  If this parameter is set to **on**, DN accuracy is verified during data insertion, affecting data insertion performance.

enable_cu_align_8k
------------------

**Parameter description**: Specifies whether to set the CUs in V3 tables to 8 KB. This parameter is supported only by clusters of version 9.1.0 or later.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that the CUs in V3 tables are set to 8,192 bytes.
-  **off** indicates that the CUs in V3 tables are set to 512 bytes.

**Default value**: **off**

enable_cu_batch_insert
----------------------

**Parameter description**: Specifies whether to enable the multi-column CU batch write feature for V2 tables. This parameter is supported only by clusters of version 9.1.0 or later.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that the multi-column CU batch write feature is enabled for V2 tables.
-  **off** indicates that the multi-column CU batch write feature is disabled for V2 tables.

**Default value**: **off**

enable_topk_optimization
------------------------

**Parameter description**: Specifies whether to enable Top K sorting optimization. This is supported only by clusters of version 9.1.0.200 or later.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that Top K sorting optimization is enabled.
-  **off** indicates that Top K sorting optimization is disabled.

**Default value**: **on**

late_read_strategy
------------------

**Parameter description**: Specifies whether to use the late materialization feature. This is supported only by clusters of version 9.1.0.200 or later.

**Type**: USERSET

**Value range**: enumerated values

-  **topk**: enables the late materialization optimization method for statements that involve both sorting and limiting.
-  **none**: indicates that the late materialization optimization method is not used.

**Default value**: **topk**
