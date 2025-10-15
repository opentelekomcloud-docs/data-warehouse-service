:original_name: dws_04_0911.html

.. _dws_04_0911:

Genetic Query Optimizer
=======================

This section describes parameters related to genetic query optimizer. The genetic query optimizer (GEQO) is an algorithm that plans queries by using heuristic searching. This algorithm reduces planning time for complex queries and the cost of producing plans are sometimes inferior to those found by the normal exhaustive-search algorithm.

geqo
----

**Parameter description**: Controls the use of genetic query optimization.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates GEQO is enabled.
-  **off** indicates GEQO is disabled.

**Default value**: **on**

.. important::

   Generally, do not set this parameter to **off**. **geqo_threshold** provides more subtle control of GEQO.

geqo_threshold
--------------

**Parameter description:** Specifies the number of **FROM** items. Genetic query optimization is used to plan queries when the number of statements executed is greater than this value.

**Type**: USERSET

**Value range**: an integer ranging from 2 to INT_MAX

**Default value**: **12**

.. important::

   -  For simpler queries it is best to use the regular, exhaustive-search planner, but for queries with many tables it is better to use GEQO to manage the queries.
   -  A **FULL OUTER JOIN** construct counts as only one **FROM** item.

geqo_effort
-----------

**Parameter description**: Controls the trade-off between planning time and query plan quality in GEQO.

**Type**: USERSET

**Value range**: an integer ranging from 1 to 10

**Default value**: **5**

.. important::

   -  Larger values increase the time spent in query planning, but also increase the probability that an efficient query plan is chosen.
   -  **geqo_effort** does not have direct effect. This parameter is only used to compute the default values for the other variables that influence GEQO behavior. You can manually set other parameters as required.

geqo_pool_size
--------------

**Parameter description**: Specifies the pool size used by GEQO, that is, the number of individuals in the genetic population.

**Type**: USERSET

**Value range**: an integer ranging from 0 to INT_MAX

.. important::

   The value of this parameter must be at least **2**, and useful values are typically from **100** to **1000**. If this parameter is set to **0**, GaussDB(DWS) selects a proper value based on **geqo_effort** and the number of tables.

**Default value**: **0**

geqo_generations
----------------

**Parameter description**: Specifies the number parameter iterations of the algorithm used by GEQO.

**Type**: USERSET

**Value range**: an integer ranging from 0 to INT_MAX

.. important::

   The value of this parameter must be at least **1**, and useful values are typically from **100** to **1000**. If it is set to **0**, a suitable value is chosen based on **geqo_pool_size**.

**Default value**: **0**

geqo_selection_bias
-------------------

**Parameter description**: Specifies the selection bias used by GEQO. The selection bias is the selective pressure within the population.

**Type**: USERSET

**Value range**: a floating point number ranging from 1.5 to 2.0

**Default value**: **2**

geqo_seed
---------

**Parameter description**: Specifies the initial value of the random number generator used by GEQO to select random paths through the join order search space.

**Type**: USERSET

**Value range**: a floating point number ranging from 0.0 to 1.0

.. important::

   Varying the value changes the setting of join paths explored, and may result in a better or worse path being found.

**Default value**: **0**
