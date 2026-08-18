:original_name: dws_04_1419.html

.. _dws_04_1419:

Text Matching Query Rewriting
=============================

DWS supports text matching query rewriting, including complex queries such as **JOIN**, **AGG**, **CTE**, and subqueries.

Constraints
-----------

-  Subqueries of a query can also use materialized views for text matching.
-  If the query statement contains **ORDER BY**, the **ORDER BY** column must exist in the output column of the materialized view.
-  The distinctions between a query and a materialized view can include spaces, line breaks, and comments.
-  If an alias is assigned to the output column of a materialized view, the same alias must be used in the query for proper matching.
-  Queries in materialized views cannot contain **FOR UPDATE**.

Examples
--------

Take tpch Q1 as an example. When a materialized view is generated from the intricate query in Q1, the query plan indicates that the statement has been modified to access the materialized view.

::

   CREATE MATERIALIZED VIEW mv_tpch_q1 enable query rewrite as
   SELECT
           l_returnflag,
           l_linestatus,
           sum(l_quantity) AS sum_qty,
           sum(l_extendedprice) AS sum_base_price,
           sum(l_extendedprice * (1 - l_discount)) AS sum_disc_price,
           sum(l_extendedprice * (1 - l_discount) * (1 + l_tax)) AS sum_charge,
           avg(l_quantity) AS avg_qty,
           avg(l_extendedprice) AS avg_price,
           avg(l_discount) AS avg_disc,
           count(*) AS count_order
   FROM
           lineitem
   WHERE l_shipdate <= DATE '1998-12-01' - INTERVAL '3 day'
   GROUP BY l_returnflag,l_linestatus
   ORDER BY sum_qty;

   explain (costs off)
   SELECT
           l_returnflag,
           l_linestatus,
           sum(l_quantity) AS sum_qty,
           sum(l_extendedprice) AS sum_base_price,
           sum(l_extendedprice * (1 - l_discount)) AS sum_disc_price,
           sum(l_extendedprice * (1 - l_discount) * (1 + l_tax)) AS sum_charge,
           avg(l_quantity) AS avg_qty,
           avg(l_extendedprice) AS avg_price,
           avg(l_discount) AS avg_disc,
           count(*) AS count_order
   FROM
           lineitem
   WHERE l_shipdate <= DATE '1998-12-01' - INTERVAL '3 day'
   GROUP BY l_returnflag,l_linestatus
   ORDER BY sum_qty;
                    QUERY PLAN
   --------------------------------------------
     id |              operation
    ----+-------------------------------------
      1 | ->  Streaming (type: GATHER)
      2 |    ->  Sort
      3 |       ->  Seq Scan on mv_tpch_q1

If the **ORDER BY** section aligns with the materialized view definition, complete matching can also be executed.

::

   CREATE TABLE t1 (a int,b int);
   CREATE MATERIALIZED VIEW mv_t1 enable query rewrite AS SELECT sum(a), b FROM t1 GROUP BY b;

   EXPLAIN (costs off) SELECT sum(a), b FROM t1 GROUP BY b ORDER BY 1;
                QUERY PLAN
   -------------------------------------
     id |          operation
    ----+------------------------------
      1 | ->  Streaming (type: GATHER)
      2 |    ->  Sort
      3 |       ->  Seq Scan on mv_t1
