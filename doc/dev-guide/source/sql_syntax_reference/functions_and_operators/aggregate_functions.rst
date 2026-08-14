:original_name: dws_06_0046.html

.. _dws_06_0046:

Aggregate Functions
===================

Aggregate functions are commonly used in databases. They compute a single result from a set of input values, helping users quickly obtain key data overview from a large amount of data, such as the total number, average number, maximum value, and deduplicated count. Aggregate functions are widely used in scenarios such as data reports and service analysis.

.. table:: **Table 1** Aggregate functions

   +-------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   | Category                                  | Function                                                                                                                                                                       | Description                                                                                                                         |
   +===========================================+================================================================================================================================================================================+=====================================================================================================================================+
   | Statistics-based computing                | :ref:`sum(expression) <en-us_topic_0000001811634701__en-us_topic_0000001233708713_section395611461573>`                                                                        | Sum of the expression                                                                                                               |
   +-------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   |                                           | :ref:`max(expression) <en-us_topic_0000001811634701__en-us_topic_0000001233708713_section1863912531276>`                                                                       | Maximum value of the expression                                                                                                     |
   +-------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   |                                           | :ref:`min(expression) <en-us_topic_0000001811634701__en-us_topic_0000001233708713_section875863483>`                                                                           | Minimum value of the expression                                                                                                     |
   +-------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   |                                           | :ref:`avg(expression) <en-us_topic_0000001811634701__en-us_topic_0000001233708713_section1415518102819>`                                                                       | Average value of the expression                                                                                                     |
   +-------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   |                                           | :ref:`count(expression) <en-us_topic_0000001811634701__en-us_topic_0000001233708713_section89961834791>`                                                                       | Number of non-null (NOT NULL) rows in a table                                                                                       |
   +-------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   |                                           | :ref:`count(*) <en-us_topic_0000001811634701__en-us_topic_0000001233708713_section198975405920>`                                                                               | Total number of rows in a table (including rows containing NULL values)                                                             |
   +-------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   | Quantile and distribution analysis        | :ref:`median(expression) <en-us_topic_0000001811634701__en-us_topic_0000001233708713_section85441714818>`                                                                      | Median                                                                                                                              |
   +-------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   |                                           | :ref:`percentile_cont(const) within group(order by expression) <en-us_topic_0000001811634701__en-us_topic_0000001233708713_section9646174519811>`                              | Continuous distribution percentile. It is used to calculate the value of the specified percentile through linear interpolation.     |
   +-------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   |                                           | :ref:`percentile_disc(const) within group(order by expression) <en-us_topic_0000001811634701__en-us_topic_0000001233708713_section712919521811>`                               | Discrete percentile. It returns the actual value in the dataset that is closest to the specified percentile.                        |
   +-------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   | Aggregation and string concatenation      | :ref:`array_agg(expression) <en-us_topic_0000001811634701__en-us_topic_0000001233708713_section162711463918>`                                                                  | Aggregate multiple rows of values into an array.                                                                                    |
   +-------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   |                                           | :ref:`string_agg(expression, delimiter) <en-us_topic_0000001811634701__en-us_topic_0000001233708713_section739195413920>`                                                      | Concatenate multiple rows of strings into a single string using a specified delimiter.                                              |
   +-------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   |                                           | :ref:`listagg(expression [, delimiter]) WITHIN GROUP(ORDER BY order-list) <en-us_topic_0000001811634701__en-us_topic_0000001233708713_section192814041015>`                    | Concatenate multiple rows of strings into a single string in a specified order.                                                     |
   +-------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   |                                           | :ref:`group_concat(expression [ORDER BY {col_name | expr} [ASC | DESC]] [SEPARATOR str_val]) <en-us_topic_0000001811634701__en-us_topic_0000001233708713_section852151141016>` | Concatenate multiple rows of string values in a group into a single string.                                                         |
   +-------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   | Advanced statistical analysis             | :ref:`var_pop(expression) <en-us_topic_0000001811634701__en-us_topic_0000001233708713_section644814071020>`                                                                    | Population variance                                                                                                                 |
   +-------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   |                                           | :ref:`var_samp(expression) <en-us_topic_0000001811634701__en-us_topic_0000001233708713_section16208184518109>`                                                                 | Sample variance                                                                                                                     |
   +-------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   |                                           | :ref:`variance(expression) <en-us_topic_0000001811634701__en-us_topic_0000001233708713_section6879499125>`                                                                     | Sample variance                                                                                                                     |
   +-------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   |                                           | :ref:`stddev_pop(expression) <en-us_topic_0000001811634701__en-us_topic_0000001233708713_section17823929121018>`                                                               | Population standard deviation                                                                                                       |
   +-------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   |                                           | :ref:`stddev_samp(expression) <en-us_topic_0000001811634701__en-us_topic_0000001233708713_section165731134161015>`                                                             | Sample standard deviation                                                                                                           |
   +-------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   |                                           | :ref:`stddev(expression) <en-us_topic_0000001811634701__en-us_topic_0000001233708713_section1424812631213>`                                                                    | Sample standard deviation                                                                                                           |
   +-------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   |                                           | :ref:`covar_pop(Y, X) <en-us_topic_0000001811634701__en-us_topic_0000001233708713_section18864182301017>`                                                                      | Population covariance between Y and X                                                                                               |
   +-------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   |                                           | :ref:`covar_samp(Y, X) <en-us_topic_0000001811634701__en-us_topic_0000001233708713_section138921719171015>`                                                                    | Sample covariance between Y and X                                                                                                   |
   +-------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   |                                           | :ref:`corr(Y, X) <en-us_topic_0000001811634701__en-us_topic_0000001233708713_section9721596111>`                                                                               | Correlation coefficient between Y and X                                                                                             |
   +-------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   | Linear regression analysis                | :ref:`regr_avgx(Y, X) <en-us_topic_0000001811634701__en-us_topic_0000001233708713_section2072112371111>`                                                                       | Mean value of X                                                                                                                     |
   +-------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   |                                           | :ref:`regr_avgy(Y, X) <en-us_topic_0000001811634701__en-us_topic_0000001233708713_section8436192712118>`                                                                       | Average value of Y                                                                                                                  |
   +-------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   |                                           | :ref:`regr_count(Y, X) <en-us_topic_0000001811634701__en-us_topic_0000001233708713_section19619183731118>`                                                                     | Number of non-null pairs (X, Y)                                                                                                     |
   +-------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   |                                           | :ref:`regr_r2(Y, X) <en-us_topic_0000001811634701__en-us_topic_0000001233708713_section1655144617110>`                                                                         | Square of the correlation coefficient                                                                                               |
   +-------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   |                                           | :ref:`regr_slope(Y, X) <en-us_topic_0000001811634701__en-us_topic_0000001233708713_section2342175091111>`                                                                      | Slope of the least-squares-fit linear equation determined by the (X, Y) pairs                                                       |
   +-------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   |                                           | :ref:`regr_intercept(Y, X) <en-us_topic_0000001811634701__en-us_topic_0000001233708713_section1561720411113>`                                                                  | y-intercept of the least-squares-fit linear equation determined by the (X, Y) pairs                                                 |
   +-------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   |                                           | :ref:`regr_sxx(Y, X) <en-us_topic_0000001811634701__en-us_topic_0000001233708713_section794111546118>`                                                                         | Sum of squares of X                                                                                                                 |
   +-------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   |                                           | :ref:`regr_syy(Y, X) <en-us_topic_0000001811634701__en-us_topic_0000001233708713_section1346614219126>`                                                                        | Sum of squares of Y                                                                                                                 |
   +-------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   |                                           | :ref:`regr_sxy(Y, X) <en-us_topic_0000001811634701__en-us_topic_0000001233708713_section1381635841118>`                                                                        | Sum of the products of X and Y.                                                                                                     |
   +-------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   | Logical and bitwise operation aggregation | :ref:`bit_and(expression) <en-us_topic_0000001811634701__en-us_topic_0000001233708713_section13862164919101>`                                                                  | Bitwise AND operation on the values of all rows                                                                                     |
   +-------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   |                                           | :ref:`bit_or(expression) <en-us_topic_0000001811634701__en-us_topic_0000001233708713_section6680154171020>`                                                                    | Bitwise OR operation on the values of all rows                                                                                      |
   +-------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   |                                           | :ref:`bool_and(expression) <en-us_topic_0000001811634701__en-us_topic_0000001233708713_section12954135816107>`                                                                 | **TRUE** if all input values are true                                                                                               |
   +-------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   |                                           | :ref:`bool_or(expression) <en-us_topic_0000001811634701__en-us_topic_0000001233708713_section2932842114>`                                                                      | **TRUE** if any input value is true                                                                                                 |
   +-------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   |                                           | :ref:`every(expression) <en-us_topic_0000001811634701__en-us_topic_0000001233708713_section162441314121117>`                                                                   | Equivalent to **bool_and()**. **TRUE** if all input values are true.                                                                |
   +-------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   | Deduplication and verification            | :ref:`checksum(expression) <en-us_topic_0000001811634701__en-us_topic_0000001233708713_section21572013111220>`                                                                 | Checksum of data. It compares the results to determine whether the data in the tables before and after the operation is consistent. |
   +-------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   |                                           | :ref:`approx_count_distinct(col_name) <en-us_topic_0000001811634701__section193530231191>`                                                                                     | Approximate number of rows that contain a distinct value                                                                            |
   +-------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
   |                                           | :ref:`UNIQ(col_name) <en-us_topic_0000001811634701__section123261624219>`                                                                                                      | Number of rows that contain a distinct value                                                                                        |
   +-------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------+

.. _en-us_topic_0000001811634701__en-us_topic_0000001233708713_section395611461573:

sum(expression)
---------------

Description: Specifies the sum of **expression** across all input values.

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

.. _en-us_topic_0000001811634701__en-us_topic_0000001233708713_section1863912531276:

max(expression)
---------------

Description: Returns the maximum value of **expression** across all input values.

Argument types: any array, numeric, string, or date/time type

Return type: same as the argument type

Examples:

::

   SELECT MAX(inv_quantity_on_hand) FROM tpcds.inventory;
      max
   ---------
    1000000
   (1 row)

.. _en-us_topic_0000001811634701__en-us_topic_0000001233708713_section875863483:

min(expression)
---------------

Description: Returns the minimum value of **expression** across all input values.

Argument types: any array, numeric, string, or date/time type

Return type: same as the argument type

Examples:

::

   SELECT MIN(inv_quantity_on_hand) FROM tpcds.inventory;
    min
   -----
      0
   (1 row)

.. _en-us_topic_0000001811634701__en-us_topic_0000001233708713_section1415518102819:

avg(expression)
---------------

Description: Calculates the arithmetic average of all input values.

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

.. _en-us_topic_0000001811634701__en-us_topic_0000001233708713_section85441714818:

median(expression)
------------------

Description: Calculates the median of all input values. Currently, only the numeric and interval types are supported. Null values are not considered in the calculation.

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

.. _en-us_topic_0000001811634701__en-us_topic_0000001233708713_section9646174519811:

percentile_cont(const) within group(order by expression)
--------------------------------------------------------

Description: Returns a value corresponding to the specified percentile in the ordering, interpolating between adjacent input items if needed. Null values are not involved in the calculation.

Parameters:

-  **const**: percentile. The value ranges from **0** to **1**.
-  **expression**: column or expression used for sorting. Currently, only the numeric and interval types are supported.

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

.. _en-us_topic_0000001811634701__en-us_topic_0000001233708713_section712919521811:

percentile_disc(const) within group(order by expression)
--------------------------------------------------------

Description: returns the first input value whose position in the ordering equals or exceeds the specified percentile.

Parameters:

-  **const**: percentile. The value ranges from **0** to **1**.
-  **expression**: column or expression used for sorting. Currently, only the numeric and interval types are supported. Null values are not considered in the calculation.

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

.. _en-us_topic_0000001811634701__en-us_topic_0000001233708713_section89961834791:

count(expression)
-----------------

Description: Returns the number of input rows for which the value of expression is not null.

Return type: bigint

Examples:

::

   SELECT COUNT(inv_quantity_on_hand) FROM tpcds.inventory;
     count
   ----------
    11158087
   (1 row)

.. _en-us_topic_0000001811634701__en-us_topic_0000001233708713_section198975405920:

count(*)
--------

Description: Returns the total number of rows in a table (including rows containing NULL values).

Return type: bigint

Examples:

::

   SELECT COUNT(*) FROM tpcds.inventory;
     count
   ----------
    11745000
   (1 row)

.. _en-us_topic_0000001811634701__en-us_topic_0000001233708713_section162711463918:

array_agg(expression)
---------------------

Description: Input values, including nulls, concatenated into an array The input parameters of the function do not support the array type.

Return type: array of the argument type

Example:

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

.. _en-us_topic_0000001811634701__en-us_topic_0000001233708713_section739195413920:

string_agg(expression, delimiter)
---------------------------------

Description: Concatenates input values into a single string using a specified delimiter.

Return type: same as the argument type

Example:

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

.. _en-us_topic_0000001811634701__en-us_topic_0000001233708713_section192814041015:

listagg(expression [, delimiter]) WITHIN GROUP(ORDER BY order-list)
-------------------------------------------------------------------

Description: Orders the aggregated column data according to the sorting method specified by **WITHIN GROUP** and concatenates it into a string using the specified delimiter.

Parameters:

-  **expression**: Mandatory. It specifies an aggregation column name or a column-based, valid expression. It does not support the **DISTINCT** keyword and the **VARIADIC** parameter.
-  **delimiter**: Optional. It specifies a delimiter, which can be a string constant or a deterministic expression based on a group of columns. The default value is empty.
-  **order-list**: Mandatory. It specifies the sorting mode in a group.

Return type: text

.. note::

   **listagg** is a column-to-row aggregation function, compatible with Oracle Database 11g Release 2. You can specify the **OVER** clause as a window function. When **listagg** is used as a window function, the **OVER** clause does not support the window sorting or framework of **ORDER BY**, so as to avoid ambiguity in **listagg** and **ORDER BY** of the **WITHIN GROUP** clause.

Example:

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

The aggregation column is of the time interval type.

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

.. _en-us_topic_0000001811634701__en-us_topic_0000001233708713_section852151141016:

group_concat(expression [ORDER BY {col_name \| expr} [ASC \| DESC]] [SEPARATOR str_val])
----------------------------------------------------------------------------------------

Description: concatenates the specified **str_val** delimiters used by column data into a string. The concatenation uses a sorting method that must be specified by the **ORDER BY** clause. **ORDER BY 1** is not allowed.

Parameters:

-  **expression**: (mandatory) specifies a column name or a column-based valid expression. It does not support the **DISTINCT** keyword or the **VARIADIC** parameter.
-  **str_val**: (optional) specifies a delimiter, which can be a string constant or a deterministic expression based on grouped columns. The default value indicates that commas (,) are used as delimiters.

Return type: text

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

.. _en-us_topic_0000001811634701__en-us_topic_0000001233708713_section18864182301017:

covar_pop(Y, X)
---------------

Description: Calculates the population covariance.

Return type: double precision

Examples:

::

   SELECT COVAR_POP(sr_fee, sr_net_loss) FROM tpcds.store_returns WHERE sr_customer_sk < 1000;
       covar_pop
   ------------------
    829.749627587403
   (1 row)

.. _en-us_topic_0000001811634701__en-us_topic_0000001233708713_section138921719171015:

covar_samp(Y, X)
----------------

Description: Calculates the sample covariance.

Return type: double precision

Examples:

::

   SELECT COVAR_SAMP(sr_fee, sr_net_loss) FROM tpcds.store_returns WHERE sr_customer_sk < 1000;
       covar_samp
   ------------------
    830.052235037289
   (1 row)

.. _en-us_topic_0000001811634701__en-us_topic_0000001233708713_section17823929121018:

stddev_pop(expression)
----------------------

Description: Calculates the population standard deviation (square root of the population variance).

Return type: **double precision** for floating-point arguments, otherwise **numeric**

Examples:

::

   SELECT STDDEV_POP(inv_quantity_on_hand) FROM tpcds.inventory WHERE inv_warehouse_sk = 1;
       stddev_pop
   ------------------
    289.224294957556
   (1 row)

.. _en-us_topic_0000001811634701__en-us_topic_0000001233708713_section165731134161015:

stddev_samp(expression)
-----------------------

Description: Calculates the sample standard deviation (square root of the sample variance).

Return type: **double precision** for floating-point arguments, otherwise **numeric**

Examples:

::

   SELECT STDDEV_SAMP(inv_quantity_on_hand) FROM tpcds.inventory WHERE inv_warehouse_sk = 1;
      stddev_samp
   ------------------
    289.224359757315
   (1 row)

.. _en-us_topic_0000001811634701__en-us_topic_0000001233708713_section644814071020:

var_pop(expression)
-------------------

Description: Calculates the population variance of the input values (square of the population standard deviation)

Return type: **double precision** for floating-point arguments, otherwise **numeric**

Examples:

::

   SELECT VAR_POP(inv_quantity_on_hand) FROM tpcds.inventory WHERE inv_warehouse_sk = 1;
         var_pop
   --------------------
    83650.692793695475
   (1 row)

.. _en-us_topic_0000001811634701__en-us_topic_0000001233708713_section16208184518109:

var_samp(expression)
--------------------

Description: Calculates the sample variance of the input values (square of the sample standard deviation)

Return type: **double precision** for floating-point arguments, otherwise **numeric**

Examples:

::

   SELECT VAR_SAMP(inv_quantity_on_hand) FROM tpcds.inventory WHERE inv_warehouse_sk = 1;
         var_samp
   --------------------
    83650.730277028768
   (1 row)

.. _en-us_topic_0000001811634701__en-us_topic_0000001233708713_section13862164919101:

bit_and(expression)
-------------------

Description: Performs a bitwise AND operation on all non-null input values. If all input values are NULL, the result is also NULL.

Return type: same as the argument type

Examples:

::

   SELECT BIT_AND(inv_quantity_on_hand) FROM tpcds.inventory WHERE inv_warehouse_sk = 1;
    bit_and
   ---------
          0
   (1 row)

.. _en-us_topic_0000001811634701__en-us_topic_0000001233708713_section6680154171020:

bit_or(expression)
------------------

Description: Performs a bitwise OR operation on all non-null input values. If all input values are NULL, the result is also NULL.

Return type: same as the argument type

Examples:

::

   SELECT BIT_OR(inv_quantity_on_hand) FROM tpcds.inventory WHERE inv_warehouse_sk = 1;
    bit_or
   --------
      1023
   (1 row)

.. _en-us_topic_0000001811634701__en-us_topic_0000001233708713_section12954135816107:

bool_and(expression)
--------------------

Description: Returns **TRUE** if all input values are true. Otherwise, **FALSE** is returned.

Return type: Boolean

Examples:

::

   SELECT bool_and(100 <2500);
    bool_and
   ----------
    t
   (1 row)

.. _en-us_topic_0000001811634701__en-us_topic_0000001233708713_section2932842114:

bool_or(expression)
-------------------

Description: Returns **TRUE** if any input value is true. Otherwise, **FALSE** is returned.

Return type: Boolean

Examples:

::

   SELECT bool_or(100 <2500);
    bool_or
   ----------
    t
   (1 row)

.. _en-us_topic_0000001811634701__en-us_topic_0000001233708713_section9721596111:

corr(Y, X)
----------

Description: Calculates the correlation coefficient (covariance divided by the product of the standard deviations of two variables).

Return type: double precision

Examples:

::

   SELECT CORR(sr_fee, sr_net_loss) FROM tpcds.store_returns WHERE sr_customer_sk < 1000;
          corr
   -------------------
    0.0381383624904186
   (1 row)

.. _en-us_topic_0000001811634701__en-us_topic_0000001233708713_section162441314121117:

every(expression)
-----------------

Description: Is equivalent to :ref:`bool_and(expression) <en-us_topic_0000001811634701__en-us_topic_0000001233708713_section12954135816107>` Description: Returns **TRUE** if all input values are true. Otherwise, **FALSE** is returned.

Return type: Boolean

Examples:

::

   SELECT every(100 <2500);
    every
   -------
    t
   (1 row)

.. _en-us_topic_0000001811634701__en-us_topic_0000001233708713_section2072112371111:

regr_avgx(Y, X)
---------------

Description: Calculates the average of the independent variable (**sum(X)/N**).

Return type: double precision

Examples:

::

   SELECT REGR_AVGX(sr_fee, sr_net_loss) FROM tpcds.store_returns WHERE sr_customer_sk < 1000;
       regr_avgx
   ------------------
    578.606576740795
   (1 row)

.. _en-us_topic_0000001811634701__en-us_topic_0000001233708713_section8436192712118:

regr_avgy(Y, X)
---------------

Description: Calculates the average of the dependent variable (**sum(Y)/N**).

Return type: double precision

Examples:

::

   SELECT REGR_AVGY(sr_fee, sr_net_loss) FROM tpcds.store_returns WHERE sr_customer_sk < 1000;
       regr_avgy
   ------------------
    50.0136711629602
   (1 row)

.. _en-us_topic_0000001811634701__en-us_topic_0000001233708713_section19619183731118:

regr_count(Y, X)
----------------

Description: Calculates the number of input rows in which both expressions are non-null.

Return type: bigint

Examples:

::

   SELECT REGR_COUNT(sr_fee, sr_net_loss) FROM tpcds.store_returns WHERE sr_customer_sk < 1000;
    regr_count
   ------------
          2743
   (1 row)

.. _en-us_topic_0000001811634701__en-us_topic_0000001233708713_section1561720411113:

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

.. _en-us_topic_0000001811634701__en-us_topic_0000001233708713_section1655144617110:

regr_r2(Y, X)
-------------

Description: Calculates the square of the correlation coefficient.

Return type: double precision

Examples:

::

   SELECT REGR_R2(sr_fee, sr_net_loss) FROM tpcds.store_returns WHERE sr_customer_sk < 1000;
         regr_r2
   --------------------
    0.00145453469345058
   (1 row)

.. _en-us_topic_0000001811634701__en-us_topic_0000001233708713_section2342175091111:

regr_slope(Y, X)
----------------

Description: Slope of the least-squares-fit linear equation determined by the (X, Y) pairs

Return type: double precision

Examples:

::

   SELECT REGR_SLOPE(sr_fee, sr_net_loss) FROM tpcds.store_returns WHERE sr_customer_sk < 1000;
        regr_slope
   --------------------
    0.00139920009665259
   (1 row)

.. _en-us_topic_0000001811634701__en-us_topic_0000001233708713_section794111546118:

regr_sxx(Y, X)
--------------

Description: Calculates **sum(X^2)** - **sum(X)^2/N** (sum of the squares of the independent variable **X**).

Return type: double precision

Examples:

::

   SELECT REGR_SXX(sr_fee, sr_net_loss) FROM tpcds.store_returns WHERE sr_customer_sk < 1000;
        regr_sxx
   ------------------
    1626645991.46135
   (1 row)

.. _en-us_topic_0000001811634701__en-us_topic_0000001233708713_section1381635841118:

regr_sxy(Y, X)
--------------

Description: Calculates **sum(X*Y)** - **sum(X) \* sum(Y)/N** (sum of the products of independent variable **X** and dependent variable **Y**).

Return type: double precision

Examples:

::

   SELECT REGR_SXY(sr_fee, sr_net_loss) FROM tpcds.store_returns WHERE sr_customer_sk < 1000;
        regr_sxy
   ------------------
    2276003.22847225
   (1 row)

.. _en-us_topic_0000001811634701__en-us_topic_0000001233708713_section1346614219126:

regr_syy(Y, X)
--------------

Description: Calculates **sum(Y^2) - sum(Y)^2/N** (sum of squares of the dependent variable)

Return type: double precision

Examples:

::

   SELECT REGR_SYY(sr_fee, sr_net_loss) FROM tpcds.store_returns WHERE sr_customer_sk < 1000;
       regr_syy
   -----------------
    2189417.6547314
   (1 row)

.. _en-us_topic_0000001811634701__en-us_topic_0000001233708713_section1424812631213:

stddev(expression)
------------------

Description: Specifies the alias of :ref:`stddev_samp(expression) <en-us_topic_0000001811634701__en-us_topic_0000001233708713_section165731134161015>` and calculates the sample standard deviation.

Return type: **double precision** for floating-point arguments, otherwise **numeric**

Examples:

::

   SELECT STDDEV(inv_quantity_on_hand) FROM tpcds.inventory WHERE inv_warehouse_sk = 1;
         stddev
   ------------------
    289.224359757315
   (1 row)

.. _en-us_topic_0000001811634701__en-us_topic_0000001233708713_section6879499125:

variance(expression)
--------------------

Description: Specifies the alias of :ref:`var_samp(expression) <en-us_topic_0000001811634701__en-us_topic_0000001233708713_section16208184518109>` and calculates the sample variance.

Return type: **double precision** for floating-point arguments, otherwise **numeric**

Examples:

::

   SELECT VARIANCE(inv_quantity_on_hand) FROM tpcds.inventory WHERE inv_warehouse_sk = 1;
         variance
   --------------------
    83650.730277028768
   (1 row)

.. _en-us_topic_0000001811634701__en-us_topic_0000001233708713_section21572013111220:

checksum(expression)
--------------------

Description: Returns the CHECKSUM value of all input values. This function can be used to check whether the data in the tables is the same before and after the backup, restoration, or migration of the DWS database. Before and after the backup and restoration or data migration, you need to manually execute a SQL command to obtain the execution result. By comparing the obtained execution results, it can be determined whether the data in the table is consistent before and after the operation.

.. note::

   -  For a table with a large amount of data, it may take a long time to execute the **CHECKSUM()** function.
   -  If the CHECKSUM values of two tables are different, it indicates that the contents of the two tables are different. Using the hash function in the CHECKSUM function may incur conflicts. There is low possibility that two tables with different contents may have the same CHECKSUM value. The same problem may occur when CHECKSUM is used for columns.
   -  If the time type is timestamp, timestamptz, or smalldatetime, ensure that the time zone settings are the same when calculating the CHECKSUM value.

-  If the CHECKSUM value of a column is calculated and the column type can be changed to TEXT by default, set *expression* to the column name.
-  If the CHECKSUM value of a column is calculated and the column type cannot be changed to TEXT by default, set *expression* to *Column name*\ **::TEXT**.
-  If the CHECKSUM value of all columns is calculated, set *expression* to *Table name*\ **::TEXT**.

The following data types can be converted into TEXT by default: char, name, int8, int2, int1, int4, raw, pg_node_tree, float4, float8, bpchar, varchar, nvarchar, nvarchar2, date, timestamp, timestamptz, numeric, and smalldatetime. Other types need to be forcibly converted into TEXT.

Return type: numeric

Example:

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

.. _en-us_topic_0000001811634701__section193530231191:

approx_count_distinct(col_name)
-------------------------------

Description: Estimates the number of rows in a column after deduplication (cardinality) using the HyperLogLog++ (HLL++) algorithm. This function is supported only in clusters of version 8.3.0 or later.

Parameter description: **col_name** indicates the column whose cardinality needs to be estimated.

.. note::

   You can adjust the error rate by setting the **approx_count_distinct_precision** parameter.

   -  The value range is [10,20]. The default value is 17. The theoretical error rate is 3‰.

   -  This parameter indicates the number of buckets in the HyperLogLog++ (HLL++) algorithm. A larger value indicates a larger number of buckets and a smaller theoretical error rate.
   -  A larger value of this parameter results in more computing time and memory resource overhead, but is still far less than the overhead of the **count distinct** statement. You are advised to use this function to replace the **COUNT DISTINCT** statement when the estimation cardinality is large.

Example:

::

   CREATE TABLE employeeinfo (empno smallint, ename varchar(20), job varchar(20), hiredate date,deptno smallint) WITH (ORIENTATION = COLUMN);
   INSERT INTO employeeinfo VALUES (7155, 'JACK', 'SALESMAN', '2018-12-01', 30);
   INSERT INTO employeeinfo VALUES (7003, 'TOM', 'FINANCE', '2016-06-15', 20);
   INSERT INTO employeeinfo VALUES (7357, 'MAX', 'SALESMAN', '2020-10-01', 30);

   SELECT APPROX_COUNT_DISTINCT(empno) from employeeinfo;
    approx_count_distinct
   -----------------------
                        3
   (1 row)

   SELECT COUNT(DISTINCT empno) FROM employeeinfo GROUP BY ename;
    count
   -------
        1
        1
        1
   (3 rows)

.. _en-us_topic_0000001811634701__section123261624219:

UNIQ(col_name)
--------------

Description: Calculates the number of rows after deduplication in a column and returns a deduplicated value. Its function is similar to that of **COUNT DISTINCT**. This function is supported only in clusters of version 8.3.0 or later.

Parameter description: **col_name** indicates the column for which the number of rows after deduplication needs to be calculated. The value can be SMALLINT, INTEGER, BIGINT, REAL, DOUBLE PRECISION, TEXT, VARCHAR, TIMESTAMP, TIMESTAMPTZ, DATE, TIMETZ or UUID.

.. note::

   -  Currently, only DWS column-store tables support the **UNIQ()** function.
   -  When the **UNIQ()** function is used, the SQL statement must contain **GROUP BY**. To achieve better performance, the GROUP BY fields must be evenly distributed.
   -  To deduplicate three or more columns in an SQL statement, you can use the **UNIQ()** function, which is more efficient than the **COUNT DISTINCT** function.
   -  Generally, the memory usage of the **UNIQ()** function is lower than that of the **COUNT DISTINCT** function. If the memory usage exceeds the threshold when the **COUNT DISTINCT** function is used, you can use the **UNIQ()** function.

Examples:

::

   CREATE TABLE employeeinfo (empno smallint, ename varchar(20), job varchar(20), hiredate date,deptno smallint) WITH (ORIENTATION = COLUMN);
   INSERT INTO employeeinfo VALUES (7155, 'JACK', 'SALESMAN', '2018-12-01', 30);
   INSERT INTO employeeinfo VALUES (7003, 'TOM', 'FINANCE', '2016-06-15', 20);
   INSERT INTO employeeinfo VALUES (7357, 'MAX', 'SALESMAN', '2020-10-01', 30);

   SELECT UNIQ(deptno) FROM employeeinfo GROUP BY ename;
    uniq
   ------
       1
       1
       1
   (3 rows)
