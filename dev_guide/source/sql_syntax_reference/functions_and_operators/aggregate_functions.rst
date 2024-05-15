:original_name: dws_06_0046.html

.. _dws_06_0046:

Aggregate Functions
===================

sum(expression)
---------------

Description: Sum of expression across all input values

Return type:

Generally, same as the argument data type. In the following cases, type conversion occurs:

-  **BIGINT** for **SMALLINT** or **INT** arguments
-  **NUMBER** for **BIGINT** arguments
-  **DOUBLE PRECISION** for floating-point arguments

Examples:

::

   SELECT SUM(ss_ext_tax) FROM tpcds.STORE_SALES;
     sum
   --------------
    213267594.69
   (1 row)

max(expression)
---------------

Description: Specifies the maximum value of **expression** across all input values.

Argument types: any array, numeric, string, or date/time type

Return type: same as the argument type

Examples:

::

   SELECT MAX(inv_quantity_on_hand) FROM tpcds.inventory;
      max
   ---------
    1000000
   (1 row)

min(expression)
---------------

Description: Specifies the minimum value of **expression** across all input values.

Argument types: any array, numeric, string, or date/time type

Return type: same as the argument type

Examples:

::

   SELECT MIN(inv_quantity_on_hand) FROM tpcds.inventory;
    min
   -----
      0
   (1 row)

avg(expression)
---------------

Description: Average (arithmetic mean) of all input values

If the input parameter type is DOUBLE PRECISION, it accepts values from 1.34E-154 to 1.34E+154. Values outside this range cause the "value out of range: overflow" error. To avoid this, use the cast function to change the column type to numeric.

Return type:

-  **NUMBER** for any integer-type argument.
-  **DOUBLE PRECISION** for floating-point parameters.
-  Other values are the same as the input data type.

Examples:

::

   SELECT AVG(inv_quantity_on_hand) FROM tpcds.inventory;
            avg
   ----------------------
    500.0387129084044604
   (1 row)

median(expression)
------------------

Description: Median of all input values Currently, only the numeric and interval types are supported. Null values are not used for calculation.

Return type:

-  If all input values are integers, a median of the **NUMERIC** type is returned; otherwise, a median of the same type as the input values is returned.
-  In the Teradata-compatible mode, if the input values are integers, the returned median is rounded to the nearest integer.

Examples:

::

   SELECT MEDIAN(inv_quantity_on_hand) FROM tpcds.inventory;
    median
   --------
       500
   (1 row)

percentile_cont(const) within group(order by expression)
--------------------------------------------------------

Description: returns a value corresponding to the specified percentile in the ordering, interpolating between adjacent input items if needed. Null values are not used for calculation.

Input: *const* indicates a number ranging from 0 to 1. Currently, only numeric and interval expressions are supported.

Return type:

-  If all input values are integers, a median of the **NUMERIC** type is returned; otherwise, a median of the same type as the input values is returned.
-  In the Teradata-compatible mode, if the input values are integers, the returned median is rounded to the nearest integer.

Examples:

::

   SELECT percentile_cont(0.3) within group(order by x) FROM (SELECT generate_series(1,5) AS x) AS t;
   percentile_cont
   -----------------
   2.2
   (1 row)
   SELECT percentile_cont(0.3) within group(order by x desc) FROM (SELECT generate_series(1,5) AS x) AS t;
   percentile_cont
   -----------------
   3.8
   (1 row)

percentile_disc(const) within group(order by expression)
--------------------------------------------------------

Description: returns the first input value whose position in the ordering equals or exceeds the specified percentile.

Input: *const* indicates a number ranging from 0 to 1. Currently, only numeric and interval expressions are supported. Null values are not used for calculation.

Return type: If all input values are integers, a median of the **NUMERIC** type is returned; otherwise, a median of the same type as the input values is returned.

Examples:

::

   SELECT percentile_disc(0.3) within group(order by x) FROM (SELECT generate_series(1,5) AS x) AS t;
   percentile_disc
   -----------------
   2
   (1 row)
   SELECT percentile_disc(0.3) within group(order by x desc) FROM (SELECT generate_series(1,5) AS x) AS t;
   percentile_disc
   -----------------
   4
   (1 row)

count(expression)
-----------------

Description: Number of input rows for which the value of expression is not null

Return type: bigint

Examples:

::

   SELECT COUNT(inv_quantity_on_hand) FROM tpcds.inventory;
     count
   ----------
    11158087
   (1 row)

count(*)
--------

Description: Number of input rows

Return type: bigint

Examples:

::

   SELECT COUNT(*) FROM tpcds.inventory;
     count
   ----------
    11745000
   (1 row)

array_agg(expression)
---------------------

Description: Input values, including nulls, concatenated into an array The input parameters of the function do not support the array format.

Return type: array of the argument type

Examples:

Create the **employeeinfo** table and insert data into the table:

::

   CREATE TABLE employeeinfo (empno smallint, ename varchar(20), job varchar(20), hiredate date,deptno smallint);
   INSERT INTO employeeinfo VALUES (7155, 'JACK', 'SALESMAN', '2018-12-01', 30);
   INSERT INTO employeeinfo VALUES (7003, 'TOM', 'FINANCE', '2016-06-15', 20);
   INSERT INTO employeeinfo VALUES (7357, 'MAX', 'SALESMAN', '2020-10-01', 30);

   SELECT * FROM employeeinfo;
    empno | ename |   job    |      hiredate       | deptno
   -------+-------+----------+---------------------+--------
     7155 | JACK  | SALESMAN | 2018-12-01 00:00:00 |     30
     7357 | MAX   | SALESMAN | 2020-10-01 00:00:00 |     30
     7003 | TOM   | FINANCE  | 2016-06-15 00:00:00 |     20
   (3 rows)

Query the names of all employees in the department whose ID is **30**:

::

   SELECT array_agg(ename) FROM employeeinfo where deptno = 30;
    array_agg
   ------------
    {JACK,MAX}
   (1 row)

Query all employees in the same department:

::

   SELECT deptno, array_agg(ename) FROM employeeinfo group by deptno;
    deptno | array_agg
   --------+------------
        30 | {JACK,MAX}
        20 | {TOM}
   (2 rows)

   SELECT distinct array_agg(ename) OVER (PARTITION BY deptno) FROM employeeinfo;
    array_agg
   ------------
    {TOM}
    {JACK,MAX}
   (2 rows)

Query all department IDs and deduplicate them:

::

   SELECT array_agg(distinct deptno) FROM employeeinfo group by deptno;
    array_agg
   -----------
    {20}
    {30}
   (2 rows)

Sort the deduplicated department IDs in descending order:

::

   SELECT array_agg(distinct deptno order by deptno desc) FROM employeeinfo;
    array_agg
   -----------
    {30,20}
   (1 row)

string_agg(expression, delimiter)
---------------------------------

Description: Input values concatenated into a string, separated by delimiter

Return type: same as the argument type

Examples:

Query all employees in the same department based on the created table **employeeinfo**:

::

   SELECT deptno, string_agg(ename,',') FROM employeeinfo group by deptno;
    deptno | string_agg
   --------+------------
        30 | JACK,MAX
        20 | TOM
   (2 rows)

Query employees whose work IDs are smaller than 7156:

::

   SELECT string_agg(ename,',') FROM employeeinfo where empno < 7156;
    string_agg
   ------------
    TOM,JACK
   (1 row)

listagg(expression [, delimiter]) WITHIN GROUP(ORDER BY order-list)
-------------------------------------------------------------------

Description: Aggregation column data sorted according to the mode specified by **WITHIN GROUP**, and concatenated to a string using the specified delimiter

-  **expression**: Mandatory. It specifies an aggregation column name or a column-based, valid expression. It does not support the **DISTINCT** keyword and the **VARIADIC** parameter.
-  **delimiter**: Optional. It specifies a delimiter, which can be a string constant or a deterministic expression based on a group of columns. The default value is empty.
-  **order-list**: Mandatory. It specifies the sorting mode in a group.

Return type: text

.. note::

   **listagg** is a column-to-row aggregation function, compatible with Oracle Database 11g Release 2. You can specify the **OVER** clause as a window function. When **listagg** is used as a window function, the **OVER** clause does not support the window sorting or framework of **ORDER BY**, so as to avoid ambiguity in **listagg** and **ORDER BY** of the **WITHIN GROUP** clause.

Examples:

The aggregation column is of the text character set type:

::

   SELECT deptno, listagg(ename, ',') WITHIN GROUP(ORDER BY ename) AS employees FROM emp GROUP BY deptno;
    deptno |              employees
   --------+--------------------------------------
        10 | CLARK,KING,MILLER
        20 | ADAMS,FORD,JONES,SCOTT,SMITH
        30 | ALLEN,BLAKE,JAMES,MARTIN,TURNER,WARD
   (3 rows)

The aggregation column is of the integer type:

::

   SELECT deptno, listagg(mgrno, ',') WITHIN GROUP(ORDER BY mgrno NULLS FIRST) AS mgrnos FROM emp GROUP BY deptno;
    deptno |            mgrnos
   --------+-------------------------------
        10 | 7782,7839
        20 | 7566,7566,7788,7839,7902
        30 | 7698,7698,7698,7698,7698,7839
   (3 rows)

The aggregation column is of the floating point type:

::

   SELECT job, listagg(bonus, '($); ') WITHIN GROUP(ORDER BY bonus DESC) || '($)' AS bonus FROM emp GROUP BY job;
       job     |                      bonus
   ------------+-------------------------------------------------
    CLERK      | 10234.21($); 2000.80($); 1100.00($); 1000.22($)
    PRESIDENT  | 23011.88($)
    ANALYST    | 2002.12($); 1001.01($)
    MANAGER    | 10000.01($); 2399.50($); 999.10($)
    SALESMAN   | 1000.01($); 899.00($); 99.99($); 9.00($)
   (5 rows)

The aggregation column is of the time type:

::

   SELECT deptno, listagg(hiredate, ', ') WITHIN GROUP(ORDER BY hiredate DESC) AS hiredates FROM emp GROUP BY deptno;
    deptno |                                                          hiredates
   --------+------------------------------------------------------------------------------------------------------------------------------
        10 | 1982-01-23 00:00:00, 1981-11-17 00:00:00, 1981-06-09 00:00:00
        20 | 2001-04-02 00:00:00, 1999-12-17 00:00:00, 1987-05-23 00:00:00, 1987-04-19 00:00:00, 1981-12-03 00:00:00
        30 | 2015-02-20 00:00:00, 2010-02-22 00:00:00, 1997-09-28 00:00:00, 1981-12-03 00:00:00, 1981-09-08 00:00:00, 1981-05-01 00:00:00
   (3 rows)

The aggregation column is of the time interval type:

::

   SELECT deptno, listagg(vacationTime, '; ') WITHIN GROUP(ORDER BY vacationTime DESC) AS vacationTime FROM emp GROUP BY deptno;
    deptno |                                    vacationtime
   --------+------------------------------------------------------------------------------------
        10 | 1 year 30 days; 40 days; 10 days
        20 | 70 days; 36 days; 9 days; 5 days
        30 | 1 year 1 mon; 2 mons 10 days; 30 days; 12 days 12:00:00; 4 days 06:00:00; 24:00:00
   (3 rows)

By default, the delimiter is empty:

::

   SELECT deptno, listagg(job) WITHIN GROUP(ORDER BY job) AS jobs FROM emp GROUP BY deptno;
    deptno |                     jobs
   --------+----------------------------------------------
        10 | CLERKMANAGERPRESIDENT
        20 | ANALYSTANALYSTCLERKCLERKMANAGER
        30 | CLERKMANAGERSALESMANSALESMANSALESMANSALESMAN
   (3 rows)

When **listagg** is used as a window function, the **OVER** clause does not support the window sorting of **ORDER BY**, and the **listagg** column is an ordered aggregation of the corresponding groups.

::

   SELECT deptno, mgrno, bonus, listagg(ename,'; ') WITHIN GROUP(ORDER BY hiredate) OVER(PARTITION BY deptno) AS employees FROM emp;
    deptno | mgrno |  bonus   |                 employees
   --------+-------+----------+-------------------------------------------
        10 |  7839 | 10000.01 | CLARK; KING; MILLER
        10 |       | 23011.88 | CLARK; KING; MILLER
        10 |  7782 | 10234.21 | CLARK; KING; MILLER
        20 |  7566 |  2002.12 | FORD; SCOTT; ADAMS; SMITH; JONES
        20 |  7566 |  1001.01 | FORD; SCOTT; ADAMS; SMITH; JONES
        20 |  7788 |  1100.00 | FORD; SCOTT; ADAMS; SMITH; JONES
        20 |  7902 |  2000.80 | FORD; SCOTT; ADAMS; SMITH; JONES
        20 |  7839 |   999.10 | FORD; SCOTT; ADAMS; SMITH; JONES
        30 |  7839 |  2399.50 | BLAKE; TURNER; JAMES; MARTIN; WARD; ALLEN
        30 |  7698 |     9.00 | BLAKE; TURNER; JAMES; MARTIN; WARD; ALLEN
        30 |  7698 |  1000.22 | BLAKE; TURNER; JAMES; MARTIN; WARD; ALLEN
        30 |  7698 |    99.99 | BLAKE; TURNER; JAMES; MARTIN; WARD; ALLEN
        30 |  7698 |  1000.01 | BLAKE; TURNER; JAMES; MARTIN; WARD; ALLEN
        30 |  7698 |   899.00 | BLAKE; TURNER; JAMES; MARTIN; WARD; ALLEN
   (14 rows)

group_concat(expression [ORDER BY {col_name \| expr} [ASC \| DESC]] [SEPARATOR str_val])
----------------------------------------------------------------------------------------

Description: concatenates the specified **str_val** delimiters used by column data into a string. The concatenation uses a sorting method that must be specified by the **ORDER BY** clause. **ORDER BY 1** is not allowed.

-  **expression**: (mandatory) specifies a column name or a column-based valid expression. It does not support the **DISTINCT** keyword or the **VARIADIC** parameter.
-  **str_val**: (optional) specifies a delimiter, which can be a string constant or a deterministic expression based on grouped columns. The default value indicates that commas (,) are used as delimiters.

Return type: text

.. note::

   The group_concat function is supported only in 8.1.2 or later.

Examples:

The default delimiter is a comma (,).

::

   SELECT group_concat(sname) FROM group_concat_test;
                  group_concat
   ------------------------------------------
    ADAMS,FORD,JONES,KING,MILLER,SCOTT,SMITH
   (1 row)

Delimiters can be customized for the **group_concat** function.

::

   SELECT group_concat(sname separator ';') from group_concat_test;
                  group_concat
   ------------------------------------------
    ADAMS;FORD;JONES;KING;MILLER;SCOTT;SMITH
   (1 row)

The **group_concat** function supports the **ORDER BY** clause, which concatenates column data in sequence.

::

   SELECT group_concat(sname order by snumber separator ';') FROM group_concat_test;
                  group_concat
   ------------------------------------------
    MILLER;FORD;SCOTT;SMITH;KING;JONES;ADAMS
   (1 row)

covar_pop(Y, X)
---------------

Description: Overall covariance

Return type: double precision

Examples:

::

    SELECT COVAR_POP(sr_fee, sr_net_loss) FROM tpcds.store_returns WHERE sr_customer_sk < 1000;
       covar_pop
   ------------------
    829.749627587403
   (1 row)

covar_samp(Y, X)
----------------

Description: Sample covariance

Return type: double precision

Examples:

::

   SELECT COVAR_SAMP(sr_fee, sr_net_loss) FROM tpcds.store_returns WHERE sr_customer_sk < 1000;
       covar_samp
   ------------------
    830.052235037289
   (1 row)

stddev_pop(expression)
----------------------

Description: Overall standard difference

If the input parameter type is DOUBLE PRECISION, it accepts values from 1.34E-154 to 1.34E+154. Values outside this range cause the "value out of range: overflow" error. To avoid this, use the cast function to change the column type to numeric.

Return type: **double precision** for floating-point arguments, otherwise **numeric**

Examples:

::

   SELECT STDDEV_POP(inv_quantity_on_hand) FROM tpcds.inventory WHERE inv_warehouse_sk = 1;
       stddev_pop
   ------------------
    289.224294957556
   (1 row)

stddev_samp(expression)
-----------------------

Description: Sample standard deviation of the input values

If the input parameter type is DOUBLE PRECISION, it accepts values from 1.34E-154 to 1.34E+154. Values outside this range cause the "value out of range: overflow" error. To avoid this, use the cast function to change the column type to numeric.

Return type: **double precision** for floating-point arguments, otherwise **numeric**

Examples:

::

   SELECT STDDEV_SAMP(inv_quantity_on_hand) FROM tpcds.inventory WHERE inv_warehouse_sk = 1;
      stddev_samp
   ------------------
    289.224359757315
   (1 row)

var_pop(expression)
-------------------

Description: Population variance of the input values (square of the population standard deviation)

If the input parameter type is DOUBLE PRECISION, it accepts values from 1.34E-154 to 1.34E+154. Values outside this range cause the "value out of range: overflow" error. To avoid this, use the cast function to change the column type to numeric.

Return type: **double precision** for floating-point arguments, otherwise **numeric**

Examples:

::

   SELECT VAR_POP(inv_quantity_on_hand) FROM tpcds.inventory WHERE inv_warehouse_sk = 1;
         var_pop
   --------------------
    83650.692793695475
   (1 row)

var_samp(expression)
--------------------

Description: Sample variance of the input values (square of the sample standard deviation)

If the input parameter type is DOUBLE PRECISION, it accepts values from 1.34E-154 to 1.34E+154. Values outside this range cause the "value out of range: overflow" error. To avoid this, use the cast function to change the column type to numeric.

Return type: **double precision** for floating-point arguments, otherwise **numeric**

Examples:

::

   SELECT VAR_SAMP(inv_quantity_on_hand) FROM tpcds.inventory WHERE inv_warehouse_sk = 1;
         var_samp
   --------------------
    83650.730277028768
   (1 row)

bit_and(expression)
-------------------

Description: The bitwise AND of all non-null input values, or null if none

Return type: same as the argument type

Examples:

::

   SELECT BIT_AND(inv_quantity_on_hand) FROM tpcds.inventory WHERE inv_warehouse_sk = 1;
    bit_and
   ---------
          0
   (1 row)

bit_or(expression)
------------------

Description: The bitwise OR of all non-null input values, or null if none

Return type: same as the argument type

Examples:

::

   SELECT BIT_OR(inv_quantity_on_hand) FROM tpcds.inventory WHERE inv_warehouse_sk = 1;
    bit_or
   --------
      1023
   (1 row)

bool_and(expression)
--------------------

Description: Its value is **true** if all input values are **true**, otherwise **false**.

Return type: Boolean

Examples:

::

   SELECT bool_and(100 <2500);
    bool_and
   ----------
    t
   (1 row)

bool_or(expression)
-------------------

Description: Its value is **true** if at least one input value is **true**, otherwise **false**.

Return type: Boolean

Examples:

::

   SELECT bool_or(100 <2500);
    bool_or
   ----------
    t
   (1 row)

corr(Y, X)
----------

Description: Correlation coefficient

Return type: double precision

Examples:

::

   SELECT CORR(sr_fee, sr_net_loss) FROM tpcds.store_returns WHERE sr_customer_sk < 1000;
          corr
   -------------------
    .0381383624904186
   (1 row)

every(expression)
-----------------

Description: Equivalent to **bool_and**

Return type: Boolean

Examples:

::

   SELECT every(100 <2500);
    every
   -------
    t
   (1 row)

rank(expression)
----------------

Description: The tuples in different groups are sorted non-consecutively by **expression**.

Return type: bigint

Examples:

::

   SELECT d_moy, d_fy_week_seq, rank() OVER(PARTITION BY d_moy ORDER BY d_fy_week_seq) FROM tpcds.date_dim WHERE d_moy < 4 AND d_fy_week_seq < 7 ORDER BY 1,2;
      d_moy | d_fy_week_seq | rank
   -------+---------------+------
        1 |             1 |    1
        1 |             1 |    1
        1 |             1 |    1
        1 |             1 |    1
        1 |             1 |    1
        1 |             1 |    1
        1 |             1 |    1
        1 |             2 |    8
        1 |             2 |    8
        1 |             2 |    8
        1 |             2 |    8
        1 |             2 |    8
        1 |             2 |    8
        1 |             2 |    8
        1 |             3 |   15
        1 |             3 |   15
        1 |             3 |   15
        1 |             3 |   15
        1 |             3 |   15
        1 |             3 |   15
        1 |             3 |   15
        1 |             4 |   22
        1 |             4 |   22
        1 |             4 |   22
        1 |             4 |   22
        1 |             4 |   22
        1 |             4 |   22
        1 |             4 |   22
        1 |             5 |   29
        1 |             5 |   29
        2 |             5 |    1
        2 |             5 |    1
        2 |             5 |    1
        2 |             5 |    1
        2 |             5 |    1
        2 |             6 |    6
        2 |             6 |    6
        2 |             6 |    6
        2 |             6 |    6
        2 |             6 |    6
        2 |             6 |    6
        2 |             6 |    6
   (42 rows)

regr_avgx(Y, X)
---------------

Description: Average of the independent variable (**sum(X)/N**)

Return type: double precision

Examples:

::

   SELECT REGR_AVGX(sr_fee, sr_net_loss) FROM tpcds.store_returns WHERE sr_customer_sk < 1000;
       regr_avgx
   ------------------
    578.606576740795
   (1 row)

regr_avgy(Y, X)
---------------

Description: Average of the dependent variable (**sum(Y)/N**)

Return type: double precision

Examples:

::

   SELECT REGR_AVGY(sr_fee, sr_net_loss) FROM tpcds.store_returns WHERE sr_customer_sk < 1000;
       regr_avgy
   ------------------
    50.0136711629602
   (1 row)

regr_count(Y, X)
----------------

Description: Number of input rows in which both expressions are non-null

Return type: bigint

Examples:

::

   SELECT REGR_COUNT(sr_fee, sr_net_loss) FROM tpcds.store_returns WHERE sr_customer_sk < 1000;
    regr_count
   ------------
          2743
   (1 row)

regr_intercept(Y, X)
--------------------

Description: y-intercept of the least-squares-fit linear equation determined by the (X, Y) pairs

Return type: double precision

Examples:

::

   SELECT REGR_INTERCEPT(sr_fee, sr_net_loss) FROM tpcds.store_returns WHERE sr_customer_sk < 1000;
     regr_intercept
   ------------------
    49.2040847848607
   (1 row)

regr_r2(Y, X)
-------------

Description: Square of the correlation coefficient

Return type: double precision

Examples:

::

   SELECT REGR_R2(sr_fee, sr_net_loss) FROM tpcds.store_returns WHERE sr_customer_sk < 1000;
         regr_r2
   --------------------
    .00145453469345058
   (1 row)

regr_slope(Y, X)
----------------

Description: Slope of the least-squares-fit linear equation determined by the (X, Y) pairs

Return type: double precision

Examples:

::

   SELECT REGR_SLOPE(sr_fee, sr_net_loss) FROM tpcds.store_returns WHERE sr_customer_sk < 1000;
        regr_slope
   --------------------
    .00139920009665259
   (1 row)

regr_sxx(Y, X)
--------------

Description: **sum(X^2) - sum(X)^2/N** (sum of squares of the independent variables)

Return type: double precision

Examples:

::

   SELECT REGR_SXX(sr_fee, sr_net_loss) FROM tpcds.store_returns WHERE sr_customer_sk < 1000;
        regr_sxx
   ------------------
    1626645991.46135
   (1 row)

regr_sxy(Y, X)
--------------

Description: **sum(X*Y) - sum(X) \* sum(Y)/N** ("sum of products" of independent times dependent variable)

Return type: double precision

Examples:

::

   SELECT REGR_SXY(sr_fee, sr_net_loss) FROM tpcds.store_returns WHERE sr_customer_sk < 1000;
        regr_sxy
   ------------------
    2276003.22847225
   (1 row)

regr_syy(Y, X)
--------------

Description: **sum(Y^2) - sum(Y)^2/N** ("sum of squares" of the dependent variable)

Return type: double precision

Examples:

::

   SELECT REGR_SYY(sr_fee, sr_net_loss) FROM tpcds.store_returns WHERE sr_customer_sk < 1000;
       regr_syy
   -----------------
    2189417.6547314
   (1 row)

stddev(expression)
------------------

Description: Alias of **stddev_samp**

If the input parameter type is DOUBLE PRECISION, it accepts values from 1.34E-154 to 1.34E+154. Values outside this range cause the "value out of range: overflow" error. To avoid this, use the cast function to change the column type to numeric.

Return type: **double precision** for floating-point arguments, otherwise **numeric**

Examples:

::

   SELECT STDDEV(inv_quantity_on_hand) FROM tpcds.inventory WHERE inv_warehouse_sk = 1;
         stddev
   ------------------
    289.224359757315
   (1 row)

variance(expexpression,ression)
-------------------------------

Description: Alias of **var_samp**

If the input parameter type is DOUBLE PRECISION, it accepts values from 1.34E-154 to 1.34E+154. Values outside this range cause the "value out of range: overflow" error. To avoid this, use the cast function to change the column type to numeric.

Return type: **double precision** for floating-point arguments, otherwise **numeric**

Examples:

::

   SELECT VARIANCE(inv_quantity_on_hand) FROM tpcds.inventory WHERE inv_warehouse_sk = 1;
         variance
   --------------------
    83650.730277028768
   (1 row)

checksum(expression)
--------------------

Description: Returns the CHECKSUM value of all input values. This function can be used to check whether the data in the tables before and after GaussDB(DWS) data restoration or migration is the same. Other databases cannot be checked by using this function. Before and after database backup, database restoration, or data migration, you need to manually run SQL commands to obtain the execution results. Compare the obtained execution results to check whether the data in the tables before and after the backup or migration is the same.

.. note::

   -  For large tables, the CHECKSUM function may take a long time.
   -  If the CHECKSUM values of two tables are different, it indicates that the contents of the two tables are different. Using the hash function in the CHECKSUM function may incur conflicts. There is low possibility that two tables with different contents may have the same CHECKSUM value. The same problem may occur when CHECKSUM is used for columns.
   -  If the time type is timestamp, timestamptz, or smalldatetime, ensure that the time zone settings are the same when calculating the CHECKSUM value.

-  If the CHECKSUM value of a column is calculated and the column type can be changed to TEXT by default, set *expression* to the column name.
-  If the CHECKSUM value of a column is calculated and the column type cannot be changed to TEXT by default, set *expression* to *Column name*\ **::TEXT**.
-  If the CHECKSUM value of all columns is calculated, set *expression* to *Table name*\ **::TEXT**.

The following types of data can be converted into TEXT types by default: char, name, int8, int2, int1, int4, raw, pg_node_tree, float4, float8, bpchar, varchar, nvarchar2, date, timestamp, timestamptz, numeric, and smalldatetime. Other types need to be forcibly converted to TEXT.

Return type: numeric

Examples:

The following shows the CHECKSUM value of a column that can be converted to the TEXT type by default:

::

   SELECT CHECKSUM(inv_quantity_on_hand) FROM tpcds.inventory;
        checksum
   -------------------
    24417258945265247
   (1 row)

CHECKSUM value of a column that cannot be converted to the TEXT type by default (Note that the CHECKSUM parameter is *column_name*\ **::TEXT**):

::

   SELECT CHECKSUM(inv_quantity_on_hand::TEXT) FROM tpcds.inventory;
        checksum
   -------------------
    24417258945265247
   (1 row)

The following shows the CHECKSUM value of all columns in a table. Note that the CHECKSUM parameter is set to *Table name*\ **::TEXT**. The table name is not modified by its schema.

::

   SELECT CHECKSUM(inventory::TEXT) FROM tpcds.inventory;
        checksum
   -------------------
    25223696246875800
   (1 row)
