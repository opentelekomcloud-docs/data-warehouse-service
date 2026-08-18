:original_name: dws_04_0463.html

.. _dws_04_0463:

Configuration Parameter Hints
=============================

Function
--------

A hint, or a GUC hint, specifies a configuration parameter value when a plan is generated.

Precautions
-----------

-  If a parameter set by hint takes effect at the statement level, the hint must be written to the top-level query instead of the subquery. For **UNION**, **INTERSECT**, **EXCEPT**, and **MINUS** statements, you can write the GUC hint at the statement level to any **SELECT** clause that participates in the set operation. The configuration parameters set by the GUC hint take effect on each **SELECT** clause that participates in the set operation.
-  When a subquery is pulled up, all GUC hints on the subquery are discarded.
-  If a parameter is set by both the statement-level GUC hint and the subquery-level GUC hint, the subquery-level GUC hint takes effect in the corresponding subquery, and the statement-level GUC hint takes effect in other subqueries of the statement.

Syntax
------

.. code-block:: console

   set [global]([@block_name] guc_name guc_value)

Parameters
----------

-  **global** indicates that the parameter set by hint takes effect at the statement level. If **global** is not specified, the parameter takes effect only in the subquery where the hint is located.
-  *block_name* indicates the block name of the statement block. For details, see :ref:`block_name <en-us_topic_0000002080515442__en-us_topic_0000001460722632_li99021444551>`.
-  **guc_name** indicates the name of the configuration parameter specified by hint.

-  **guc_value** indicates the value of a configuration parameter specified by hint.

Currently, GUC hints support only some configuration parameters. Some parameters cannot be configured at the subquery level and can only be configured at the statement level. The following table lists the supported parameters.

.. table:: **Table 1** Configuration parameters supported by GUC hints

   +-----------------------------------+-------------------------------------------+
   | Parameter                         | Configured at the Subquery Level (Yes/No) |
   +===================================+===========================================+
   | agg_max_mem                       | Yes                                       |
   +-----------------------------------+-------------------------------------------+
   | agg_redistribute_enhancement      | Yes                                       |
   +-----------------------------------+-------------------------------------------+
   | best_agg_plan                     | Yes                                       |
   +-----------------------------------+-------------------------------------------+
   | cost_model_version                | No                                        |
   +-----------------------------------+-------------------------------------------+
   | cost_param                        | No                                        |
   +-----------------------------------+-------------------------------------------+
   | enable_array_optimization         | No                                        |
   +-----------------------------------+-------------------------------------------+
   | enable_bitmapscan                 | Yes                                       |
   +-----------------------------------+-------------------------------------------+
   | enable_broadcast                  | Yes                                       |
   +-----------------------------------+-------------------------------------------+
   | enable_csqual_pushdown            | No                                        |
   +-----------------------------------+-------------------------------------------+
   | enable_redistribute               | Yes                                       |
   +-----------------------------------+-------------------------------------------+
   | enable_extrapolation_stats        | Yes                                       |
   +-----------------------------------+-------------------------------------------+
   | enable_fast_query_shipping        | No                                        |
   +-----------------------------------+-------------------------------------------+
   | enable_force_vector_engine        | No                                        |
   +-----------------------------------+-------------------------------------------+
   | enable_hashagg                    | Yes                                       |
   +-----------------------------------+-------------------------------------------+
   | enable_hashfilter                 | No                                        |
   +-----------------------------------+-------------------------------------------+
   | enable_hashjoin                   | Yes                                       |
   +-----------------------------------+-------------------------------------------+
   | enable_index_nestloop             | Yes                                       |
   +-----------------------------------+-------------------------------------------+
   | enable_indexonlyscan              | Yes                                       |
   +-----------------------------------+-------------------------------------------+
   | enable_indexscan                  | Yes                                       |
   +-----------------------------------+-------------------------------------------+
   | enable_join_pseudoconst           | Yes                                       |
   +-----------------------------------+-------------------------------------------+
   | enable_mergejoin                  | Yes                                       |
   +-----------------------------------+-------------------------------------------+
   | enable_mixedagg                   | No                                        |
   +-----------------------------------+-------------------------------------------+
   | enable_nestloop                   | Yes                                       |
   +-----------------------------------+-------------------------------------------+
   | enable_nodegroup_debug            | No                                        |
   +-----------------------------------+-------------------------------------------+
   | enable_partition_dynamic_pruning  | Yes                                       |
   +-----------------------------------+-------------------------------------------+
   | enable_seqscan                    | Yes                                       |
   +-----------------------------------+-------------------------------------------+
   | enable_sonic_hashagg              | No                                        |
   +-----------------------------------+-------------------------------------------+
   | enable_sonic_hashjoin             | Yes                                       |
   +-----------------------------------+-------------------------------------------+
   | enable_sort                       | Yes                                       |
   +-----------------------------------+-------------------------------------------+
   | enable_stream_ctescan             | No                                        |
   +-----------------------------------+-------------------------------------------+
   | enable_tidscan                    | Yes                                       |
   +-----------------------------------+-------------------------------------------+
   | enable_value_redistribute         | Yes                                       |
   +-----------------------------------+-------------------------------------------+
   | enable_vector_engine              | No                                        |
   +-----------------------------------+-------------------------------------------+
   | expected_computing_nodegroup      | No                                        |
   +-----------------------------------+-------------------------------------------+
   | force_bitmapand                   | Yes                                       |
   +-----------------------------------+-------------------------------------------+
   | from_collapse_limit               | Yes                                       |
   +-----------------------------------+-------------------------------------------+
   | join_collapse_limit               | Yes                                       |
   +-----------------------------------+-------------------------------------------+
   | join_num_distinct                 | Yes                                       |
   +-----------------------------------+-------------------------------------------+
   | outer_join_max_rows_multipler     | Yes                                       |
   +-----------------------------------+-------------------------------------------+
   | prefer_hashjoin_path              | No                                        |
   +-----------------------------------+-------------------------------------------+
   | qrw_inlist2join_optmode           | Yes                                       |
   +-----------------------------------+-------------------------------------------+
   | qual_num_distinct                 | Yes                                       |
   +-----------------------------------+-------------------------------------------+
   | query_dop                         | No                                        |
   +-----------------------------------+-------------------------------------------+
   | query_max_mem                     | No                                        |
   +-----------------------------------+-------------------------------------------+
   | query_mem                         | No                                        |
   +-----------------------------------+-------------------------------------------+
   | rewrite_rule                      | No                                        |
   +-----------------------------------+-------------------------------------------+
   | setop_optmode                     | Yes                                       |
   +-----------------------------------+-------------------------------------------+
   | skew_option                       | Yes                                       |
   +-----------------------------------+-------------------------------------------+
   | stream_ctescan_max_estimate_mem   | No                                        |
   +-----------------------------------+-------------------------------------------+
   | stream_ctescan_pred_threshold     | No                                        |
   +-----------------------------------+-------------------------------------------+
   | stream_ctescan_refcount_threshold | No                                        |
   +-----------------------------------+-------------------------------------------+
   | windowagg_pushdown_enhancement    | No                                        |
   +-----------------------------------+-------------------------------------------+
   | index_selectivity_cost            | Yes                                       |
   +-----------------------------------+-------------------------------------------+
   | index_cost_limit                  | Yes                                       |
   +-----------------------------------+-------------------------------------------+

Examples
--------

Hint the query plan in :ref:`Examples <en-us_topic_0000002053159594__en-us_topic_0000001658028034_section671421102912>` as follows:

.. code-block::

   explain
   select /*+ set global(query_dop 0) */ i_product_name product_name
   ...

This hint indicates that the **query_dop** parameter is set to **0** when the plan for a statement is generated, which means the SMP adaptation function is enabled. The generated plan is as follows:

|image1|

.. |image1| image:: /_static/images/en-us_image_0000001811610593.png
