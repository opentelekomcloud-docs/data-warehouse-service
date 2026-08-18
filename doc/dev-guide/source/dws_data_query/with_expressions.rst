:original_name: dws_04_1002.html

.. _dws_04_1002:

WITH Expressions
================

The WITH expression is used to define auxiliary statements used in large queries. These auxiliary statements are usually called common table expressions (CTE), which can be understood as a named subquery. The subquery can be referenced multiple times by its name in the query.

An auxiliary statement may use **SELECT**, **INSERT**, **UPDATE**, or **DELETE**. The **WITH** clause can be attached to a main statement, which can be a **SELECT**, **INSERT**, or **DELETE** statement.

SELECT in WITH
--------------

This section describes the usage of **SELECT** in a **WITH** clause.

**Syntax**

::

   [WITH [RECURSIVE] with_query [, ...] ] SELECT ...

The syntax of **with_query** is as follows:

::

   with_query_name [ ( column_name [, ...] ) ]
       AS [ [ NOT ] MATERIALIZED ] ( {select | values | insert | update | delete} )

.. caution::

   -  If you use **MATERIALIZED**, the subquery runs once and its result set is saved. If you use **NOT MATERIALIZED**, the subquery is replaced with its reference in the main query.

   -  The SQL statement specified by the AS statement of a CTE must be a statement that can return query results. It can be a common **SELECT** query statement or other data modification statements such as **INSERT**, **UPDATE**, **DELETE**, and **VALUES**. When using a data modification statement, you need to use the **RETURNING** clause to return tuples. Example:

      ::

         WITH s AS (INSERT INTO t VALUES(1) RETURNING a) SELECT * FROM s;

   -  A **WITH** expression indicates the CTE definition in a SQL statement block. Multiple CTEs can be defined at the same time. You can specify column names for each CTE or use the aliases of the columns in the query output. Example:

      ::

         WITH s1(a, b) AS (SELECT x, y FROM t1), s2 AS (SELECT x, y FROM t2) SELECT * FROM s1 JOIN s2 ON s1.a=s2.x;

      This statement defines two CTEs: **s1** and **s2**. **s1** specifies the column names **a** and **b**, and **s2** does not specify the column names. Therefore, the column names are the output column names **x** and **y**.

   -  Each CTE can be referenced zero, one, or more times in the main query.

   -  CTEs with the same name cannot exist in the same statement block. If CTEs with the same name exist in different statement blocks, the CTE in the nearest statement block is referenced.

   -  An SQL statement may contain multiple SQL statement blocks. Each statement block can contain a **WITH** expression. The CTE in each **WITH** expression can be referenced in the current statement block, subsequent CTEs of the current statement block, and sub-layer statement blocks, however, it cannot be referenced in the parent statement block. The definition of each CTE is also a statement block. Therefore, a WITH expression can also be defined in the statement block.

   -  When the CTE is materialized because MATERIALIZED is specified or the CTE is referenced for multiple times, the CTE is regarded as a table. The query or filter condition in the CTE cannot be independently elevated outside the CTE for other optimization. In the following example, the filter condition in the CTE cannot be deduced equivalently with the JOIN condition. As a result, the partitioned tables involved in JOIN cannot be optimized using partition pruning.

      ::

         CREATE TABLE date_part(ds date) distribute by hash(ds);

         CREATE TABLE t1(id1 int, ds date) distribute by hash(ds)
         partition by range(ds)
          ( partition p1 values less than('20250101'),
            partition p2 values less than('20250201'),
            partition p3 values less than('20250301'),
            partition p4 values less than('20250401'),
            partition p5 values less than('20250501'),
            partition p6 values less than('20250601'));

         CREATE TABLE t3(id3 int, ds date) distribute by hash(ds)
         partition by range(ds)
          ( partition p1 values less than('20250101'),
            partition p2 values less than('20250201'),
            partition p3 values less than('20250301'),
            partition p4 values less than('20250401'),
            partition p5 values less than('20250501'),
            partition p6 values less than('20250601'));

         explain verbose -- When the CTE is materialized, the query optimizer cannot elevate the filter conditions into the JOIN, thus hindering partition pruning on the partitioned tables involved in the JOIN.
          with dt as (select ds from date_part where ds>='2025-05-04' and ds < '2025-05-06'),
          t1 as(select id1 from t1 join dt t2 on t1.ds = t2.ds),
          t2 as (select id3 from t3 join dt t2 on t3.ds = t2.ds) select * from t1 join t2 on t1.id1 = 100;

         explain with dt as (select ds from date_part where ds>='2025-05-04' and ds < '2025-05-06'),
         t1 as(select id1 from t1 join dt t2 on t1.ds = t2.ds),
         t2 as(select id3 from t3 join dt t2 on t3.ds = t2.ds)
         select * from t1 join t2 on t1.id1 = 100;
                                                                                   QUERY PLAN
         ---------------------------------------------------------------------------------------------------------------------------------------------------------------
           id |                          operation                           | E-rows | E-memory | E-width | E-costs
          ----+--------------------------------------------------------------+--------+----------+---------+---------
            1 | ->  Row Adapter                                              |      2 |          |       8 | 66.27
            2 |    ->  Vector Streaming (type: GATHER)                       |      2 |          |       8 | 66.27
            3 |       ->  Vector Nest Loop (4,15)                            |      2 | 1MB      |       8 | 60.27
            4 |          ->  Vector CTE Append(5, 7)                         |      2 | 1MB      |      12 | 40.26
            5 |             ->  Vector Streaming(type: LOCAL GATHER)         |      1 | 2MB      |       8 | 20.01
            6 |                ->  CStore Scan on date_part  [5, CTE dt(0)]  |      1 | 1MB      |       8 | 20.01
            7 |             ->  Vector Nest Loop (8,9)                       |      2 | 1MB      |      12 | 20.25
            8 |                ->  Vector CTE Scan on dt(0) t2               |      1 | 1MB      |       8 | 0.02
            9 |                ->  Vector Materialize                        |      4 | 16MB     |       4 | 20.22
           10 |                   ->  Vector Streaming(type: BROADCAST)      |      4 | 2MB      |       4 | 20.21
           11 |                      ->  Vector Nest Loop (12,14)            |      2 | 1MB      |       4 | 20.04
           12 |                         ->  Vector Partition Iterator        |      1 | 1MB      |      12 | 20.00
           13 |                            ->  Partitioned CStore Scan on t1 |      1 | 1MB      |      12 | 20.00
           14 |                         ->  Vector CTE Scan on dt(0) t2      |      1 | 1MB      |       8 | 0.02
           15 |          ->  Vector Partition Iterator                       |      1 | 1MB      |      12 | 20.00
           16 |             ->  Partitioned CStore Scan on t3                |      1 | 1MB      |      12 | 20.00

                                                                  Predicate Information (identified by plan id)
          -------------------------------------------------------------------------------------------------------------------------------------------------------------
            3 --Vector Nest Loop (4,15)
                  Join Filter: (t2.ds = t3.ds)
            6 --CStore Scan on date_part
                  CU Predicate Filter: ((ds >= '2025-05-04 00:00:00'::timestamp without time zone) AND (ds < '2025-05-06 00:00:00'::timestamp without time zone))
                  Pushdown Predicate Filter: ((ds >= '2025-05-04 00:00:00'::timestamp without time zone) AND (ds < '2025-05-06 00:00:00'::timestamp without time zone))
           11 --Vector Nest Loop (12,14)
                  Join Filter: (public.t1.ds = t2.ds)
           12 --Vector Partition Iterator
                  Iterations: 6
           13 --Partitioned CStore Scan on t1
                  CU Predicate Filter: (id1 = 100)
                  Pushdown Predicate Filter: (id1 = 100)
                  Partitions Selected by Static Prune: 1..6
           15 --Vector Partition Iterator
                  Iterations: 6
           16 --Partitioned CStore Scan on t3
                  Partitions Selected by Static Prune: 1..6

            ====== Query Summary =====
          -------------------------------
          System available mem: 2990080KB
          Query Max mem: 2990080KB
          Query estimated mem: 11312KB
          Turbo Engine: true
         (45 rows)


         explain -- When the CTE is not materialized, the database expands it into a subquery in the query plan. In this expansion, the optimizer can integrate the CTE's filter conditions with the JOIN conditions from the outer query. This integration allows for partition pruning to be applied to partitioned tables involved in the JOIN.
         with dt as not materialized
         (select ds from date_part where ds>='2025-05-04' and ds < '2025-05-06'),
         t1 as(select id1 from t1 join dt t2 on t1.ds = t2.ds),
         t2 as(select id3 from t3 join dt t2 on t3.ds = t2.ds) select * from t1 join t2 on t1.id1 = 100;
                                                                                           QUERY PLAN
         -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
           id |                       operation                        | E-rows | E-memory | E-width | E-costs
          ----+--------------------------------------------------------+--------+----------+---------+---------
            1 | ->  Row Adapter                                        |      2 |          |       8 | 52.26
            2 |    ->  Vector Streaming (type: GATHER)                 |      2 |          |       8 | 52.26
            3 |       ->  Vector Nest Loop (4,6)                       |      2 | 1MB      |       8 | 46.26
            4 |          ->  Vector Streaming(type: BROADCAST)         |      2 | 2MB      |       8 | 20.10
            5 |             ->  CStore Scan on date_part               |      1 | 1MB      |       8 | 20.01
            6 |          ->  Vector Materialize                        |      2 | 16MB     |      16 | 26.15
            7 |             ->  Vector Nest Loop (8,11)                |      2 | 1MB      |      16 | 26.14
            8 |                ->  Vector Streaming(type: BROADCAST)   |      2 | 2MB      |      12 | 3.10
            9 |                   ->  Vector Partition Iterator        |      1 | 1MB      |      12 | 3.01
           10 |                      ->  Partitioned CStore Scan on t3 |      1 | 1MB      |      12 | 3.01
           11 |                ->  Vector Nest Loop (12,14)            |      2 | 1MB      |       4 | 23.03
           12 |                   ->  Vector Partition Iterator        |      1 | 1MB      |      12 | 3.01
           13 |                      ->  Partitioned CStore Scan on t1 |      1 | 1MB      |      12 | 3.01
           14 |                   ->  CStore Scan on date_part         |      1 | 1MB      |       8 | 20.01

                                                                          Predicate Information (identified by plan id)
          -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
            3 --Vector Nest Loop (4,6)
                  Join Filter: (t3.ds = public.date_part.ds)
            5 --CStore Scan on date_part
                  CU Predicate Filter: ((ds >= '2025-05-04 00:00:00'::timestamp without time zone) AND (ds < '2025-05-06 00:00:00'::timestamp without time zone))
                  Pushdown Predicate Filter: ((ds >= '2025-05-04 00:00:00'::timestamp without time zone) AND (ds < '2025-05-06 00:00:00'::timestamp without time zone))
            9 --Vector Partition Iterator
                  Iterations: 1
           10 --Partitioned CStore Scan on t3
                  CU Predicate Filter: ((ds >= '2025-05-04 00:00:00'::timestamp without time zone) AND (ds < '2025-05-06 00:00:00'::timestamp without time zone))
                  Pushdown Predicate Filter: ((ds >= '2025-05-04 00:00:00'::timestamp without time zone) AND (ds < '2025-05-06 00:00:00'::timestamp without time zone))
                  Partitions Selected by Static Prune: 6
           11 --Vector Nest Loop (12,14)
                  Join Filter: (public.t1.ds = public.date_part.ds)
           12 --Vector Partition Iterator
                  Iterations: 1
           13 --Partitioned CStore Scan on t1
                  CU Predicate Filter: ((id1 = 100) AND (ds >= '2025-05-04 00:00:00'::timestamp without time zone) AND (ds < '2025-05-06 00:00:00'::timestamp without time zone))
                  Pushdown Predicate Filter: ((id1 = 100) AND (ds >= '2025-05-04 00:00:00'::timestamp without time zone) AND (ds < '2025-05-06 00:00:00'::timestamp without time zone))
                  Partitions Selected by Static Prune: 6
           14 --CStore Scan on date_part
                  CU Predicate Filter: ((ds >= '2025-05-04 00:00:00'::timestamp without time zone) AND (ds < '2025-05-06 00:00:00'::timestamp without time zone))
                  Pushdown Predicate Filter: ((ds >= '2025-05-04 00:00:00'::timestamp without time zone) AND (ds < '2025-05-06 00:00:00'::timestamp without time zone))

            ====== Query Summary =====
          -------------------------------
          System available mem: 2990080KB
          Query Max mem: 2990080KB
          Query estimated mem: 12352KB
          Turbo Engine: true
         (48 rows)

The purpose of SELECT in WITH is to break down complex queries into simple parts. Example:

::

       WITH regional_sales AS (
            SELECT region, SUM(amount) AS total_sales
            FROM orders
            GROUP BY region
        ), top_regions AS (
            SELECT region
            FROM regional_sales
            WHERE total_sales > (SELECT SUM(total_sales)/10 FROM regional_sales)
        )
        SELECT region,
               product,
               SUM(quantity) AS product_units,
               SUM(amount) AS product_sales
        FROM orders
        WHERE region IN (SELECT region FROM top_regions)
        GROUP BY region, product;

The **WITH** clause defines two auxiliary statements: **regional_sales** and **top_regions**. The output of **regional_sales** is used in **top_regions**, and the output of **top_regions** is used in the main **SELECT** query. This example can be written without **WITH**. In that case, it must be written with a two-layer nested sub-SELECT statement, making the query longer and difficult to maintain.

Recursive WITH Query
--------------------

By declaring the keyword **RECURSIVE**, a WITH query can reference its own output.

The common form of a recursive WITH query is as follows:

::

   non_recursive_term UNION [ALL] recursive_term

**UNION** performs deduplication when combining sets. **UNION ALL** directly combines result sets without deduplication. Only recursive items can contain references to the query output.

When using recursive WITH, ensure that the recursive item of the query does not return a tuple. Otherwise, the query will loop infinitely.

The table **tree** is used to store information about all nodes in the following figure.

|image1|

The table definition statement is as follows:

::

   CREATE TABLE tree(id INT, parentid INT);

The data in the table is as follows:

::

   INSERT INTO tree VALUES(1,0),(2,1),(3,1),(4,2),(5,2),(6,3),(7,3),(8,4),(9,4),(10,6),(11,6),(12,10);

   SELECT * FROM tree;
    id | parentid
   ----+----------
     1 |        0
     2 |        1
     3 |        1
     4 |        2
     5 |        2
     6 |        3
     7 |        3
     8 |        4
     9 |        4
    10 |        6
    11 |        6
    12 |       10
   (12 rows)

You can run the following **WITH RECURSIVE** statement to return the nodes and hierarchy information of the entire tree starting from node 1 at the top layer:

::

   WITH RECURSIVE nodeset AS
   (
   -- recursive initializing query
   SELECT id, parentid, 1 AS level FROM tree
   WHERE id = 1
   UNION ALL
   -- recursive join query
   SELECT tree.id, tree.parentid, level + 1 FROM tree, nodeset
   WHERE tree.parentid = nodeset.id
   )
   SELECT * FROM nodeset ORDER BY id;

In the preceding query, a typical **WITH RECURSIVE** expression contains the CTE of at least one recursive query. The CTE is defined as a **UNION ALL** set operation. The first branch is the recursive start query, and the second branch is the recursive join query, the first part is referenced for continuous recursive join. When this statement is executed, the recursive start query is executed once, and the join query is executed several times. The results are added to the start query result set until the results of some join queries are empty.

The command output is as follows:

::

    id | parentid | level
   ----+----------+-------
     1 |        0 |     1
     2 |        1 |     2
     3 |        1 |     2
     4 |        2 |     3
     5 |        2 |     3
     6 |        3 |     3
     7 |        3 |     3
     8 |        4 |     4
     9 |        4 |     4
    10 |        6 |     4
    11 |        6 |     4
    12 |       10 |     5
   (12 rows)

According to the returned result, the start query result contains the result set whose level is 1. The join query is executed for five times. The result sets whose levels are 2, 3, 4, and 5 are output for the first four times. During the fifth execution, there is no record whose parentid is the same as the output result set ID, that is, there is no redundant child node. Therefore, the query ends.

.. note::

   DWS supports distributed execution of **WITH RECURSIVE** expressions. **WITH RECURSIVE** involves cyclic data. DWS introduces the **max_recursive_times** parameter to control the maximum number of cycles in WITH RECURSIVE. The default value is **200**. If the number of cycles exceeds **200**, an error is reported.

Data Modification Statements in WITH
------------------------------------

Use the **INSERT**, **UPDATE**, and **DELETE** commands in the WITH clause. This allows the user to perform multiple different operations in the same query. The following is an example:

::

   WITH moved_tree AS (
        DELETE FROM tree
        WHERE parentid = 4
        RETURNING * )
    INSERT INTO tree_log
    SELECT * FROM moved_tree;

The preceding query example actually moves rows from **tree** to **tree_log**. The **DELETE** command in the **WITH** clause deletes the specified rows from **tree**, returns their contents through the **RETURNING** clause, and then the main query reads the output and inserts it into **tree_log**.

To retrieve the modified content instead of the target table, the data modification statement in the **WITH** clause should include the **RETURNING** clause. This clause creates a temporary table that can be accessed by the rest of the query. If a data modification statement in the **WITH** statement lacks a **RETURNING** clause, it cannot form a temporary table and cannot be referenced in the remaining queries.

If the **RECURSIVE** keyword is specified, recursive self-reference is not allowed in data modification statements. In some cases, you can bypass this restriction by referencing the output of recursive the **WITH** statement. For example:

::

   WITH RECURSIVE included_parts(sub_part, part) AS (
        SELECT sub_part, part FROM parts WHERE part = 'our_product'
      UNION ALL
        SELECT p.sub_part, p.part
        FROM included_parts pr, parts p
        WHERE p.part = pr.sub_part
      )
   DELETE FROM parts
      WHERE part IN (SELECT part FROM included_parts);

This query will remove all direct or indirect subparts of a product.

The substatements in the **WITH** clause are executed at the same time as the main query. Therefore, when using the data modification statement in a WITH statement, the actual update order is in an unpredictable manner. All statements are executed in the same snapshot, and the effect of the statements is invisible on the target table. This mitigates the unpredictability of the actual order of row updates and means that **RETURNING** data is the only way to convey changes between different **WITH** substatements and the main query.

In this example, the outer layer **SELECT** can return the data before the update.

::

   WITH t AS (
        UPDATE tree SET id = id + 1
        RETURNING * )
   SELECT * FROM tree;

In this example, the external SELECT returns the updated data.

::

   WITH t AS (
   UPDATE tree SET id = id + 1
        RETURNING * )
   SELECT * FROM t;

The same row cannot be updated twice in a single statement. Otherwise, the update effect will be unpredictable. If only one update takes effect, it is difficult (and sometimes impossible) to predict which one takes effect.

.. |image1| image:: /_static/images/en-us_image_0000001587804470.png
