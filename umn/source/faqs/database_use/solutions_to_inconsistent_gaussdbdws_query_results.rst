:original_name: dws_03_2107.html

.. _dws_03_2107:

Solutions to Inconsistent GaussDB(DWS) Query Results
====================================================

In GaussDB(DWS), inconsistencies in the returned results of the same query statement when using SQL queries occur. Such issues are primarily due to improper syntax or usage. Proper service usage can prevent these problems. The following are some examples of query results inconsistency along with the solutions.

Window Function Results Are Incompletely Sorted
-----------------------------------------------

**Scenario:**

In the window function **row_number()**, column **c** of table **t3** is queried after sorting. The two query results are different.

::

   SELECT * FROM t3 order by 1,2,3;
    a | b | c
   ---+---+---
    1 | 2 | 1
    1 | 2 | 2
    1 | 2 | 3
   (3 rows)

   SELECT c,rn FROM (select c,row_number() over(order by a,b) as rn from t3) where rn = 1;
    c | rn
   ---+----
   1  |  1
   (1 row)
   SELECT c,rn FROM (select c,row_number() over(order by a,b) as rn from t3) where rn = 1;
    c | rn
   ---+----
    3 |  1
   (1 row)

**Analysis:**

As shown above, run **select c,rn from (select c,row_number() over(order by a,b) as rn from t3) where rn = 1;** twice, the results are different. That is because duplicate values **1** and **2** exist in the sorting columns **a** and **b** of the window function while their values in column **c** are different. As a result, when the first record is obtained based on the sorting result in columns **a** and **b**, the obtained data in column **c** is random, as a result, the result sets are inconsistent.

**Solution:**

The values in column **c** need to be added to the sorting.

::

   SELECT c,rn FROM (select c,row_number() over(order by a,b,c) as rn from t3) where rn = 1;
    c | rn
   ---+----
    1 |  1
   (1 row)

Using Sorting in Subviews/Subqueries
------------------------------------

**Scenario**

After table **test** and view **v** are created, the query results are inconsistent when sorting is used to query table **test** in a subquery.

::

   CREATE TABLE test(a serial ,b int);
   INSERT INTO test(b) VALUES(1);
   INSERT INTO test(b) SELECT b FROM test;
   ...
   INSERT INTO test(b) SELECT b FROM test;
   CREATE VIEW v as SELECT * FROM test ORDER BY a;

Problem SQL:

::

   SELECT * FROM v limit 1;
    a | b
   ---+---
    3 | 1
   (1 row)

   SELECT * FROM (select * from test order by a) limit 10;
    a  | b
   ----+---
    14 | 1
   (1 row)

   SELECT * FROM test order by a limit 10;
    a | b
   ---+---
    1 | 1
   (1 row)

**Analysis:**

**ORDER BY** is invalid for subviews and subqueries.

**Solution:**

You are not advised to use **ORDER BY** in subviews and subqueries. To ensure that the results are in order, use **ORDER BY** in the outermost query.

LIMIT in Subqueries
-------------------

**Scenario**: When **LIMIT** is used in a subquery, the two query results are inconsistent.

::

   SELECT * FROM (select a from test limit 1 ) order by 1;
    a
   ---
    5
   (1 row)

   SELECT * FROM (select a from test limit 1 ) order by 1;
    a
   ---
    1
   (1 row)

**Analysis:**

The LIMIT in the subquery causes random results to be obtained.

**Solution:**

To ensure the stability of the final query result, do not use **LIMIT** in subqueries.

Using String_agg
----------------

**Scenario**: When **string_agg** is used to query the table **employee**, the query results are inconsistent.

::

   SELECT * FROM employee;
    empno | ename  |   job   | mgr  |      hiredate       |  sal  | comm | deptno
   -------+--------+---------+------+---------------------+-------+------+--------
     7654 | MARTIN | SALEMAN | 7698 | 2022-11-08 00:00:00 | 12000 | 1400 |     30
     7566 | JONES  | MANAGER | 7839 | 2022-11-08 00:00:00 | 32000 |    0 |     20
     7499 | ALLEN  | SALEMAN | 7698 | 2022-11-08 00:00:00 | 16000 |  300 |     30
   (3 rows)

   SELECT count(*) FROM (select deptno, string_agg(ename, ',') from employee group by deptno) t1, (select deptno, string_agg(ename, ',') from employee group by deptno) t2 where t1.string_agg = t2.string_agg;
    count
   -------
        2
   (1 row)

   SELECT count(*) FROM (select deptno, string_agg(ename, ',') from employee group by deptno) t1, (select deptno, string_agg(ename, ',') from employee group by deptno) t2 where t1.string_agg = t2.string_agg;
    count
   -------
        1
   (1 row)

**Analysis:**

The **string_agg** function is used to concatenate data in a group into one row. However, if you use **string_agg(ename, ',')**, the order of concatenated results needs to be specified. For example, in the preceding statement, **select deptno, string_agg(ename, ',') from employee group by deptno;**

can output either of the following:

::

   30 | ALLEN,MARTIN

Or:

::

   30 |MARTIN,ALLEN

In the preceding scenario, the result of subquery **t1** may be different from that of subquery **t2** when deptno is **30**.

**Solution:**

Add **ORDER BY** to **String_agg** to ensure that data is concatenated in sequence.

::

   SELECT count(*) FROM (select deptno, string_agg(ename, ',' order by ename desc) from employee group by deptno) t1 ,(select deptno, string_agg(ename, ',' order by ename desc) from employee group by deptno) t2 where t1.string_agg = t2.string_agg;

Database Compatibility Mode
---------------------------

**Scenario:** The query results of empty strings in the database are inconsistent.

database1 (TD-compatible):

::

   td=# select '' is null;
    isnull
   --------
    f
   (1 row)

database2 (ORA compatible):

::

   ora=# select '' is null;
    isnull
   --------
    t
   (1 row)

**Analysis:**

The empty string query results are different because the syntax of the empty string is different from that of the null string in different database compatibility.

GaussDB(DWS) currently supports three database compatibility modes: Oracle, TD, and MySQL. The syntax and behavior vary depending on the compatibility mode. For details about the compatibility differences, see "Syntax Compatibility Differences Among Oracle, Teradata, and MySQL" in *GaussDB(DWS) Developer Guide*.

Databases in different compatibility modes have different compatibility issues. You can run **select datname, datcompatibility from pg_database;** to check the database compatibility.

**Solution:**

The problem is solved when the compatibility modes of the databases in the two environments are set to the same. The **DBCOMPATIBILITY** attribute of a database does not support **ALTER**. You can only specify the same **DBCOMPATIBILITY** attribute when creating a database.

The configuration item **behavior_compat_options** for database compatibility behaviors is configured inconsistently.
---------------------------------------------------------------------------------------------------------------------

**Scenario:** The calculation results of the **add_months** function are inconsistent.

database1:

::

   SELECT add_months('2018-02-28',3) from dual;
   add_months
   ---------------------
   2018-05-28 00:00:00
   (1 row)

database2:

::

   SELECT add_months('2018-02-28',3) from dual;
   add_months
   ---------------------
   2018-05-31 00:00:00
   (1 row)

**Analysis:**

Some behaviors may vary depending on the settings of the database compatibility configuration item **behavior_compat_options**. For details about the options of this item, see "GUC Parameters > Miscellaneous Parameters > behavior_compat_options" in *GaussDB(DWS) Developer Guide*..

The **end_month_calculate** in **behavior_compat_options** controls the calculation logic of the **add_months** function. If this parameter is specified, and the **Day** of **param1** indicates the last day of a month shorter than **result**, the **Day** in the calculation result will equal that in **result**.

**Solution:**

The **behavior_compat_options** parameter must be configured consistently. This parameter is of the **USERSET** type and can be set at the session level or modified at the cluster level.

The attributes of the user-defined function are not properly set.
-----------------------------------------------------------------

**Scenario:** When the customized function **get_count()** is invoked, the results are inconsistent.

::

   CREATE FUNCTION get_count() returns int
   SHIPPABLE
   as $$
   declare
       result int;
   begin
   result = (select count(*) from test); --test table is a hash table.
       return result;
   end;
   $$
   language plpgsql;

Call this function.

::

   SELECT get_count();
    get_count
   -----------
         2106
   (1 row)

   SELECT get_count() FROM t_src;
    get_count
   -----------
         1032
   (1 row)

**Analysis:**

This function specifies the **SHIPPABLE** attribute. When a plan is generated, the function pushes it down to DNs for execution. The test table defined in the function is a hash table. Therefore, each DN has only part of the data in the table, the result returned by **select count(*) from test;** is not the result of full data in the test table. The expected result changes after **from** is added.

**Solution:**

Use either of the following methods (the first method is recommended):

#. Change the function to not push down: **ALTER FUNCTION get_count() not shippable;**
#. Change the table used in the function to a replication table. In this way, the full data of the table is stored on each DN. Even if the plan is pushed down to DNs for execution, the result set will be as expected.

Using the Unlogged Table
------------------------

**Scenario:**

After an unlogged table is used and the cluster is restarted, the associated query result set is abnormal, and some data is missing in the unlogged table.

**Analysis:**

If **max_query_retry_times** is set to **0** and the keyword **UNLOGGED** is specified during table creation, the created table will be an unlogged table. Data written to unlogged tables is not written to the write-ahead log, which makes them considerably faster than ordinary tables. However, an unlogged table is automatically truncated after a crash or unclean shutdown, incurring data loss risks. The contents of an unlogged table are also not replicated to standby servers. Any indexes created on an unlogged table are not automatically logged as well. If the cluster restarts unexpectedly (process restart, node fault, or cluster restart), some data in the memory is not flushed to disks in a timely manner, and some data is lost, causing the result set to be abnormal.

**Solution:**

The security of unlogged tables cannot be ensured if the cluster goes faulty. In most cases, unlogged tables are only used as temporary tables. If a cluster is faulty, you need to rebuild the unlogged table or back up the data and import it to the database again to ensure that the data is normal.
