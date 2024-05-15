:original_name: dws_04_0485.html

.. _dws_04_0485:

Case: Adjusting the GUC Parameter best_agg_plan
===============================================

Symptom
-------

The t1 table is defined as follows:

::

   create table t1(a int, b int, c int) distribute by hash(a);

Assume that the distribution column of the result set provided by the agg lower-layer operator is setA, and the group by column of the agg operation is setB, the agg operations can be performed in two scenarios in the stream framework.

**Scenario 1: setA is a subset of setB.**

In this scenario, the aggregation result of the lower-layer result set is the correct result, which can be directly used by the upper-layer operator. For details, see the following figure:

::

   explain select a, count(1) from t1 group by a;
    id |          operation           | E-rows | E-width | E-costs
   ----+------------------------------+--------+---------+---------
     1 | ->  Streaming (type: GATHER) |     30 |       4 | 15.56
     2 |    ->  HashAggregate         |     30 |       4 | 14.31
     3 |       ->  Seq Scan on t1     |     30 |       4 | 14.14
   (3 rows)

**Scenario 2: setA is not a subset of setB.**

In this scenario, the Stream execution framework is classified into the following three plans:

hashagg+gather(redistribute)+hashagg

redistribute+hashagg(+gather)

hashagg+redistribute+hashagg(+gather)

GaussDB(DWS) provides the guc parameter **best_agg_plan** to intervene the execution plan, and forces the plan to generate the corresponding execution plan. This parameter can be set to **0**, **1**, **2**, and **3**.

-  When the value is set to **1**, the first plan is forcibly generated.
-  When the value is set to **2** and if the **group by** column can be redistributed, the second plan is forcibly generated. Otherwise, the first plan is generated.
-  When the value is set to **3** and if the **group by** column can be redistributed, the third plan is generated. Otherwise, the first plan is generated.
-  When the value is set to **0**, the query optimizer chooses the most optimal plan by the three preceding plans' evaluation cost.

Possible impacts are as follows:

::

   set best_agg_plan to 1;
   SET
   explain select b,count(1) from t1 group by b;
    id |            operation            | E-rows | E-width | E-costs
   ----+---------------------------------+--------+---------+---------
     1 | ->  HashAggregate               |      8 |       4 | 15.83
     2 |    ->  Streaming (type: GATHER) |     25 |       4 | 15.83
     3 |       ->  HashAggregate         |     25 |       4 | 14.33
     4 |          ->  Seq Scan on t1     |     30 |       4 | 14.14
   (4 rows)
   set best_agg_plan to 2;
   SET
   explain select b,count(1) from t1 group by b;
    id |                operation                | E-rows | E-width | E-costs
   ----+-----------------------------------------+--------+---------+---------
     1 | ->  Streaming (type: GATHER)            |     30 |       4 | 15.85
     2 |    ->  HashAggregate                    |     30 |       4 | 14.60
     3 |       ->  Streaming(type: REDISTRIBUTE) |     30 |       4 | 14.45
     4 |          ->  Seq Scan on t1             |     30 |       4 | 14.14
   (4 rows)
   set best_agg_plan to 3;
   SET
   explain select b,count(1) from t1 group by b;
    id |                operation                | E-rows | E-width | E-costs
   ----+-----------------------------------------+--------+---------+---------
     1 | ->  Streaming (type: GATHER)            |     30 |       4 | 15.84
     2 |    ->  HashAggregate                    |     30 |       4 | 14.59
     3 |       ->  Streaming(type: REDISTRIBUTE) |     25 |       4 | 14.59
     4 |          ->  HashAggregate              |     25 |       4 | 14.33
     5 |             ->  Seq Scan on t1          |     30 |       4 | 14.14
   (5 rows)

Summary
-------

Generally, the optimizer chooses an optimal execution plan, but the cost estimation, especially that of the intermediate result set, has large deviations, which may result in large deviations in agg calculation. In this case, you need to use best_agg_plan to adjust the agg calculation model.

When the aggregation convergence ratio is very small, that is, the number of result sets does not become small obviously after the agg operation (5 times is a critical point), you can select the redistribute+hashagg or hashagg+redistribute+hashagg execution mode.
