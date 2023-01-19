:original_name: dws_04_0909.html

.. _dws_04_0909:

Optimizer Method Configuration
==============================

These configuration parameters provide a crude method of influencing the query plans chosen by the query optimizer. If the default plan chosen by the optimizer for a particular query is not optimal, a temporary solution is to use one of these configuration parameters to force the optimizer to choose a different plan. Better ways include adjusting the optimizer cost constants, manually running **ANALYZE**, increasing the value of the :ref:`default_statistics_target <en-us_topic_0000001098974600__sa5c1527051e54fbdb6c5346d54bcbf5a>` configuration parameter, and adding the statistics collected in a specific column using **ALTER TABLE SET STATISTICS**.

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

**Parameter description**: Controls the use of **stream** in concurrent updates. This parameter is restricted by the :ref:`enable_stream_operator <en-us_topic_0000001099134668__se97ba22fff0144d784f5363903a1f584>` parameter.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that the optimizer can generate stream plans for the **UPDATE** statement.
-  **off** indicates that the optimizer can generate only non-stream plans for the **UPDATE** statement.

**Default value**: **on**

.. _en-us_topic_0000001099134668__se97ba22fff0144d784f5363903a1f584:

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

enable_change_hjcost
--------------------

**Parameter description**: Specifies whether the optimizer excludes internal table running costs when selecting the Hash Join cost path. If it is set to **on**, tables with a few records and high running costs are more possible to be selected.

**Type**: SUSET

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

.. _en-us_topic_0000001099134668__section746841514523:

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

   -  If :ref:`enable_nodegroup_debug <en-us_topic_0000001099134668__section1426622145210>` is set to **off**, the list of candidate computing Node Groups consists of the Node Group where the operator's operation objects are located and the specified Node Group.
   -  If :ref:`enable_nodegroup_debug <en-us_topic_0000001099134668__section1426622145210>` is set to **on**, the specified Node Group is used as the candidate Node Group.

**Default value**: **bind**

.. _en-us_topic_0000001099134668__section1426622145210:

enable_nodegroup_debug
----------------------

**Parameter description**: Specifies whether the optimizer assigns computing workloads to a specific Node Group when multiple Node Groups exist in an environment. The Node Group mechanism is now for internal use only. You do not need to set it.

This parameter takes effect only when :ref:`expected_computing_nodegroup <en-us_topic_0000001099134668__section746841514523>` is set to a specific Node Group.

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

**Value range**: a floating point number ranging from 0 to DBL_MAX

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

**Default value**: **cost_base**

.. _en-us_topic_0000001099134668__section1211182712176:

skew_option
-----------

**Parameter description**: Specifies whether an optimization policy is used

**Type**: USERSET

**Value range**: a string

-  **off**: policy disabled
-  **normal**: radical policy. All possible skews are optimized.
-  **lazy**: conservative policy. Uncertain skews are ignored.

**Default value:** **normal**
