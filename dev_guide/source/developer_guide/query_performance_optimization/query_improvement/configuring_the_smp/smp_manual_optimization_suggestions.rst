:original_name: dws_04_0473.html

.. _dws_04_0473:

SMP Manual Optimization Suggestions
===================================

To manually optimize SMP, you need to be familiar with :ref:`Suggestions for SMP Parameter Settings <en-us_topic_0000001100991256>`. This section describes how to optimize SMP.

Constraints
-----------

The CPU, memory, I/O, and network bandwidth resources are sufficient. The SMP architecture uses abundant resources to save time. After the plan parallelism is executed, resource consumption increases. When these resources become a bottleneck, SMP may deteriorate, rather than improve performance. In addition, it takes a longer time to generate SMP plans than serial plans. Therefore, in TP services that mainly involve short queries or in case resources are insufficient, you are advised to disable SMP by setting **query_dop** to **1**.

Procedure
---------

#. Observe the current system load situation. If the resource is sufficient (the resource usage ratio is smaller than 50%), perform step 2. Otherwise, exit this system.

#. Set **query_dop** to **1** (default value). Use **explain** to generate an execution plan and check whether the plan can be used in scenarios in :ref:`Application Scenarios and Restrictions <en-us_topic_0000001147271175>`. If the plan can be used, go to the next step.

#. Set **query_dop=-**\ *value*. The value range of the parallelism degree is [1, *value*].

#. Set **query_dop=**\ *value*. The parallelism degree is 1 or *value*.

#. Before the query statement is executed, set **query_dop** to an appropriate value. After the statement is executed, set **query_dop** to **off**. For example:

   ::

      SET query_dop = 0;
      SELECT COUNT(*) FROM t1 GROUP BY a;
      ......
      SET query_dop = 1;

   .. note::

      -  If resources are enough, the higher the parallelism degree is, the better the performance improvement effect is.
      -  The SMP parallelism degree supports a session level setting and you are advised to enable the SMP before executing the query that meets the requirements. After the execution is complete, disable the SMP. Otherwise, SMP may affect services in peak hours.
      -  SMP adaptation (**query_dop** <= 0) depends on resource management. If resource management is disabled (use_workload_manager is **off**), plans with parallelism degree of only 1 or 2 are generated.
