:original_name: dws_04_0463.html

.. _dws_04_0463:

Configuration Parameter Hints
=============================

Function
--------

A hint, or a GUC hint, specifies a configuration parameter value when a plan is generated.

Syntax
------

.. code-block::

   set [global](guc_name guc_value)

Parameters
----------

-  **global** indicates that the parameter set by hint takes effect at the statement level. If **global** is not specified, the parameter takes effect only in the subquery where the hint is located.
-  **guc_name** indicates the name of the configuration parameter specified by hint.

-  **guc_value** indicates the value of a configuration parameter specified by hint.

.. note::

   -  If a parameter set by hint takes effect at the statement level, the hint must be written to the top-level query instead of the subquery. For **UNION**, **INTERSECT**, **EXCEPT**, and **MINUS** statements, you can write the GUC hint at the statement level to any **SELECT** clause that participates in the set operation. The configuration parameters set by the GUC hint take effect on each **SELECT** clause that participates in the set operation.
   -  When a subquery is pulled up, all GUC hints on the subquery are discarded.
   -  If a parameter is set by both the statement-level GUC hint and the subquery-level GUC hint, the subquery-level GUC hint takes effect in the corresponding subquery, and the statement-level GUC hint takes effect in other subqueries of the statement.

Currently, GUC hints support only some configuration parameters. Some parameters cannot be configured at the subquery level and can only be configured at the statement level. The following table lists the supported parameters.

.. table:: **Table 1** Configuration parameters supported by GUC hints

   +----------------------------------+-------------------------------------------+
   | Parameter                        | Configured at the Subquery Level (Yes/No) |
   +==================================+===========================================+
   | agg_redistribute_enhancement     | Yes                                       |
   +----------------------------------+-------------------------------------------+
   | best_agg_plan                    | Yes                                       |
   +----------------------------------+-------------------------------------------+
   | cost_model_version               | No                                        |
   +----------------------------------+-------------------------------------------+
   | cost_param                       | No                                        |
   +----------------------------------+-------------------------------------------+
   | enable_bitmapscan                | Yes                                       |
   +----------------------------------+-------------------------------------------+
   | enable_broadcast                 | Yes                                       |
   +----------------------------------+-------------------------------------------+
   | enable_extrapolation_stats       | Yes                                       |
   +----------------------------------+-------------------------------------------+
   | enable_fast_query_shipping       | No                                        |
   +----------------------------------+-------------------------------------------+
   | enable_force_vector_engine       | No                                        |
   +----------------------------------+-------------------------------------------+
   | enable_hashagg                   | Yes                                       |
   +----------------------------------+-------------------------------------------+
   | enable_hashjoin                  | Yes                                       |
   +----------------------------------+-------------------------------------------+
   | enable_index_nestloop            | Yes                                       |
   +----------------------------------+-------------------------------------------+
   | enable_indexscan                 | Yes                                       |
   +----------------------------------+-------------------------------------------+
   | enable_join_pseudoconst          | Yes                                       |
   +----------------------------------+-------------------------------------------+
   | enable_nestloop                  | Yes                                       |
   +----------------------------------+-------------------------------------------+
   | enable_nodegroup_debug           | No                                        |
   +----------------------------------+-------------------------------------------+
   | enable_partition_dynamic_pruning | Yes                                       |
   +----------------------------------+-------------------------------------------+
   | enable_sort                      | Yes                                       |
   +----------------------------------+-------------------------------------------+
   | enable_vector_engine             | No                                        |
   +----------------------------------+-------------------------------------------+
   | expected_computing_nodegroup     | No                                        |
   +----------------------------------+-------------------------------------------+
   | force_bitmapand                  | Yes                                       |
   +----------------------------------+-------------------------------------------+
   | from_collapse_limit              | Yes                                       |
   +----------------------------------+-------------------------------------------+
   | join_collapse_limit              | Yes                                       |
   +----------------------------------+-------------------------------------------+
   | join_num_distinct                | Yes                                       |
   +----------------------------------+-------------------------------------------+
   | qrw_inlist2join_optmode          | Yes                                       |
   +----------------------------------+-------------------------------------------+
   | qual_num_distinct                | Yes                                       |
   +----------------------------------+-------------------------------------------+
   | query_dop                        | No                                        |
   +----------------------------------+-------------------------------------------+
   | rewrite_rule                     | No                                        |
   +----------------------------------+-------------------------------------------+
   | skew_option                      | Yes                                       |
   +----------------------------------+-------------------------------------------+

Examples
--------

Hint the query plan in :ref:`Examples <en-us_topic_0000001188642062__section671421102912>` as follows:

.. code-block::

   explain
   select /*+ set global(query_dop 0) */ i_product_name product_name
   ...

This hint indicates that the **query_dop** parameter is set to **0** when the plan for a statement is generated, which means the SMP adaptation function is enabled. The generated plan is as follows:

|image1|

.. |image1| image:: /_static/images/en-us_image_0000001188642274.png
