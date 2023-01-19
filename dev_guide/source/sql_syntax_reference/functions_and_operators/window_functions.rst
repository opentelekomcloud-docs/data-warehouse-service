:original_name: dws_06_0047.html

.. _dws_06_0047:

Window Functions
================

Regular aggregate functions return a single value calculated from values in a row, or group all rows into a single output row. Window functions perform a calculation across a set of rows and return a value for each row.

-  A window function call represents the application of an aggregate-like function over some portion of the rows selected by a query. Therefore, aggregate functions (:ref:`Aggregate Functions <dws_06_0046>`) can also be used as window functions. In addition, window functions are able to scan all the rows and divide the query rows into a partition by using the **PARTITION BY** clause.
-  Column-store tables support only the window functions **rank (expression)** and **row_number (expression)** and the aggregate functions **sum**, **count**, **avg**, **min**, and **max**. Row-store tables do not have such restrictions.
-  Invoking a window function requires special syntax using the **OVER** clause to specify a window. The **OVER** clause is used for grouping data and sorting the elements in a group. Window functions are used for generating sequence numbers for the values in the group.
-  **order by** in a window function must be followed by a column name. If it is followed by a number, the number is processed as a constant value and the target column is not ranked.

Syntax of a Window Function
---------------------------

::

   function_name ([expression [, expression ... ]]) OVER ( window_definition ) function_name ([expression [, expression ... ]]) OVER window_namefunction_name ( * ) OVER ( window_definition ) function_name ( * ) OVER window_name

**window_definition** is defined as follows:

::

   [ existing_window_name ] [ PARTITION BY expression [, ...] ] [ ORDER BY expression [ ASC | DESC | USING operator ] [ NULLS { FIRST | LAST } ] [, ...] ] [ frame_clause ]

**frame_clause** is defined as follows:

::

   [ RANGE | ROWS ] frame_start [ RANGE | ROWS ] BETWEEN frame_start AND frame_end

You can use **RANGE** and **ROWS** to specify the window frame. **ROWS** specifies the window in physical units (rows). **RANGE** specifies the window as a logical offset.

In **RANGE** and **ROWS**, you can use **BETWEEN** *frame_start* **AND** *frame_end* to specify the window's first and last rows. If *frame_end* is left blank, it defaults to **CURRENT ROW**.

The value options of **BETWEEN frame_start AND frame_end** are as follows:

-  **CURRENT ROW**: The current row is used as the window frame's start or end point.
-  *N* **PRECEDING**: The window frame starts from the *n*\ th row to the current row.
-  **UNBOUNDED PRECEDING**: The window frame starts at the first row of the partition.
-  *N* **FOLLOWING**: The window frame starts from the current row to the *n*\ th row.
-  **UNBOUNDED FOLLOWING**: The window frame ends with the last row of the partition.

*frame_start* cannot be **UNBOUNDED FOLLOWING**, *frame_end* cannot be **UNBOUNDED PRECEDING**, and *frame_end* cannot be earlier than *frame_start*. For example, **RANGE BETWEEN CURRENT ROW AND** *value* **PRECEDING** is not allowed.


Window Functions
----------------

-  RANK()

   Description: The **RANK** function is used for generating non-consecutive sequence numbers for the values in each group. The same values have the same sequence number.

   Return type: bigint

   For example:

   ::

      SELECT d_mon, d_week_seq, rank() OVER(PARTITION BY d_mon ORDER BY d_week_seq) FROM reason_date WHERE d_mon < 4 AND d_week_seq < 7 ORDER BY 1,2;
       d_mon | d_week_seq | rank
      -------+------------+------
           1 |          1 |    1
           1 |          1 |    1
           1 |          2 |    3
           1 |          2 |    3
           2 |          3 |    1
           2 |          3 |    1
      (6 rows)

-  ROW_NUMBER()

   Description: The **ROW_NUMBER** function is used for generating consecutive sequence numbers for the values in each group. The same values have different sequence numbers.

   Return type: bigint

   For example:

   ::

      SELECT d_mon, d_week_seq, Row_number() OVER(PARTITION BY d_mon ORDER BY d_week_seq) FROM reason_date WHERE d_mon < 4 AND d_week_seq < 7 ORDER BY 1,2;
       d_mon | d_week_seq | row_number
      -------+------------+------------
           1 |          1 |          1
           1 |          1 |          2
           1 |          2 |          3
           1 |          2 |          4
           2 |          3 |          1
           2 |          3 |          2
      (6 rows)

-  DENSE_RANK()

   Description: The **DENSE_RANK** function is used for generating consecutive sequence numbers for the values in each group. The same values have the same sequence number.

   Return type: bigint

   For example:

   ::

      SELECT d_mon, d_week_seq, dense_rank() OVER(PARTITION BY d_mon ORDER BY d_week_seq) FROM reason_date WHERE d_mon < 4 AND d_week_seq < 7 ORDER BY 1,2;
       d_mon | d_week_seq | dense_rank
      -------+------------+------------
           1 |          1 |          1
           1 |          1 |          1
           1 |          2 |          2
           1 |          2 |          2
           2 |          3 |          1
           2 |          3 |          1
      (6 rows)

-  PERCENT_RANK()

   Description: The **PERCENT_RANK** function is used for generating corresponding sequence numbers for the values in each group. That is, the function calculates the value according to the formula Sequence number = (**Rank** - 1)/(**Total rows** - 1). **Rank** is the corresponding sequence number generated based on the **RANK** function for the value and **Total rows** is the total number of elements in a group.

   Return type: double precision

   For example:

   ::

      SELECT d_mon, d_week_seq, percent_rank() OVER(PARTITION BY d_mon ORDER BY d_week_seq) FROM reason_date WHERE d_mon < 4 AND d_week_seq < 7 ORDER BY 1,2;
       d_mon | d_week_seq |   percent_rank
      -------+------------+------------------
           1 |          1 |                0
           1 |          1 |                0
           1 |          2 | .666666666666667
           1 |          2 | .666666666666667
           2 |          3 |                0
           2 |          3 |                0
      (6 rows)

-  CUME_DIST()

   Description: The **CUME_DIST** function is used for generating accumulative distribution sequence numbers for the values in each group. That is, the function calculates the value according to the following formula: Sequence number = Number of rows preceding or peer with current row/Total rows.

   Return type: double precision

   For example:

   ::

      SELECT d_mon, d_week_seq, cume_dist() OVER(PARTITION BY d_mon ORDER BY d_week_seq) FROM reason_date e_dim WHERE d_mon < 4 AND d_week_seq < 7 ORDER BY 1,2;
       d_mon | d_week_seq | cume_dist
      -------+------------+-----------
           1 |          1 |        .5
           1 |          1 |        .5
           1 |          2 |         1
           1 |          2 |         1
           2 |          3 |         1
           2 |          3 |         1
      (6 rows)

-  NTILE(num_buckets integer)

   Description: The **NTILE** function is used for equally allocating sequential data sets to the buckets whose quantity is specified by **num_buckets** according to **num_buckets integer** and allocating the bucket number to each row. Divide the partition as equally as possible.

   Return type: integer

   For example:

   ::

      SELECT d_mon, d_week_seq, ntile(3) OVER(PARTITION BY d_mon ORDER BY d_week_seq) FROM reason_date WHERE d_mon < 4 AND d_week_seq < 7 ORDER BY 1,2;
       d_mon | d_week_seq | ntile
      -------+------------+-------
           1 |          1 |     1
           1 |          1 |     1
           1 |          2 |     2
           1 |          2 |     3
           2 |          3 |     1
           2 |          3 |     2
      (6 rows)

-  LAG(value any [, offset integer [, default any ]])

   Description: The **LAG** function is used for generating lag values for the corresponding values in each group. That is, the value of the row obtained by moving forward the row corresponding to the current value by **offset** (integer) is the sequence number. If the row does not exist after the moving, the result value is the default value. If omitted, **offset** defaults to **1** and **default** to **null**.

   Return type: same as the parameter type

   For example:

   ::

      SELECT d_mon, d_week_seq, lag(d_mon,3,null) OVER(PARTITION BY d_mon ORDER BY d_week_seq) FROM reason_date WHERE d_mon < 4 AND d_week_seq < 7 ORDER BY 1,2;
       d_mon | d_week_seq | lag
      -------+------------+-----
           1 |          1 |
           1 |          1 |
           1 |          2 |
           1 |          2 |   1
           2 |          3 |
           2 |          3 |
      (6 rows)

-  LEAD(value any [, offset integer [, default any ]])

   Description: The **LEAD** function is used for generating leading values for the corresponding values in each group. That is, the value of the row obtained by moving backward the row corresponding to the current value by **offset** (integer) is the sequence number. If the number of rows after the moving exceeds the total number for the current group, the result value is the default value. If omitted, **offset** defaults to **1** and **default** to **null**.

   Return type: same as the parameter type

   For example:

   ::

      SELECT d_mon, d_week_seq, lead(d_week_seq,2) OVER(PARTITION BY d_mon ORDER BY d_week_seq) FROM  reason_date WHERE d_mon < 4 AND d_week_seq < 7 ORDER BY 1,2;
       d_mon | d_week_seq | lead
      -------+------------+------
           1 |          1 |    2
           1 |          1 |    2
           1 |          2 |
           1 |          2 |
           2 |          3 |
           2 |          3 |
      (6 rows)

-  FIRST_VALUE(value any)

   Description: The **FIRST_VALUE** function is used for returning the first value of each group.

   Return type: same as the parameter type

   For example:

   ::

      SELECT d_mon, d_week_seq, first_value(d_week_seq) OVER(PARTITION BY d_mon ORDER BY d_week_seq) FROM reason_date WHERE d_mon < 4 AND d_week_seq < 7 ORDER BY 1,2;
       d_mon | d_week_seq | first_value
      -------+------------+-------------
           1 |          1 |           1
           1 |          1 |           1
           1 |          2 |           1
           1 |          2 |           1
           2 |          3 |           3
           2 |          3 |           3
      (6 rows)

-  LAST_VALUE(value any)

   Description: Returns the last value of each group.

   Return type: same as the parameter type

   For example:

   ::

      SELECT d_mon, d_week_seq, last_value(d_mon) OVER(PARTITION BY d_mon ORDER BY d_week_seq) FROM reason_date WHERE d_mon < 4 AND d_week_seq < 6 ORDER BY 1,2;
       d_mon | d_week_seq | last_value
      -------+------------+------------
           1 |          1 |          1
           1 |          1 |          1
           1 |          2 |          1
           1 |          2 |          1
           2 |          3 |          2
           2 |          3 |          2
      (6 rows)

-  NTH_VALUE(value any, nth integer)

   Description: The *n*\ th row for a group is the returned value. If the row does not exist, **NULL** is returned by default.

   Return type: same as the parameter type

   For example:

   ::

      SELECT d_mon, d_week_seq, nth_value(d_week_seq,2) OVER(PARTITION BY d_mon ORDER BY d_week_seq) FROM reason_date WHERE d_mon < 4 AND d_week_seq < 6 ORDER BY 1,2;
       d_mon | d_week_seq | nth_value
      -------+------------+-----------
           1 |          1 |         1
           1 |          1 |         1
           1 |          2 |         1
           1 |          2 |         1
           2 |          3 |         3
           2 |          3 |         3
      (6 rows)
