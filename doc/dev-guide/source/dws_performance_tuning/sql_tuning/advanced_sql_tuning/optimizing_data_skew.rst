:original_name: dws_04_0451.html

.. _dws_04_0451:

.. _en-us_topic_0000002088734289:

Optimizing Data Skew
====================

Data skew breaks the balance among nodes in the distributed MPP architecture. If the amount of data stored or processed by a node is much greater than that by other nodes, the following problems may occur:

-  Storage skew severely limits the system capacity. The skew on a single node hinders system storage utilization.
-  Computing skew severely affects performance. The data to be processed on the skew node is much more than that on other nodes, deteriorating overall system performance.
-  Data skew severely affects the scalability of the MPP architecture. During storage or computing, data with the same values is often placed on the same node. Therefore, even if you add nodes after a data skew occurs, the skew data (data with the same values) is still placed on the node and affects the system capacity or performance bottleneck.

DWS provides a complete solution for data skew, including storage and computing skew.

Data Skew in the Storage Layer
------------------------------

In the DWS database, data is distributed and stored on each DN. You can improve the query efficiency by using distributed execution. However, if data skew occurs, bottlenecks exist on some DNs during distribution execution, affecting the query performance. This is because the distribution column is not properly selected. This can be solved by adjusting the distribution column.

Cause Analysis

#. Log in to the DWS console. On the **Clusters** page, locate the target cluster. In the **Operation** column of the target cluster, click **Monitoring Panel**. Choose **Monitoring** > **Node Monitoring**. Click the **Disks** tab to view the disk usage.

   .. note::

      Check the usage of each data disk. It is found that the usage is uneven among data disks. Generally, the difference between the highest and the lowest disk usage is small. If the difference exceeds 5%, data skew may occur.

#. Connect to the database and check the job operating status in the waiting view. It is found that the job waits for being processed by one or some DNs.

   ::

      SELECT wait_status, count(*) as cnt FROM pgxc_thread_wait_status WHERE wait_status not like '%cmd%' AND wait_status not like '%none%' and wait_status not like '%quit%' group by 1 order by 2 desc;

#. The **explain performance** of the slow statement shows that the scan time and number of scan rows in the base table of each DN are unbalanced.

   ::

      explain performance select avg(ss_wholesale_cost) from store_sales;

   |image1|

   -  Time of scanning a base table: The fastest DN takes 5 ms, and the slowest DN takes 1173 ms.
   -  Data distribution: Some DNs have 22,831,616 rows and other DNs have no row, resulting in data skew.

   |image2|

#. You can detect data skew by using the skew check interface.

   ::

      SELECT table_skewness('store_sales');

   |image3|

   ::

      SELECT table_distribution('public','store_sales');

   |image4|

**Solution - How to Find Skewed Tables:**

#. If the number of tables in the database is less than 10,000, use the skew view to query data skew status of all tables in the database.

   ::

      SELECT * FROM pgxc_get_table_skewness ORDER BY totalsize DESC;

#. If the number of tables in the database is greater than 10,000, it may take a long time (hours) to query the entire database and calculate skew columns in the **PGXC_GET_TABLE_SKEWNESS** view. You are advised to perform the following operations by referring to the definition of the **PGXC_GET_TABLE_SKEWNESS** view:

   -  In 8.1.2 and earlier cluster versions, the **table_distribution()** function is used to optimize calculation by customizing output and reducing output columns. For example:

      ::

         SELECT schemaname,tablename,max(dnsize) AS maxsize, min(dnsize) AS minsize
         FROM pg_catalog.pg_class c
         INNER JOIN pg_catalog.pg_namespace n ON n.oid = c.relnamespace
         INNER JOIN pg_catalog.table_distribution() s ON s.schemaname = n.nspname AND s.tablename = c.relname
         INNER JOIN pg_catalog.pgxc_class x ON c.oid = x.pcrelid AND x.pclocatortype = 'H'
         GROUP BY schemaname,tablename;

   -  For clusters of 8.1.3 and later versions, the **gs_table_distribution()** function can be used to check data skew of all tables in the database. The **gs_table_distribution()** function is better than the **table_distribution()** function when you query all tables in the database. In a large cluster with a large amount of data, use the **gs_table_distribution()** function.

      ::

         SELECT schemaname,tablename,max(dnsize) AS maxsize, min(dnsize) AS minsize
         FROM pg_catalog.pg_class c
         INNER JOIN pg_catalog.pg_namespace n ON n.oid = c.relnamespace
         INNER JOIN pg_catalog.gs_table_distribution() s ON s.schemaname = n.nspname AND s.tablename = c.relname
         INNER JOIN pg_catalog.pgxc_class x ON c.oid = x.pcrelid AND x.pclocatortype = 'H'
         GROUP BY schemaname,tablename;

      .. note::

         Run the following statement to query large tables:

         ::

            SELECT schemaname||'.'||tablename as table, sum(dnsize) as size FROM gs_table_distribution() group by 1 order by 2 desc limit 10;

         Run the following statement to query the table skew rate:

         ::

            WITH skew AS
            (
                    SELECT
                            schemaname,
                            tablename,
                            pg_catalog.sum(dnsize) AS totalsize,
                            pg_catalog.avg(dnsize) AS avgsize,
                            pg_catalog.max(dnsize) AS maxsize,
                            pg_catalog.min(dnsize) AS minsize,
                            (pg_catalog.max(dnsize) - pg_catalog.min(dnsize)) AS skewsize,
                            pg_catalog.stddev(dnsize) AS skewstddev
                    FROM pg_catalog.pg_class c
                    INNER JOIN pg_catalog.pg_namespace n ON n.oid = c.relnamespace
                    INNER JOIN pg_catalog.gs_table_distribution() s ON s.schemaname = n.nspname AND s.tablename = c.relname
                    INNER JOIN pg_catalog.pgxc_class x ON c.oid = x.pcrelid AND x.pclocatortype IN('H', 'N')
                    GROUP BY schemaname,tablename
            )
            SELECT
                    schemaname,
                    tablename,
                    totalsize,
                    avgsize::numeric(1000),
                    (maxsize/totalsize)::numeric(4,3)  AS maxratio,
                    (minsize/totalsize)::numeric(4,3)  AS minratio,
                    skewsize,
                    (skewsize/avgsize)::numeric(4,3)  AS skewratio,
                    skewstddev::numeric(1000)
            FROM skew
            WHERE totalsize > 0;

**Solution - How to Select a Distribution Key for a Table:**

#. If the **distinct** value of the column is large and no obvious data skew occurs, you can define multiple columns as a distribution key.

   View the size of **distinct**.

   ::

      SELECT count(distinct column1) FROM table;

   Check whether there are data skews:

   ::

      SELECT count(*) cnt, column1 FROM table group by column1 order by cnt limit 100;

#. Select the columns where **JOIN** or **GROUP BY** statement is frequently used to reduce the use of **STREAM**.

#. It is advised against using these distribution key selection methods:

   a. The default value of the distribution key (the first column) is used.
   b. The distribution key is generated through the auto-increment of sequences.
   c. The distribution key is generated using a random number. This method is recommended only when any column or any combination of two columns is skewed.

Data Skew in the Computing Layer
--------------------------------

Even if data is balanced across nodes after you change the distribution key of a table, data skew may still occur during a query. If data skew occurs in the result set of an operator on a DN, skew will also occur during the computing that involves the operator. Generally, this is caused by data redistribution during the execution.

During a query, JOIN keys and GROUP BY keys are not used as distribution columns. Data is redistributed among DNs based on the hash values of data on the keys. The redistribution is implemented using the Redistribute operator in an execution plan. Data skew in redistribution columns can lead to data skew during system operation. After the redistribution, some nodes will have much more data, process more data, and will have much lower performance than others.

In the following example, the **s** and **t** tables are joined, and **s.x** and **t.x** columns in the join condition are not their distribution keys. Table data is redistributed using the **REDISTRIBUTE** operator. Data skew occurs in the **s.x** column and not in the **t.x** column. The result set of the **Streaming** operator (**id** being **6**) on datanode2 has data three times that of other DNs and causes a skew.

::

   select * from skew s,test t where s.x = t.x order by s.a limit 1;

.. code-block::

    id |                      operation                      |        A-time
   ----+-----------------------------------------------------+-----------------------
     1 | ->  Limit                                           | 52622.382
     2 |    ->  Streaming (type: GATHER)                     | 52622.374
     3 |       ->  Limit                                     | [30138.494,52598.994]
     4 |          ->  Sort                                   | [30138.486,52598.986]
     5 |             ->  Hash Join (6,8)                     | [30127.013,41483.275]
     6 |                ->  Streaming(type: REDISTRIBUTE)    | [11365.110,22024.845]
     7 |                   ->  Seq Scan on public.skew s     | [2019.168,2175.369]
     8 |                ->  Hash                             | [2460.108,2499.850]
     9 |                   ->  Streaming(type: REDISTRIBUTE) | [1056.214,1121.887]
    10 |                      ->  Seq Scan on public.test t  | [310.848,325.569]

   6 --Streaming(type: REDISTRIBUTE)
            datanode1 (rows=5050368)
            datanode2 (rows=15276032)
            datanode3 (rows=5174272)
            datanode4 (rows=5219328)

Computing skew is more difficult to detect than storage skew. To solve computing skew, DWS provides the Runtime Load Balance Technology (RLBT) solution, controlled by the :ref:`skew_option <en-us_topic_0000001811609661__section1211182712176>` parameter. The RLBT solution addresses how to detect and solve data skew.

#. Detect data skew.

   The solution first checks whether skew data exists in redistribution columns used for computing. RLBT can detect data skew based on statistics, specified hints, or rules.

   -  Detection based on statistics

      Run the **ANALYZE** statement to collect statistics on tables. The optimizer will automatically identify skewed data on redistribution keys based on the statistics and generate optimization plans for queries having potential skew. When the redistribution key has multiple columns, statistics information can be used for identification only when all columns belong to the same base table.

      The statistics information can only provide the skew of the base table. If a column in the base table is skewed, or other columns have filtering conditions, or after the join of other tables, we cannot determine whether the skewed data still exists on the skewed column. If **skew_option** is **normal**, it indicates that the skew data still exists, and the base tables will be optimized to solve skew. If **skew_option** is **lazy**, it indicates that no more skew data exists and the optimization will stop.

   -  Detection based on specified hints

      The intermediate results of complex queries are difficult to estimate based on statistics. In this case, you can specify hints to provide the skew information based on which the optimizer optimizes queries. For details about the syntax of hints, see :ref:`Skew Hints <dws_04_0462>`.

   -  Detection based on rules

      In a business intelligence (BI) system, a large number of SQL statements having outer joins (including left joins, right joins, and full joins) are generated, and many NULL values will be generated in empty columns that have no match for outer joins. If JOIN or GROUP BY operations are performed on the columns, data skew will occur. RLBT can automatically identify this scenario and generate an optimization plan for NULL value skew.

#. Solve computing skew.

   **Join** and **Aggregate** operators are optimized to solve skew.

   -  **Join** optimization

   Skew and non-skew data is separately processed. Details are as follows:

   a. When redistribution is required on both sides of a join:

      Use **PART_REDISTRIBUTE_PART_ROUNDROBIN** on the side with skew. Specifically, perform round-robin on skew data and redistribution on non-skew data.

      Use **PART_REDISTRIBUTE_PART_BROADCAST** on the side with no skew. Specifically, perform broadcast on skew data and redistribution on non-skew data.

   b. When redistribution is required on only one side of a join:

      Use **PART_REDISTRIBUTE_PART_ROUNDROBIN** on the side where redistribution is required.

      Use **PART_LOCAL_PART_BROADCAST** on the side where redistribution is not required. Specifically, perform broadcast on skew data and retain other data locally.

   c. When a table has **NULL** values padded:

      Use **PART_REDISTRIBUTE_PART_LOCAL** on the table. Specifically, retain the **NULL** values locally and perform redistribution on other data.

   In the example query, the **s.x** column contains skewed data and its value is **0**. The optimizer identifies the skew data in statistics and generates the following optimization plan:

   .. code-block::

       id |                                operation                                |        A-time
      ----+-------------------------------------------------------------------------+-----------------------
        1 | ->  Limit                                                               | 23642.049
        2 |    ->  Streaming (type: GATHER)                                         | 23642.041
        3 |       ->  Limit                                                         | [23310.768,23618.021]
        4 |          ->  Sort                                                       | [23310.761,23618.012]
        5 |             ->  Hash Join (6,8)                                         | [20898.341,21115.272]
        6 |                ->  Streaming(type: PART REDISTRIBUTE PART ROUNDROBIN)   | [7125.834,7472.111]
        7 |                   ->  Seq Scan on public.skew s                         | [1837.079,1911.025]
        8 |                ->  Hash                                                 | [2612.484,2640.572]
        9 |                   ->  Streaming(type: PART REDISTRIBUTE PART BROADCAST) | [1193.548,1297.894]
       10 |                      ->  Seq Scan on public.test t                      | [314.343,328.707]

         5 --Vector Hash Join (6,8)
               Hash Cond: s.x = t.x
               Skew Join Optimized by Statistic
         6 --Streaming(type: PART REDISTRIBUTE PART ROUNDROBIN)
               datanode1 (rows=7635968)
               datanode2 (rows=7517184)
               datanode3 (rows=7748608)
               datanode4 (rows=7818240)

   In the preceding execution plan, **Skew Join Optimized by Statistic** indicates that this is an optimized plan used for handling data skew. The **Statistic** keyword indicates that the plan optimization is based on statistics; **Hint** indicates that the optimization is based on hints; **Rule** indicates that the optimization is based on rules. In this plan, skew and non-skew data is separately processed. Non-skew data in the **s** table is redistributed based on its hash values, and skew data (whose value is **0**) is evenly distributed on all nodes in round-robin mode. In this way, data skew is solved.

   To ensure result correctness, the **t** table also needs to be processed. In the **t** table, the data whose value is **0** (skew value in the **s.x** table) is broadcast and other data is redistributed based on its hash values.

   In this way, data skew in JOIN operations is solved. The above result shows that the output of the **Streaming** operator (**id** being **6**) is balanced and the end-to-end performance of the query is doubled.

   If the stream operator type in the execution plan is **HYBRID**, the stream mode varies depending on the skew data. The following plan is an example:

   .. code-block::

      EXPLAIN (nodes OFF, costs OFF) SELECT COUNT(*) FROM skew_scol s, skew_scol1 s1 WHERE s.b = s1.c;
      QUERY PLAN
      ------------------------------------------------------------------------------------------------------------------------------------------------------------------
      id |                                                                         operation
      ----+-----------------------------------------------------------------------------------------------------------------------------------------------------------
      1 | ->  Aggregate
      2 |    ->  Streaming (type: GATHER)
      3 |       ->  Aggregate
      4 |          ->  Hash Join (5,7)
      5 |             ->  Streaming(type: HYBRID)
      6 |                ->  Seq Scan on skew_scol s
      7 |             ->  Hash
      8 |                ->  Streaming(type: HYBRID)
      9 |                   ->  Seq Scan on skew_scol1 s1

      Predicate Information (identified by plan id)
      --------------------------------------------------------------------------------------------------------------------------------------------
      4 --Hash Join (5,7)
      Hash Cond: (s.b = s1.c)
      Skew Join Optimized by Statistic
      5 --Streaming(type: HYBRID)
      Skew Filter: (b = 1)
      Skew Filter: (b = 0)
      8 --Streaming(type: HYBRID)
      Skew Filter: (c = 0)
      Skew Filter: (c = 1)

   Data 1 has skew in the **skew_scol** table. Perform **ROUNDROBIN** on skew data and **REDISTRIBUTE** on non-skew data.

   Data 0 is the side with no skew in the **skew_scol** table. Perform **BROADCAST** on skew data and **REDISTRIBUTE** on non-skew data.

   As shown in the preceding figure, the two stream types are **PART REDISTRIBUTE PART ROUNDROBIN** and **PART REDISTRIBUTE PART BROADCAST**. In this example, the stream type is **HYBRID**.

   -  **Aggregate** optimization

   For aggregation, data on each DN is deduplicated based on the **GROUP BY** key and then redistributed. After the deduplication on DNs, the global occurrences of each value will not be greater than the number of DNs. Therefore, no serious data skew will occur. Take the following query as an example:

   ::

      select c1, c2, c3, c4, c5, c6, c7, c8, c9, count(*) from t group by c1, c2, c3, c4, c5, c6, c7, c8, c9 limit 10;

   The command output is as follows:

   .. code-block::

       id |                 operation                  |         A-time         |  A-rows
      ----+--------------------------------------------+------------------------+----------
        1 | ->  Streaming (type: GATHER)               | 130621.783             |       12
        2 |    ->  GroupAggregate                      | [85499.711,130432.341] |       12
        3 |       ->  Sort                             | [85499.509,103145.632] | 36679237
        4 |          ->  Streaming(type: REDISTRIBUTE) | [25668.897,85499.050]  | 36679237
        5 |             ->  Seq Scan on public.t       | [9835.069,10416.388]   | 36679237

         4 --Streaming(type: REDISTRIBUTE)
               datanode1 (rows=36678837)
               datanode2 (rows=100)
               datanode3 (rows=100)
               datanode4 (rows=200)

   A large amount of skew data exists. As a result, after data is redistributed based on its **GROUP BY** key, the data volume of datanode1 is hundreds of thousands of times that of others. After optimization, a GROUP BY operation is performed on the DN to deduplicate data. After redistribution, no data skew occurs.

   .. code-block::

       id |                 operation                  |        A-time
      ----+--------------------------------------------+-----------------------
        1 | ->  Streaming (type: GATHER)               | 10961.337
        2 |    ->  HashAggregate                       | [10953.014,10953.705]
        3 |       ->  HashAggregate                    | [10952.957,10953.632]
        4 |          ->  Streaming(type: REDISTRIBUTE) | [10952.859,10953.502]
        5 |             ->  HashAggregate              | [10084.280,10947.139]
        6 |                ->  Seq Scan on public.t    | [4757.031,5201.168]

       Predicate Information (identified by plan id)
      -----------------------------------------------
         3 --HashAggregate
               Skew Agg Optimized by Statistic

         4 --Streaming(type: REDISTRIBUTE)
               datanode1 (rows=17)
               datanode2 (rows=8)
               datanode3 (rows=8)
               datanode4 (rows=14)

   Applicable scope

   -  **Join** operator

      -  **nest loop**, **merge join**, and **hash join** can be optimized.
      -  If skew data is on the left to the join, **inner join**, **left join**, **semi join**, and **anti join** are supported. If skew data is on the right to the join, **inner join**, **right join**, **right semi join**, and **right anti join** are supported.
      -  For an optimization plan generated based on statistics, the optimizer checks whether it is optimal by estimating its cost. Optimization plans based on hints or rules are forcibly generated.

   -  **Aggregate** operator

      -  **array_agg**, **string_agg**, and **subplan in agg qual** cannot be optimized.
      -  A plan generated based on statistics is affected by its cost, the **plan_mode_seed** parameter, and the **best_agg_plan** parameter. A plan generated based on hints or rules are not affected by them.

.. |image1| image:: /_static/images/en-us_image_0000001163552957.png
.. |image2| image:: /_static/images/en-us_image_0000001119917890.png
.. |image3| image:: /_static/images/en-us_image_0000001163513637.png
.. |image4| image:: /_static/images/en-us_image_0000001163553799.png
