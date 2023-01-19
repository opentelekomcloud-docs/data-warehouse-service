:original_name: dws_04_0488.html

.. _dws_04_0488:

Case: Rewriting SQL Statements and Eliminating Prune Interference
=================================================================

Symptom
-------

In a test at a site, **ddw_f10_op_cust_asset_mon** is a partitioned table and the partition key is **year_mth** whose value is a combined string of month and year values.

The following figure shows the tested SQL statements:

::

   select
       count(1)
   from t_ddw_f10_op_cust_asset_mon b1
   where b1.year_mth between to_char(add_months(to_date(''20170222'','yyyymmdd'), -11),'yyyymm') and substr(''20170222'',1 ,6 );

The test result shows the SQL Scan table takes 135s. This may be the performance bottleneck.

.. note::

   add_months is a local adaptation function.

   ::

      CREATE OR REPLACE FUNCTION ADD_MONTHS(date, integer) RETURNS date
          AS $$
          SELECT
          CASE
          WHEN (EXTRACT(day FROM $1) = EXTRACT(day FROM (date_trunc('month', $1) + INTERVAL '1 month - 1 day'))) THEN
              date_trunc('month', $1) + CAST($2 + 1 || ' month - 1 day' as interval)
          ELSE
              $1 + CAST($2 || ' month' as interval)
          END
          $$
          LANGUAGE SQL
          IMMUTABLE;

Optimization
------------

According to the statement execution plan, the base table filter is displayed as follows:

.. code-block::

   Filter: (((year_mth)::text <= '201702'::text) AND ((year_mth)::text >= to_char(add_months(to_date('20170222'::text, 'YYYYMMDD'::text), (-11)), 'YYYYMM'::text)))

The query condition expression to_char(add_months(to_date(''20170222'','yyyymmdd'),-11),'yyyymm') exists in the filter condition, and this non-constant expression cannot be used for pruning. Therefore, all data of query statements in the partitioned tables is scanned.

to_date and to_char are stable functions as queried in the pg_proc. Based on the function behaviors described in Postgresql, this type of function cannot be converted into the Const value in the preprocessing phase, which is the root cause of preventing partition pruning.

Based on the preceding analysis, the optimization expression can be used for partition pruning, which is the key to performance optimization. The original SQL statements can be written to as follows:

::

   select
       count(1)
   from t_ddw_f10_op_cust_asset_mon b1
   where b1.year_mth between(substr(ADD_MONTHS('20170222'::date, -11), 1, 4)||substr(ADD_MONTHS('20170222'::date, -11), 6, 2)) and substr(''20170222'',1 ,6 );

The execution time of modified SQL statements is reduced from 135s to 18s.
