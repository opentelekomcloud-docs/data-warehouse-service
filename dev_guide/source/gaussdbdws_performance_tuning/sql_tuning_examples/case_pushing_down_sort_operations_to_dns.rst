:original_name: dws_04_0478.html

.. _dws_04_0478:

Case: Pushing Down Sort Operations to DNs
=========================================

In an execution plan, more than 95% of the execution time is spent on **window agg** performed on the CN. In this case, **sum** is performed for the two columns separately, and then another **sum** is performed for the separate sum results of the two columns. After this, trunc and sorting are performed in sequence. You can try to rewrite the statement into a subquery to push down the sorting operations.

Before optimization
-------------------

The table structure is as follows:

::

   CREATE TABLE public.test(imsi int,L4_DW_THROUGHPUT int,L4_UL_THROUGHPUT int)
   with (orientation = column) DISTRIBUTE BY hash(imsi);

The query statements are as follows:

::

   SELECT COUNT(1) over() AS DATACNT,
   IMSI AS IMSI_IMSI,
   CAST(TRUNC(((SUM(L4_UL_THROUGHPUT) + SUM(L4_DW_THROUGHPUT))), 0) AS
   DECIMAL(20)) AS TOTAL_VOLOME_KPIID
   FROM public.test AS test
   GROUP BY IMSI
   ORDER BY TOTAL_VOLOME_KPIID DESC LIMIT 10;

The execution plan is as follows:

::

   QUERY PLAN
   --------------------------------------------------------------------------------------------------------------------------------------------------------------------------
     id |                    operation                     |      A-time      | A-rows  | E-rows  | E-distinct | Peak Memory  |   E-memory   | A-width | E-width | E-costs
    ----+--------------------------------------------------+------------------+---------+---------+------------+--------------+--------------+---------+---------+----------
      1 | ->  Row Adapter                                  | 2862.008         |      10 |      10 |            | 31KB         |              |         |      28 | 48360.42
      2 |    ->  Vector Limit                              | 2861.969         |      10 |      10 |            | 8KB          |              |         |      28 | 48360.42
      3 |       ->  Vector Sort                            | 2861.946         |      10 | 1000000 |            | 479KB        |              |         |      28 | 50860.39
      4 |          ->  Vector WindowAgg                    | 2166.759         | 1000000 | 1000000 |            | 69987KB      |              |         |      28 | 26750.75
      5 |             ->  Vector Streaming (type: GATHER)  | 136.813          | 1000000 | 1000000 |            | 208KB        |              |         |      28 | 15500.75
      6 |                ->  Vector Sonic Hash Aggregate   | [71.374, 73.640] | 1000000 | 1000000 |            | [14MB, 14MB] | 96MB(2919MB) | [31,31] |      28 | 15032.00
      7 |                   ->  CStore Scan on public.test | [2.957, 2.994]   | 1000000 | 1000000 |            | [1MB, 1MB]   | 1MB          |         |      12 | 1282.00

As we can see, both **window agg** and **sort** are performed on the CN, which is time consuming.

After optimization
------------------

Modify the statement to a subquery statement, as shown below:

::

   SELECT COUNT(1) over() AS DATACNT, IMSI_IMSI, TOTAL_VOLOME_KPIID
   FROM (SELECT IMSI AS IMSI_IMSI,
   CAST(TRUNC(((SUM(L4_UL_THROUGHPUT) + SUM(L4_DW_THROUGHPUT))),
   0) AS DECIMAL(20)) AS TOTAL_VOLOME_KPIID
   FROM public.test AS test
   GROUP BY IMSI
   ORDER BY TOTAL_VOLOME_KPIID DESC LIMIT 10);

Perform **sum** on the **trunc** results of the two columns, take it as a subquery, and then perform **window agg** for the subquery to push down the sorting operation to DNs, as shown below:

::

    QUERY PLAN
   ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
     id |                       operation                        |       A-time       | A-rows  | E-rows  | E-distinct |  Peak Memory   |   E-memory   | A-width | E-width | E-costs
    ----+--------------------------------------------------------+--------------------+---------+---------+------------+----------------+--------------+---------+---------+----------
      1 | ->  Row Adapter                                        | 955.277            |      10 |       5 |            | 31KB           |              |         |      24 | 25843.13
      2 |    ->  Vector WindowAgg                                | 955.261            |      10 |       5 |            | 1572KB         |              |         |      24 | 25843.13
      3 |       ->  Vector Streaming (type: GATHER)              | 955.015            |      10 |      10 |            | 127KB          |              |         |      24 | 25843.07
      4 |          ->  Vector Limit                              | [0.018, 0.018]     |      10 |      10 |            | [8KB, 8KB]     | 1MB          |         |      28 | 25836.97
      5 |             ->  Vector Streaming(type: BROADCAST)      | [0.014, 0.014]     |      20 |      20 |            | [719KB, 719KB] | 2MB          |         |      28 | 25837.12
      6 |                ->  Vector Limit                        | [927.730, 934.283] |      20 |      20 |            | [8KB, 8KB]     | 1MB          |         |      28 | 25836.85
      7 |                   ->  Vector Sort                      | [927.720, 934.269] |      20 | 1000000 |            | [463KB, 463KB] | 16MB         | [32,32] |      28 | 27086.82
      8 |                      ->  Vector Sonic Hash Aggregate   | [456.841, 461.077] | 1000000 | 1000000 |            | [15MB, 15MB]   | 96MB(2916MB) | [31,31] |      28 | 15032.00
      9 |                         ->  CStore Scan on public.test | [2.959, 3.014]     | 1000000 | 1000000 |            | [1MB, 1MB]     | 1MB          |         |      12 | 1282.00

The optimized SQL statement greatly improves the performance by reducing the execution time from 2.862s to 0.955s. Note that the optimization result in this example is for reference only. Due to the uncertainty of **WindowAgg**, the optimized result set is related to the actual service.
