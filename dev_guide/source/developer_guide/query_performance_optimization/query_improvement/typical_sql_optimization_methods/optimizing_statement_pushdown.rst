:original_name: dws_04_0447.html

.. _dws_04_0447:

Optimizing Statement Pushdown
=============================

Statement Pushdown
------------------

Currently, the GaussDB(DWS) optimizer can use three methods to develop statement execution policies in the distributed framework: generating a statement pushdown plan, a distributed execution plan, or a distributed execution plan for sending statements.

-  A statement pushdown plan pushes query statements from a CN down to DNs for execution and returns the execution results to the CN.
-  In a distributed execution plan, a CN compiles and optimizes query statements, generates a plan tree, and then sends the plan tree to DNs for execution. After the statements have been executed, execution results will be returned to the CN.
-  A distributed execution plan for sending statements pushes queries that can be pushed down (mostly base table scanning statements) to DNs for execution. Then, the plan obtains the intermediate results and sends them to the CN, on which the remaining queries are to be executed.

The third policy sends many intermediate results from the DNs to a CN for further execution. In this case, the CN performance bottleneck (in bandwidth, storage, and computing) is caused by statements that cannot be pushed down to DNs. Therefore, you are not advised to use the query statements that only the third policy is applicable to.

Statements cannot be pushed down to DNs if they have :ref:`Functions That Do Not Support Pushdown <en-us_topic_0000001145894663__s926a7f64d03546399fe529744b9bf420>` or :ref:`Syntax That Does Not Support Pushdown <en-us_topic_0000001145894663__sf9bb07c596384fbc833219a5e72da7e4>`. Generally, you can rewrite the execution statements to solve the problem.

Viewing Whether the Execution Plan Has Been Pushed Down to DNs
--------------------------------------------------------------

Perform the following procedure to quickly determine whether the execution plan can be pushed down to DNs:

#. Set the GUC parameter :ref:`enable_fast_query_shipping <en-us_topic_0000001145894431__s9b7f64f4f112450490c8c74b520cc915>` to **off** to use the distributed framework policy for the query optimizer.

   ::

      SET enable_fast_query_shipping = off;

#. View the execution plan.

   If the execution plan contains Data Node Scan, the SQL statements cannot be pushed down to DNs. If the execution plan contains Streaming, the SQL statements can be pushed down to DNs.

   For example:

   ::

      select
      count(ss.ss_sold_date_sk order by ss.ss_sold_date_sk)c1
      from store_sales ss, store_returns sr
      where
      sr.sr_customer_sk = ss.ss_customer_sk;

   The execution plan is as follows, which indicates that the SQL statement cannot be pushed down.

   .. code-block::

                                    QUERY PLAN
      --------------------------------------------------------------------------
      Aggregate
      ->  Hash Join
      Hash Cond: (ss.ss_customer_sk = sr.sr_customer_sk)
      ->  Data Node Scan on store_sales "_REMOTE_TABLE_QUERY_"
      Node/s: All datanodes
      ->  Hash
      ->  Data Node Scan on store_returns "_REMOTE_TABLE_QUERY_"
      Node/s: All datanodes
      (8 rows)

.. _en-us_topic_0000001145894663__sf9bb07c596384fbc833219a5e72da7e4:

Syntax That Does Not Support Pushdown
-------------------------------------

SQL syntax that does not support pushdown is described using the following table definition examples:

::

   CREATE TABLE CUSTOMER1
   (
       C_CUSTKEY     BIGINT NOT NULL
     , C_NAME        VARCHAR(25) NOT NULL
     , C_ADDRESS     VARCHAR(40) NOT NULL
     , C_NATIONKEY   INT NOT NULL
     , C_PHONE       CHAR(15) NOT NULL
     , C_ACCTBAL     DECIMAL(15,2)   NOT NULL
     , C_MKTSEGMENT  CHAR(10) NOT NULL
     , C_COMMENT     VARCHAR(117) NOT NULL
   )
   DISTRIBUTE BY hash(C_CUSTKEY);
   CREATE TABLE test_stream(a int, b float);--float does not support redistribution.
   CREATE TABLE sal_emp ( c1 integer[] ) DISTRIBUTE BY replication;

-  The **returning** statement cannot be pushed down.

   ::

      explain update customer1 set C_NAME = 'a' returning c_name;
                                     QUERY PLAN
      ------------------------------------------------------------------
       Update on customer1  (cost=0.00..0.00 rows=30 width=187)
         Node/s: All datanodes
         Node expr: c_custkey
         ->  Data Node Scan on customer1 "_REMOTE_TABLE_QUERY_"  (cost=0.00..0.00 rows=30 width=187)
               Node/s: All datanodes
      (5 rows)

-  If columns in **count(distinct expr)** do not support redistribution, they do not support pushdown.

   ::

      explain verbose select count(distinct b) from test_stream;
                                                QUERY PLAN
      ------------------------------------------------------------------ Aggregate  (cost=2.50..2.51 rows=1 width=8)
         Output: count(DISTINCT test_stream.b)
         ->  Data Node Scan on test_stream "_REMOTE_TABLE_QUERY_"  (cost=0.00..0.00 rows=30 width=8)
               Output: test_stream.b
               Node/s: All datanodes
               Remote query: SELECT b FROM ONLY public.test_stream WHERE true
      (6 rows)

-  Statements using **distinct on** cannot be pushed down.

   ::

      explain verbose select distinct on (c_custkey) c_custkey from customer1 order by c_custkey;
                                                  QUERY PLAN
      ------------------------------------------------------------------ Unique  (cost=49.83..54.83 rows=30 width=8)
         Output: customer1.c_custkey
         ->  Sort  (cost=49.83..52.33 rows=30 width=8)
               Output: customer1.c_custkey
               Sort Key: customer1.c_custkey
               ->  Data Node Scan on customer1 "_REMOTE_TABLE_QUERY_"  (cost=0.00..0.00 rows=30 width=8)
                     Output: customer1.c_custkey
                     Node/s: All datanodes
                     Remote query: SELECT c_custkey FROM ONLY public.customer1 WHERE true
      (9 rows)

-  In a statement using **FULL JOIN**, if the column specified using **JOIN** does not support redistribution, the statement does not support pushdown.

   ::

      explain select * from test_stream t1 full join test_stream t2 on t1.a=t2.b;
                                                    QUERY PLAN
      ------------------------------------------------------------------ Hash Full Join  (cost=0.38..0.82 rows=30 width=24)
         Hash Cond: ((t1.a)::double precision = t2.b)
         ->  Data Node Scan on test_stream "_REMOTE_TABLE_QUERY_"  (cost=0.00..0.00 rows=30 width=12)
               Node/s: All datanodes
         ->  Hash  (cost=0.00..0.00 rows=30 width=12)
               ->  Data Node Scan on test_stream "_REMOTE_TABLE_QUERY_"  (cost=0.00..0.00 rows=30 width=12)
                     Node/s: All datanodes
      (7 rows)

-  Does not support array expression pushdown.

   ::

      explain verbose select array[c_custkey,1] from customer1 order by c_custkey;

                                QUERY PLAN
      ------------------------------------------------------------------ Sort  (cost=49.83..52.33 rows=30 width=8)
         Output: (ARRAY[customer1.c_custkey, 1::bigint]), customer1.c_custkey
         Sort Key: customer1.c_custkey
         ->  Data Node Scan on "__REMOTE_SORT_QUERY__"  (cost=0.00..0.00 rows=30 width=8)
               Output: (ARRAY[customer1.c_custkey, 1::bigint]), customer1.c_custkey
               Node/s: All datanodes
               Remote query: SELECT ARRAY[c_custkey, 1::bigint], c_custkey FROM ONLY public.customer1 WHERE true ORDER BY 2
      (7 rows)

-  The following table describes the scenarios where a statement containing **WITH RECURSIVE** cannot be pushed down in the current version, as well as the causes.

   +-----------------------+-------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | No.                   | Scenario                                                                            | Cause of Not Supporting Pushdown                                                                                                                                               |
   +=======================+=====================================================================================+================================================================================================================================================================================+
   | 1                     | The query contains foreign tables or HDFS tables.                                   | LOG: SQL can't be shipped, reason: RecursiveUnion contains HDFS Table or ForeignScan is not shippable (In this table, **LOG** describes the cause of not supporting pushdown.) |
   |                       |                                                                                     |                                                                                                                                                                                |
   |                       |                                                                                     | In the current version, queries containing foreign tables or HDFS tables do not support pushdown.                                                                              |
   +-----------------------+-------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | 2                     | Multiple Node Groups                                                                | LOG: SQL can't be shipped, reason: With-Recursive under multi-nodegroup scenario is not shippable                                                                              |
   |                       |                                                                                     |                                                                                                                                                                                |
   |                       |                                                                                     | In the current version, pushdown is supported only when all base tables are stored and computed in the same Node Group.                                                        |
   +-----------------------+-------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | 3                     | .. code-block::                                                                     | LOG: SQL can't be shipped, reason: With-Recursive does not contain "ALL" to bind recursive & none-recursive branches                                                           |
   |                       |                                                                                     |                                                                                                                                                                                |
   |                       |    WITH recursive t_result AS (                                                     | **ALL** is not used for **UNION**. In this case, the return result is deduplicated.                                                                                            |
   |                       |    SELECT dm,sj_dm,name,1 as level                                                  |                                                                                                                                                                                |
   |                       |    FROM test_rec_part                                                               |                                                                                                                                                                                |
   |                       |    WHERE sj_dm > 10                                                                 |                                                                                                                                                                                |
   |                       |    UNION                                                                            |                                                                                                                                                                                |
   |                       |    SELECT t2.dm,t2.sj_dm,t2.name||' > '||t1.name,t1.level+1                         |                                                                                                                                                                                |
   |                       |    FROM t_result t1                                                                 |                                                                                                                                                                                |
   |                       |    JOIN test_rec_part t2 ON t2.sj_dm = t1.dm                                        |                                                                                                                                                                                |
   |                       |    )                                                                                |                                                                                                                                                                                |
   |                       |    SELECT * FROM t_result t;                                                        |                                                                                                                                                                                |
   +-----------------------+-------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | 4                     | .. code-block::                                                                     | LOG: SQL can't be shipped, reason: With-Recursive contains system table is not shippable                                                                                       |
   |                       |                                                                                     |                                                                                                                                                                                |
   |                       |    WITH RECURSIVE x(id) AS                                                          | A base table contains the system catalog.                                                                                                                                      |
   |                       |    (                                                                                |                                                                                                                                                                                |
   |                       |    select count(1) from pg_class where oid=1247                                     |                                                                                                                                                                                |
   |                       |    UNION ALL                                                                        |                                                                                                                                                                                |
   |                       |    SELECT id+1 FROM x WHERE id < 5                                                  |                                                                                                                                                                                |
   |                       |    ), y(id) AS                                                                      |                                                                                                                                                                                |
   |                       |    (                                                                                |                                                                                                                                                                                |
   |                       |    select count(1) from pg_class where oid=1247                                     |                                                                                                                                                                                |
   |                       |    UNION ALL                                                                        |                                                                                                                                                                                |
   |                       |    SELECT id+1 FROM x WHERE id < 10                                                 |                                                                                                                                                                                |
   |                       |    )                                                                                |                                                                                                                                                                                |
   |                       |    SELECT y.*, x.* FROM y LEFT JOIN x USING (id) ORDER BY 1;                        |                                                                                                                                                                                |
   +-----------------------+-------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | 5                     | .. code-block::                                                                     | LOG: SQL can't be shipped, reason: With-Recursive contains only values rte is not shippable                                                                                    |
   |                       |                                                                                     |                                                                                                                                                                                |
   |                       |    WITH RECURSIVE t(n) AS (                                                         | Only **VALUES** is used for scanning base tables. In this case, the statement can be executed on the CN, and DNs are unnecessary.                                              |
   |                       |    VALUES (1)                                                                       |                                                                                                                                                                                |
   |                       |    UNION ALL                                                                        |                                                                                                                                                                                |
   |                       |    SELECT n+1 FROM t WHERE n < 100                                                  |                                                                                                                                                                                |
   |                       |    )                                                                                |                                                                                                                                                                                |
   |                       |    SELECT sum(n) FROM t;                                                            |                                                                                                                                                                                |
   +-----------------------+-------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | 6                     | .. code-block::                                                                     | LOG: SQL can't be shipped, reason: With-Recursive recursive term correlated only is not shippable                                                                              |
   |                       |                                                                                     |                                                                                                                                                                                |
   |                       |    select  a.ID,a.Name,                                                             | The correlation conditions of correlated subqueries are only in the recursion part, and the non-recursion part has no correlation condition.                                   |
   |                       |    (                                                                                |                                                                                                                                                                                |
   |                       |    with recursive cte as (                                                          |                                                                                                                                                                                |
   |                       |    select ID, PID, NAME from b where b.ID = 1                                       |                                                                                                                                                                                |
   |                       |    union all                                                                        |                                                                                                                                                                                |
   |                       |    select parent.ID,parent.PID,parent.NAME                                          |                                                                                                                                                                                |
   |                       |    from cte as child join b as parent on child.pid=parent.id                        |                                                                                                                                                                                |
   |                       |    where child.ID = a.ID                                                            |                                                                                                                                                                                |
   |                       |    )                                                                                |                                                                                                                                                                                |
   |                       |    select NAME from cte limit 1                                                     |                                                                                                                                                                                |
   |                       |    ) cName                                                                          |                                                                                                                                                                                |
   |                       |    from                                                                             |                                                                                                                                                                                |
   |                       |    (                                                                                |                                                                                                                                                                                |
   |                       |    select id, name, count(*) as cnt                                                 |                                                                                                                                                                                |
   |                       |    from a group by id,name                                                          |                                                                                                                                                                                |
   |                       |    ) a order by 1,2;                                                                |                                                                                                                                                                                |
   +-----------------------+-------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | 7                     | .. code-block::                                                                     | LOG: SQL can't be shipped, reason: With-Recursive contains conflict distribution in none-recursive(Replicate) recursive(Hash)                                                  |
   |                       |                                                                                     |                                                                                                                                                                                |
   |                       |    WITH recursive t_result AS (                                                     | The **replicate** plan is used for **limit** in the non-recursion part but the **hash** plan is used in the recursion part, resulting in conflicts.                            |
   |                       |    select * from(                                                                   |                                                                                                                                                                                |
   |                       |    SELECT dm,sj_dm,name,1 as level                                                  |                                                                                                                                                                                |
   |                       |    FROM test_rec_part                                                               |                                                                                                                                                                                |
   |                       |    WHERE sj_dm < 10 order by dm limit 6 offset 2)                                   |                                                                                                                                                                                |
   |                       |    UNION all                                                                        |                                                                                                                                                                                |
   |                       |    SELECT t2.dm,t2.sj_dm,t2.name||' > '||t1.name,t1.level+1                         |                                                                                                                                                                                |
   |                       |    FROM t_result t1                                                                 |                                                                                                                                                                                |
   |                       |    JOIN test_rec_part t2 ON t2.sj_dm = t1.dm                                        |                                                                                                                                                                                |
   |                       |    )                                                                                |                                                                                                                                                                                |
   |                       |    SELECT * FROM t_result t;                                                        |                                                                                                                                                                                |
   +-----------------------+-------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | 8                     | .. code-block::                                                                     | LOG: SQL can't be shipped, reason: Recursive CTE references recursive CTE "cte"                                                                                                |
   |                       |                                                                                     |                                                                                                                                                                                |
   |                       |    with recursive cte as                                                            | **recursive** of multiple-layers are nested. That is, a **recursive** is nested in the recursion part of another **recursive**.                                                |
   |                       |    (                                                                                |                                                                                                                                                                                |
   |                       |    select * from rec_tb4 where id<4                                                 |                                                                                                                                                                                |
   |                       |    union all                                                                        |                                                                                                                                                                                |
   |                       |    select h.id,h.parentID,h.name from                                               |                                                                                                                                                                                |
   |                       |    (                                                                                |                                                                                                                                                                                |
   |                       |    with recursive cte as                                                            |                                                                                                                                                                                |
   |                       |    (                                                                                |                                                                                                                                                                                |
   |                       |    select * from rec_tb4 where id<4                                                 |                                                                                                                                                                                |
   |                       |    union all                                                                        |                                                                                                                                                                                |
   |                       |    select h.id,h.parentID,h.name from rec_tb4 h inner join cte c on h.id=c.parentID |                                                                                                                                                                                |
   |                       |    )                                                                                |                                                                                                                                                                                |
   |                       |    SELECT id ,parentID,name from cte order by parentID                              |                                                                                                                                                                                |
   |                       |    ) h                                                                              |                                                                                                                                                                                |
   |                       |    inner join cte  c on h.id=c.parentID                                             |                                                                                                                                                                                |
   |                       |    )                                                                                |                                                                                                                                                                                |
   |                       |    SELECT id ,parentID,name from cte order by parentID,1,2,3;                       |                                                                                                                                                                                |
   +-----------------------+-------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _en-us_topic_0000001145894663__s926a7f64d03546399fe529744b9bf420:

Functions That Do Not Support Pushdown
--------------------------------------

This module describes the variability of functions. The function variability in GaussDB(DWS) is as follows:

-  **IMMUTABLE**

   Indicates that the function always returns the same result if the parameter values are the same.

-  **STABLE**

   Indicates that the function cannot modify the database, and that within a single table scan it will consistently return the same result for the same parameter values, but that its result varies by SQL statements.

-  **VOLATILE**

   Indicates that the function value can change even within a single table scan, so no optimizations can be made.

The volatility of a function can be obtained by querying its **provolatile** column in **pg_proc**. The value **i** indicates immutable, **s** indicates stable, and **v** indicates volatile. The valid values of the **proshippable** column in **pg_proc** are **t**, **f**, and **NULL**. This column and the **provolatile** column together describe whether a function is pushed down.

-  If the **provolatile** of a function is **i**, the function can be pushed down regardless of the value of **proshippable**.
-  If the **provolatile** of a function is **s** or **v**, the function can be pushed only if the value of **proshippable** is **t**.
-  CTEs containing random are not pushed down, because pushdown may lead to incorrect results.

For a UDF, you can specify the values of **provolatile** and **proshippable** during its creation. For details, see CREATE FUNCTION.

In scenarios where a function does not support pushdown, perform one of the following as required:

-  If it is a system function, replace it with a functionally equivalent one.
-  If it is a UDF function, check whether its **provolatile** and **proshippable** are correctly defined.

Example: UDF
------------

Define a user-defined function that generates fixed output for a certain input as the **immutable** type.

Take the sales information of TPCDS as an example. If you want to write a function to calculate the discount data of a product, you can define the function as follows:

::

   CREATE FUNCTION func_percent_2 (NUMERIC, NUMERIC) RETURNS NUMERIC
   AS 'SELECT $1 / $2 WHERE $2 > 0.01'
   LANGUAGE SQL
   VOLATILE;

Run the following statement:

::

   SELECT func_percent_2(ss_sales_price, ss_list_price)
   FROM store_sales;

The execution plan is as follows:

|image1|

**func_percent_2** is not pushed down, and **ss_sales_price** and **ss_list_price** are executed on a CN. In this case, a large amount of resources on the CN is consumed, and the performance deteriorates as a result.

In this example, the function returns certain output when certain input is entered. Therefore, we can modify the function to the following one:

::

   CREATE FUNCTION func_percent_1 (NUMERIC, NUMERIC) RETURNS NUMERIC
   AS 'SELECT $1 / $2 WHERE $2 > 0.01'
   LANGUAGE SQL
   IMMUTABLE;

Run the following statement:

::

   SELECT func_percent_1(ss_sales_price, ss_list_price)
   FROM store_sales;

The execution plan is as follows:

|image2|

**func_percent_1** is pushed down to DNs for quicker execution. (In TPCDS 1000X, where three CNs and 18 DNs are used, the query efficiency is improved by over 100 times).

Example 2: Pushing Down the Sorting Operation
---------------------------------------------

For details, see :ref:`Case: Pushing Down Sort Operations to DNs <dws_04_0478>`.

.. |image1| image:: /_static/images/en-us_image_0000001098655330.png
.. |image2| image:: /_static/images/en-us_image_0000001145895125.png
