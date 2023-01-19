:original_name: dws_04_0463.html

.. _dws_04_0463:

Configuration Parameter Hints
=============================

Function
--------

A hint, or a GUC hint, specifies a configuration parameter value when a plan is generated. Currently, only the following parameters are supported:

-  agg_redistribute_enhancement
-  best_agg_plan
-  enable_fast_query_shipping
-  enable_hashagg
-  enable_hashjoin
-  enable_indexscan
-  enable_nestloop
-  enable_nodegroup_debug
-  expected_computing_nodegroup
-  qrw_inlist2join_optmode
-  query_dop
-  rewrite_rule
-  skew_option

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
   -  The **enable_fast_query_shipping**, **enable_nodegroup_debug**, **expected_computing_nodegroup**, **query_dop**, and **rewrite_rule** parameters can be set only at the statement level.
   -  If a parameter is set by both the statement-level GUC hint and the subquery-level GUC hint, the subquery-level GUC hint takes effect in the corresponding subquery, and the statement-level GUC hint takes effect in other subqueries of the statement.

Example
-------

Hint the query plan in :ref:`Examples <en-us_topic_0000001098974750__section671421102912>` as follows:

.. code-block::

   explain
   select /*+ set global(query_dop 0) */ i_product_name product_name
   ...

This hint indicates that the **query_dop** parameter is set to **0** when the plan for a statement is generated, which means the SMP adaptation function is enabled. The generated plan is as follows:

|image1|

.. |image1| image:: /_static/images/en-us_image_0000001145695193.png
