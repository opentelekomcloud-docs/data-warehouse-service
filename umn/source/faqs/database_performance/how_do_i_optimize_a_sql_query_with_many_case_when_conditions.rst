:original_name: dws_03_2116.html

.. _dws_03_2116:

How Do I Optimize a SQL Query with Many CASE WHEN Conditions?
=============================================================

In service queries, the **CASE WHEN** statement checks conditions. Too many unnecessary **CASE WHEN** statements in an SQL query can affect performance.

::

   SELECT
     SUM(CASE WHEN a > 1 THEN 1 ELSE 0 END) AS a1,
     SUM(CASE WHEN a > 2 THEN 1 ELSE 0 END) AS a2,
     ...
   FROM test
   WHERE dt = '20241225';

In this example, the **CASE WHEN** statement must run for each branch, which increases the query time and affects performance.

GaussDB(DWS) offers these optimization policies to address this issue:

Using a Temporary Result Set or Subquery
----------------------------------------

Extract the complex **CASE WHEN** calculations into a temporary result set or subquery. This avoids repeating the same logic in the main query.

Create a subquery to calculate the intermediate result.

::

   SELECT
     sub.a1,
     sub.a2
   FROM (
     SELECT
       sum(case when a > 1 then 1 else 0 end) AS a1,
       sum(case when a > 2 then 1 else 0 end) AS a2
     FROM test
     WHERE dt = '20241225'
   ) sub;


   SELECT
       SUM(case_when_a1) as a1,
       SUM(case_when_a2) as a2,
       ...
   FROM (
       SELECT
           CASE WHEN a > 1 THEN 1 ELSE 0 END AS case_when_a1,
           CASE WHEN a > 2 THEN 1 ELSE 0 END AS case_when_a2,
           ...
       FROM test
       WHERE dt = '20241225'
   ) AS subquery;

Using a User-Defined Function
-----------------------------

Encapsulate the **CASE WHEN** logic in a function. Then, call the function in your query instead of rewriting the **CASE WHEN** logic multiple times.

Create a simple function count_a_gt_value.

::

   CREATE OR REPLACE FUNCTION count_a_gt_value(val INT)
   RETURNS INT AS $$
   DECLARE
       result INT;
   BEGIN
       SELECT sum(CASE WHEN a > val THEN 1 ELSE 0 END)
         INTO result
         FROM test
         WHERE dt = '20241225';
       RETURN result;
   END;
   $$ LANGUAGE plpgsql;

Use the user-defined function count_a_gt_value for query.

::

   SELECT
     count_a_gt_value(1) AS a1,
     count_a_gt_value(2) AS a2
   FROM test;
