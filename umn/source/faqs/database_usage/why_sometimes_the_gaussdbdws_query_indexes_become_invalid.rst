:original_name: dws_03_2105.html

.. _dws_03_2105:

Why Sometimes the GaussDB(DWS) Query Indexes Become Invalid?
============================================================

Creating indexes for tables can improve database query performance. However, sometimes indexes cannot be used in a query plan. This section describes several common reasons and optimization methods.

Reason 1: The Returned Result Sets Are Large.
---------------------------------------------

The following uses Seq Scan and Index Scan on a row-store table as an example:

-  Seq Scan: searches table records in sequence. All records are retrieved during each scan. This is the simplest and most basic table scanning method, and its cost is high.
-  Index Scan: searches the index first, find the target location (pointer) in the index, and then retrieve data on the target page.

Index scan is faster than sequence scan in most cases. However, if the obtained result sets account for a large proportion (more than 70%) of all data, Index Scan needs to scan indexes before reading table data. This makes it slower table scan.

Reason 2: ANALYZE Is Not Performed In a Timely Manner.
------------------------------------------------------

**ANALYZE** is used to update table statistics. If **ANALYZE** is not executed on a table or a large amount of data is added to or deleted from a table after **ANALYZE** is executed, the statistics may be inaccurate, which may cause a query to skip the index.

Optimization method: Run the **ANALYZE** statement on the table to update statistics.

Reason 3: Filtering Conditions Contains Functions or Implicit Data Type Conversion
----------------------------------------------------------------------------------

If calculation, function, or implicit data type conversion is contained in filter criteria, indexes may fail to be selected.

For example, when a table is created, indexes are created in columns **a**, **b**, and **c**.

::

   CREATE TABLE test(a int, b text, c date);

-  Perform calculation on the indexed columns.

   The following command output indicates that both **where a = 101** and **where a = 102 - 1** use the index in column a, but **where a + 1 = 102** does not use the index.

   ::

      explain verbose select * from test where a  = 101;
                                                       QUERY PLAN
      ------------------------------------------------------------------------------------------------------------
        id |                   operation                    | E-rows | E-distinct | E-memory | E-width | E-costs
       ----+------------------------------------------------+--------+------------+----------+---------+---------
         1 | ->  Streaming (type: GATHER)                   |      1 |            |          |      44 | 16.27
         2 |    ->  Index Scan using index_a on public.test |      1 |            | 1MB      |      44 | 8.27

       Predicate Information (identified by plan id)
       ---------------------------------------------
         2 --Index Scan using index_a on public.test
               Index Cond: (test.a = 101)

       Targetlist Information (identified by plan id)
       ----------------------------------------------
         1 --Streaming (type: GATHER)
               Output: a, b, c
               Node/s: dn_6005_6006
         2 --Index Scan using index_a on public.test
               Output: a, b, c
               Distribute Key: a

         ====== Query Summary =====
       -------------------------------
       System available mem: 3358720KB
       Query Max mem: 3358720KB
       Query estimated mem: 1024KB
      (24 rows)

   ::

       explain verbose select * from test where a  = 102 - 1;
                                                       QUERY PLAN
      ------------------------------------------------------------------------------------------------------------
        id |                   operation                    | E-rows | E-distinct | E-memory | E-width | E-costs
       ----+------------------------------------------------+--------+------------+----------+---------+---------
         1 | ->  Streaming (type: GATHER)                   |      1 |            |          |      44 | 16.27
         2 |    ->  Index Scan using index_a on public.test |      1 |            | 1MB      |      44 | 8.27

       Predicate Information (identified by plan id)
       ---------------------------------------------
         2 --Index Scan using index_a on public.test
               Index Cond: (test.a = 101)

       Targetlist Information (identified by plan id)
       ----------------------------------------------
         1 --Streaming (type: GATHER)
               Output: a, b, c
               Node/s: dn_6005_6006
         2 --Index Scan using index_a on public.test
               Output: a, b, c
               Distribute Key: a

         ====== Query Summary =====
       -------------------------------
       System available mem: 3358720KB
       Query Max mem: 3358720KB
       Query estimated mem: 1024KB
      (24 rows)

   ::

      explain verbose select * from test where a + 1 = 102;
                                               QUERY PLAN
      --------------------------------------------------------------------------------------------
        id |           operation            | E-rows | E-distinct | E-memory | E-width | E-costs
       ----+--------------------------------+--------+------------+----------+---------+---------
         1 | ->  Streaming (type: GATHER)   |      1 |            |          |      44 | 22.21
         2 |    ->  Seq Scan on public.test |      1 |            | 1MB      |      44 | 14.21

       Predicate Information (identified by plan id)
       ---------------------------------------------
         2 --Seq Scan on public.test
               Filter: ((test.a + 1) = 102)

       Targetlist Information (identified by plan id)
       ----------------------------------------------
         1 --Streaming (type: GATHER)
               Output: a, b, c
               Node/s: All datanodes
         2 --Seq Scan on public.test
               Output: a, b, c
               Distribute Key: a

         ====== Query Summary =====
       -------------------------------
       System available mem: 3358720KB
       Query Max mem: 3358720KB
       Query estimated mem: 1024KB
      (24 rows)

   Optimization method: Use constants instead of expressions, or put constant calculation on the right of the equal sign (=).

-  Use functions on indexed columns.

   According to the following execution result, if a function is used on an indexed column, the index fails to be selected.

   ::

      explain verbose select * from test where to_char(c, 'yyyyMMdd') = to_char(CURRENT_DATE,'yyyyMMdd');
                                                                       QUERY PLAN
      --------------------------------------------------------------------------------------------------------------------------------------------
        id |           operation            | E-rows | E-distinct | E-memory | E-width | E-costs
       ----+--------------------------------+--------+------------+----------+---------+---------
         1 | ->  Streaming (type: GATHER)   |      1 |            |          |      44 | 22.28
         2 |    ->  Seq Scan on public.test |      1 |            | 1MB      |      44 | 14.28

                                                     Predicate Information (identified by plan id)
       ------------------------------------------------------------------------------------------------------------------------------------------
         2 --Seq Scan on public.test
               Filter: (to_char(test.c, 'yyyyMMdd'::text) = to_char(('2022-11-30'::pg_catalog.date)::timestamp with time zone, 'yyyyMMdd'::text))

       Targetlist Information (identified by plan id)
       ----------------------------------------------
         1 --Streaming (type: GATHER)
               Output: a, b, c
               Node/s: All datanodes
         2 --Seq Scan on public.test
               Output: a, b, c
               Distribute Key: a

         ====== Query Summary =====
       -------------------------------
       System available mem: 3358720KB
       Query Max mem: 3358720KB
       Query estimated mem: 1024KB
      (24 rows)

   ::

      explain verbose select * from test where c = current_date;
                                                       QUERY PLAN
      ------------------------------------------------------------------------------------------------------------
        id |                   operation                    | E-rows | E-distinct | E-memory | E-width | E-costs
       ----+------------------------------------------------+--------+------------+----------+---------+---------
         1 | ->  Streaming (type: GATHER)                   |      1 |            |          |      44 | 16.27
         2 |    ->  Index Scan using index_c on public.test |      1 |            | 1MB      |      44 | 8.27

              Predicate Information (identified by plan id)
       ------------------------------------------------------------
         2 --Index Scan using index_c on public.test
               Index Cond: (test.c = '2022-11-30'::pg_catalog.date)

       Targetlist Information (identified by plan id)
       ----------------------------------------------
         1 --Streaming (type: GATHER)
               Output: a, b, c
               Node/s: All datanodes
         2 --Index Scan using index_c on public.test
               Output: a, b, c
               Distribute Key: a

         ====== Query Summary =====
       -------------------------------
       System available mem: 3358720KB
       Query Max mem: 3358720KB
       Query estimated mem: 1024KB
      (24 rows)

   Optimization method: Do not use unnecessary functions on indexed columns.

-  Implicit conversion of data types.

   This scenario is common. For example, the type of column **b** is Text, and the filtering condition is **where b = 2**. During plan generation, the Text type is implicitly converted to the Bigint type, and the actual filtering condition changes to **where b::bigint = 2**. As a result, the index in column **b** becomes invalid.

   ::

      explain verbose select * from test where b = 2;
                                               QUERY PLAN
      --------------------------------------------------------------------------------------------
        id |           operation            | E-rows | E-distinct | E-memory | E-width | E-costs
       ----+--------------------------------+--------+------------+----------+---------+---------
         1 | ->  Streaming (type: GATHER)   |      1 |            |          |      44 | 22.21
         2 |    ->  Seq Scan on public.test |      1 |            | 1MB      |      44 | 14.21

       Predicate Information (identified by plan id)
       ---------------------------------------------
         2 --Seq Scan on public.test
               Filter: ((test.b)::bigint = 2)

       Targetlist Information (identified by plan id)
       ----------------------------------------------
         1 --Streaming (type: GATHER)
               Output: a, b, c
               Node/s: All datanodes
         2 --Seq Scan on public.test
               Output: a, b, c
               Distribute Key: a

         ====== Query Summary =====
       -------------------------------
       System available mem: 3358720KB
       Query Max mem: 3358720KB
       Query estimated mem: 1024KB
      (24 rows)

   ::

      explain verbose select * from test where b = '2';
                                                       QUERY PLAN
      ------------------------------------------------------------------------------------------------------------
        id |                   operation                    | E-rows | E-distinct | E-memory | E-width | E-costs
       ----+------------------------------------------------+--------+------------+----------+---------+---------
         1 | ->  Streaming (type: GATHER)                   |      1 |            |          |      44 | 16.27
         2 |    ->  Index Scan using index_b on public.test |      1 |            | 1MB      |      44 | 8.27

       Predicate Information (identified by plan id)
       ---------------------------------------------
         2 --Index Scan using index_b on public.test
               Index Cond: (test.b = '2'::text)

       Targetlist Information (identified by plan id)
       ----------------------------------------------
         1 --Streaming (type: GATHER)
               Output: a, b, c
               Node/s: All datanodes
         2 --Index Scan using index_b on public.test
               Output: a, b, c
               Distribute Key: a

         ====== Query Summary =====
       -------------------------------
       System available mem: 3358720KB
       Query Max mem: 3358720KB
       Query estimated mem: 1024KB
      (24 rows)

   Optimization method: Use constants of the same type as the indexed column to avoid implicit type conversion.

Scenario 4: Hashjoin Is Replaced with Nestloop + Indexscan.
-----------------------------------------------------------

When two tables are joined, the number of rows in the result set filtered by the WHERE condition in one table is small, thus the number of rows in the final result set is also small. In this case, the effect of nestloop+indexscan is better than that of hashjoin. The better execution plan is as follows:

You can see that the Index Cond: (t1.b = t2.b) at layer 5 has pushed the join condition down to the base table scanning.

::

   explain verbose select t1.a,t1.b from t1,t2 where t1.b=t2.b and t2.a=4;
    id |                    operation                     | E-rows | E-distinct | E-memory | E-width | E-costs
   ----+--------------------------------------------------+--------+------------+----------+---------+---------
     1 | ->  Streaming (type: GATHER)                     |     26 |            |          |       8 | 17.97
     2 |    ->  Nested Loop (3,5)                         |     26 |            | 1MB      |       8 | 11.97
     3 |       ->  Streaming(type: BROADCAST)             |      2 |            | 2MB      |       4 | 2.78
     4 |          ->  Seq Scan on public.t2               |      1 |            | 1MB      |       4 | 2.62
     5 |       ->  Index Scan using t1_b_idx on public.t1 |     26 |            | 1MB      |       8 | 9.05
   (5 rows)

    Predicate Information (identified by plan id)
   -----------------------------------------------
      4 --Seq Scan on public.t2
            Filter: (t2.a = 4)
      5 --Index Scan using t1_b_idx on public.t1
            Index Cond: (t1.b = t2.b)
   (4 rows)

    Targetlist Information (identified by plan id)
   ------------------------------------------------
      1 --Streaming (type: GATHER)
            Output: t1.a, t1.b
            Node/s: All datanodes
      2 --Nested Loop (3,5)
            Output: t1.a, t1.b
      3 --Streaming(type: BROADCAST)
            Output: t2.b
            Spawn on: datanode2
            Consumer Nodes: All datanodes
      4 --Seq Scan on public.t2
            Output: t2.b
            Distribute Key: t2.a
      5 --Index Scan using t1_b_idx on public.t1
            Output: t1.a, t1.b
            Distribute Key: t1.a
   (15 rows)

      ====== Query Summary =====
   ---------------------------------
    System available mem: 9262694KB
    Query Max mem: 9471590KB
    Query estimated mem: 5144KB
   (3 rows)

If the optimizer does not select such an execution plan, you can optimize it as follows:

::

   set enable_index_nestloop = on;
   set enable_hashjoin = off;
   set enable_seqscan = off;

Reason 5: The Scan Method Is Incorrectly Specified by Hints.
------------------------------------------------------------

GaussDB(DWS) plan hints can specify three scan method: tablescan, indexscan, and indexonlyscan.

-  Table Scan: full table scan, such as Seq Scan of row-store tables and CStore Scan of column-store tables.
-  Index Scan: scans indexes and then obtains table records based on the indexes.
-  Index-Only Scan: scans indexes, which cover all required results. Compared with the index scan, the index-only scan covers all queried columns. In this way, only indexes are retrieved, and data records do not need to be retrieved.

In Index-Only Scan scenarios, Index Scan specified by a hint will be invalid.

::

   explain verbose select*+ indexscan(test)*/ b from test where b = '1';
   WARNING:  unused hint: IndexScan(test)
                                                      QUERY PLAN
   -----------------------------------------------------------------------------------------------------------------
     id |                      operation                      | E-rows | E-distinct | E-memory | E-width | E-costs
    ----+-----------------------------------------------------+--------+------------+----------+---------+---------
      1 | ->  Streaming (type: GATHER)                        |      1 |            |          |      32 | 16.27
      2 |    ->  Index Only Scan using index_b on public.test |      1 |            | 1MB      |      32 | 8.27

      Predicate Information (identified by plan id)
    --------------------------------------------------
      2 --Index Only Scan using index_b on public.test
            Index Cond: (test.b = '1'::text)

      Targetlist Information (identified by plan id)
    --------------------------------------------------
      1 --Streaming (type: GATHER)
            Output: b
            Node/s: All datanodes
      2 --Index Only Scan using index_b on public.test
            Output: b
            Distribute Key: a

      ====== Query Summary =====
    -------------------------------
    System available mem: 3358720KB
    Query Max mem: 3358720KB
    Query estimated mem: 1024KB
   (24 rows)

::

   explain verbose select*+ indexonlyscan(test)*/ b from test where b = '1';
                                                      QUERY PLAN
   -----------------------------------------------------------------------------------------------------------------
     id |                      operation                      | E-rows | E-distinct | E-memory | E-width | E-costs
    ----+-----------------------------------------------------+--------+------------+----------+---------+---------
      1 | ->  Streaming (type: GATHER)                        |      1 |            |          |      32 | 16.27
      2 |    ->  Index Only Scan using index_b on public.test |      1 |            | 1MB      |      32 | 8.27

      Predicate Information (identified by plan id)
    --------------------------------------------------
      2 --Index Only Scan using index_b on public.test
            Index Cond: (test.b = '1'::text)

      Targetlist Information (identified by plan id)
    --------------------------------------------------
      1 --Streaming (type: GATHER)
            Output: b
            Node/s: All datanodes
      2 --Index Only Scan using index_b on public.test
            Output: b
            Distribute Key: a

      ====== Query Summary =====
    -------------------------------
    System available mem: 3358720KB
    Query Max mem: 3358720KB
    Query estimated mem: 1024KB
   (24 rows)

Optimization method: Correctly specify Index scan and Index-Only Scan.

Reason 6: Incorrect Use of GIN Index in Full-Text Retrieval
-----------------------------------------------------------

To accelerate text search, you can create a GIN index for full-text search.

::

   CREATE INDEX idxb ON test using gin(to_tsvector('english',b));

When creating the GIN index, you must use the 2-argument version of to_tsvector. Only when the query also uses the 2-argument version and the arguments are the same as that in the Gin index, the GIN index can be called.

.. note::

   The to_tsvector() function accepts one or two augments. If the one-augment version of the index is used, the system will use the configuration specified by **default_text_search_config** by default. To create an index, the two-augment version must be used, or the index content may be inconsistent.

::

   explain verbose select  * from test where to_tsvector(b) @@ to_tsquery('cat') order by 1;
                                             QUERY PLAN
   -----------------------------------------------------------------------------------------------
     id |             operation             | E-rows | E-distinct | E-memory | E-width | E-costs
    ----+-----------------------------------+--------+------------+----------+---------+---------
      1 | ->  Streaming (type: GATHER)      |      2 |            |          |      44 | 22.23
      2 |    ->  Sort                       |      2 |            | 16MB     |      44 | 14.23
      3 |       ->  Seq Scan on public.test |      1 |            | 1MB      |      44 | 14.21

           Predicate Information (identified by plan id)
    -----------------------------------------------------------
      3 --Seq Scan on public.test
            Filter: (to_tsvector(test.b) @@ '''cat'''::tsquery)

    Targetlist Information (identified by plan id)
    ----------------------------------------------
      1 --Streaming (type: GATHER)
            Output: a, b, c
            Merge Sort Key: test.a
            Node/s: All datanodes
      2 --Sort
            Output: a, b, c
            Sort Key: test.a
      3 --Seq Scan on public.test
            Output: a, b, c
            Distribute Key: a

      ====== Query Summary =====
    -------------------------------
    System available mem: 3358720KB
    Query Max mem: 3358720KB
    Query estimated mem: 1024KB
   (29 rows)

::

   explain verbose select  * from test where to_tsvector('english',b) @@ to_tsquery('cat') order by 1;
                                                 QUERY PLAN
   -------------------------------------------------------------------------------------------------------
     id |                 operation                 | E-rows | E-distinct | E-memory | E-width | E-costs
    ----+-------------------------------------------+--------+------------+----------+---------+---------
      1 | ->  Streaming (type: GATHER)              |      2 |            |          |      44 | 20.03
      2 |    ->  Sort                               |      2 |            | 16MB     |      44 | 12.03
      3 |       ->  Bitmap Heap Scan on public.test |      1 |            | 1MB      |      44 | 12.02
      4 |          ->  Bitmap Index Scan            |      1 |            | 1MB      |       0 | 8.00

                         Predicate Information (identified by plan id)
    ---------------------------------------------------------------------------------------
      3 --Bitmap Heap Scan on public.test
            Recheck Cond: (to_tsvector('english'::regconfig, test.b) @@ '''cat'''::tsquery)
      4 --Bitmap Index Scan
            Index Cond: (to_tsvector('english'::regconfig, test.b) @@ '''cat'''::tsquery)

    Targetlist Information (identified by plan id)
    ----------------------------------------------
      1 --Streaming (type: GATHER)
            Output: a, b, c
            Merge Sort Key: test.a
            Node/s: All datanodes
      2 --Sort
            Output: a, b, c
            Sort Key: test.a
      3 --Bitmap Heap Scan on public.test
            Output: a, b, c
            Distribute Key: a

      ====== Query Summary =====
    -------------------------------
    System available mem: 3358720KB
    Query Max mem: 3358720KB
    Query estimated mem: 2048KB
   (32 rows)

Optimization method: Use the 2-argument version of to_tsvector for the query and ensure that the argument values are the same as those in the index.
