:original_name: dws_04_0488.html

.. _dws_04_0488:

Case: Rewriting SQL Statements and Eliminating Prune Interference
=================================================================

A filter criterion that contains the expression of partition key cannot be used for pruning. As a result, the query statement scans almost all data in the partitioned table.

Before Optimization
-------------------

**t_ddw_f10_op_cust_asset_mon** indicates the partitioned table. **year_mth** indicates the partition key. This field is an integer consisting of the **year** and **mth** values.

The following figure shows the tested SQL statements.

::

   SELECT
       count(1)
   FROM t_ddw_f10_op_cust_asset_mon b1
   WHERE b1.year_mth  < substr('20200722',1 ,6 )
   AND b1.year_mth + 1 >= substr('20200722',1 ,6 );

The test result shows that the table scan of the SQL statement takes 10 seconds. The execution plan of the SQL statement is as follows.

::

   EXPLAIN (ANALYZE ON, VERBOSE ON)
   SELECT
       count(1)
   FROM t_ddw_f10_op_cust_asset_mon b1
   WHERE b1.year_mth < substr('20200722',1 ,6 )
   AND b1.year_mth + 1 >= cast(substr('20200722',1 ,6 ) AS int);
                                                                                                   QUERY PLAN
   -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
     id |                                   operation                                   |        A-time         |  A-rows  |  E-rows  | E-distinct | Peak Memory  | E-memory | A-width | E-width |  E-costs
    ----+-------------------------------------------------------------------------------+-----------------------+----------+----------+------------+--------------+----------+---------+---------+-----------
      1 | ->  Aggregate                                                                 | 10662.260             |        1 |        1 |            | 32KB         |          |         |       8 | 593656.42
      2 |    ->  Streaming (type: GATHER)                                               | 10662.172             |        4 |        4 |            | 136KB        |          |         |       8 | 593656.42
      3 |       ->  Aggregate                                                           | [9692.785, 10656.068] |        4 |        4 |            | [24KB, 24KB] | 1MB      |         |       8 | 593646.42
      4 |          ->  Partition Iterator                                               | [8787.198, 9629.138]  | 16384000 | 32752850 |            | [16KB, 16KB] | 1MB      |         |       0 | 573175.88
      5 |             ->  Partitioned Seq Scan on public.t_ddw_f10_op_cust_asset_mon b1 | [8365.655, 9152.115]  | 16384000 | 32752850 |            | [32KB, 32KB] | 1MB      |         |       0 | 573175.88

                                    SQL Diagnostic Information
    -------------------------------------------------------------------------------------------
    Partitioned table unprunable Qual
            table public.t_ddw_f10_op_cust_asset_mon b1:
            left side of expression "((year_mth + 1) > 202008)" invokes function-call/type-conversion

                      Predicate Information (identified by plan id)
    ----------------------------------------------------------------------------------
      4 --Partition Iterator
            Iterations: 6
      5 --Partitioned Seq Scan on public.t_ddw_f10_op_cust_asset_mon b1
            Filter: ((b1.year_mth < 202007::bigint) AND ((b1.year_mth + 1) >= 202007))
            Rows Removed by Filter: 81920000
            Partitions Selected by Static Prune: 1..6

After Optimization
------------------

After analyzing the execution plan of the statement and checking the SQL self-diagnosis information in the execution plan, the following diagnosis information is found:

.. code-block::

                                    SQL Diagnostic Information
    ------------------------------------------------------------------------------------------
    Partitioned table unprunable Qual
            table public.t_ddw_f10_op_cust_asset_mon b1:
            left side of expression "((year_mth + 1) > 202008)" invokes function-call/type-conversion

The filter criterion contains the expression **(year_mth + 1) > 202008**. A filter criterion that contains the expression of partition key cannot be used for pruning. As a result, the query statement scans almost all data in the partitioned table.

Compared with the original SQL statement, the expression **(year_mth + 1) > 202008** is derived from the expression **b1.year_mth + 1 > substr('20200822',1 ,6 )**. Based on the diagnosis information, the SQL statement is modified as follows.

::

   SELECT
       count(1)
   FROM t_ddw_f10_op_cust_asset_mon b1
   WHERE b1.year_mth <= substr('20200822',1 ,6 )
   AND b1.year_mth > cast(substr('20200822',1 ,6 ) AS int) - 1;

After the modification, the SQL statement execution information is as follows. The alarm indicating that the pruning is not performed is cleared. After the pruning, the score of the partition to be scanned is 1, and the execution time is shortened from 10 seconds to 3 seconds.

::

   EXPLAIN (analyze ON, verbose ON)
   SELECT
       count(1)
   FROM t_ddw_f10_op_cust_asset_mon b1
   WHERE b1.year_mth < substr('20200722',1 ,6 )
   AND b1.year_mth >= cast(substr('20200722',1 ,6 ) AS int) - 1;
                                                                                                   QUERY PLAN
   ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
     id |                                   operation                                   |        A-time        |  A-rows  |  E-rows  | E-distinct | Peak Memory  | E-memory | A-width | E-width |  E-costs
    ----+-------------------------------------------------------------------------------+----------------------+----------+----------+------------+--------------+----------+---------+---------+-----------
      1 | ->  Aggregate                                                                 | 3009.796             |        1 |        1 |            | 32KB         |          |         |       8 | 501541.70
      2 |    ->  Streaming (type: GATHER)                                               | 3009.718             |        4 |        4 |            | 136KB        |          |         |       8 | 501541.70
      3 |       ->  Aggregate                                                           | [2675.509, 3003.298] |        4 |        4 |            | [24KB, 24KB] | 1MB      |         |       8 | 501531.70
      4 |          ->  Partition Iterator                                               | [1820.725, 2053.836] | 16384000 | 16380697 |            | [16KB, 16KB] | 1MB      |         |       0 | 491293.75
      5 |             ->  Partitioned Seq Scan on public.t_ddw_f10_op_cust_asset_mon b1 | [1420.972, 1590.083] | 16384000 | 16380697 |            | [16KB, 16KB] | 1MB      |         |       0 | 491293.75

                   Predicate Information (identified by plan id)
    ----------------------------------------------------------------------------
      4 --Partition Iterator
            Iterations: 1
      5 --Partitioned Seq Scan on public.t_ddw_f10_op_cust_asset_mon b1
            Filter: ((b1.year_mth < 202007::bigint) AND (b1.year_mth >= 202006))
            Partitions Selected by Static Prune: 6
