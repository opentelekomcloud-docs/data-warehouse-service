:original_name: dws_06_0047.html

.. _dws_06_0047:

Window Functions
================

Regular aggregate functions return a single value calculated from values in a row, or group all rows into a single output row. Window functions perform a calculation across a set of rows and return a value for each row.

A window function call represents the application of an aggregate-like function over some portion of the rows selected by a query. Therefore, aggregate functions (:ref:`Aggregate Functions <dws_06_0046>`) can also be used as window functions. A window function can scan all rows and display the raw data and aggregation analysis results at the same time.

Precautions
-----------

-  Column-store tables support only the window functions **rank (expression)** and **row_number (expression)** and the aggregate functions **sum**, **count**, **avg**, **min**, and **max**. Row-store tables do not have such restrictions.

-  A single query can contain one or more window function expressions.

-  Window functions can appear only in output columns. If you want to use the values of a window function for condition filtering, you need to nest the window function in the subquery and use the aliases of the window function expression at the outer layer for condition filtering. Example:

   .. code-block::

      SELECT classid, id, score FROM(SELECT *, avg(score) OVER(PARTITION BY classid) as avg_score FROM score) WHERE score >= avg_score;

-  In the query block where the window function is located, the **GROUP BY** expression can be used for grouping and deduplication. In this case, the PARTITION BY clause in the window function must be a subset of the GROUP BY expression to ensure that the window function performs calculation on the deduplication result. The expression of the **ORDER BY** clause must be a subset of the **GROUP BY** expression or an aggregate function of an aggregate operation. Example:

   .. code-block::

      SELECT classid,rank() OVER(PARTITION BY classid ORDER BY sum(score)) as avg_score FROM score GROUP BY classid, id;

Syntax
------

A window function uses the **OVER** clause to define a window. The **OVER** clause is used for grouping data and sorting the elements in a group. Window functions are used for generating sequence numbers for the values in the group.

::

   function_name ([expression [, expression ... ]]) OVER ( window_definition )
   function_name ([expression [, expression ... ]]) OVER window_name
   function_name ( * ) OVER ( window_definition )
   function_name ( * ) OVER window_name

**window_definition** is defined as follows:

::

   [ existing_window_name ]
   [ PARTITION BY expression [, ...] ]
   [ ORDER BY expression [ ASC | DESC | USING operator ] [ NULLS { FIRST | LAST } ] [, ...] ]
   [ frame_clause ]

The **PARTITION BY** option specifies that rows with the same **PARTITION BY** expression value are grouped.

The **ORDER BY** option is used to control the order in which the window function processes rows. **ORDER BY** must be followed by a column name. If **ORDER BY** is followed by a number, the number is processed as a constant and does not sort the target column.

**frame_clause** is defined as follows:

::

   [ RANGE | ROWS ] frame_start
   [ RANGE | ROWS ] BETWEEN frame_start AND frame_end

When you need to specify a window to calculate the results of all rows in a group, you need to specify the start row and end row of the window range. The window range supports the RANGE and ROWS modes. The ROWS mode specifies the window by the physical unit (row), and the RANGE mode specifies the window as the logical offset.

In **RANGE** and **ROWS**, you can use **BETWEEN** *frame_start* **AND** *frame_end* to specify the window's first and last rows. If only **frame_start** is specified, **frame_end** is **CURRENT ROW** by default.

The values of **frame_start** and **frame_end** are as follows:

-  **CURRENT ROW**: The current row is used as the window frame's start or end point.
-  *N* **PRECEDING**: The window frame starts from the *n*\ th row to the current row.
-  **UNBOUNDED PRECEDING**: The window frame starts at the first row of the partition.
-  *N* **FOLLOWING**: The window frame starts from the current row to the *n*\ th row.
-  **UNBOUNDED FOLLOWING**: The window frame ends with the last row of the partition.

*frame_start* cannot be **UNBOUNDED FOLLOWING**, *frame_end* cannot be **UNBOUNDED PRECEDING**, and *frame_end* cannot be earlier than *frame_start*. For example, **RANGE BETWEEN CURRENT ROW AND** *N* **PRECEDING** is not allowed.

For clusters of version 8.2.1.220 or later, the LAST_VALUE function supports the IGNORE NULLS syntax. This syntax returns the last value in a non-null window. If all values are **NULL**, **NULL** is returned. The format is as follows:

::

   LAST_VALUE (expression [IGNORE NULLS]) OVER (window_definition)

Currently, only **ROWS between CURRENT ROW and UNBOUNDED FOLLOWING** and **ROWS BETWEEN UNBOUNDED PRECEDING and CURRENT ROW** are supported.

RANK()
------

Description: The **RANK** function is used for generating non-consecutive sequence numbers for the values in each group. The same values have the same rank value but with sequence numbers.

Return type: bigint

Example:

In the **score(id, classid, score)** table, the rows are student ID, class ID, and exam score.

Use the **RANK** function to sort student scores.

::

   CREATE TABLE score(id int,classid int,score int);
   INSERT INTO score VALUES(1,1,95),(2,2,95),(3,2,85),(4,1,70),(5,2,88),(6,1,70);

   SELECT id, classid, score,RANK() OVER(ORDER BY score DESC) FROM score;
    id | classid | score | rank
   ----+---------+-------+------
     1 |       1 |    95 |    1
     2 |       2 |    95 |    1
     6 |       1 |    70 |    5
     4 |       1 |    70 |    5
     5 |       2 |    88 |    3
     3 |       2 |    85 |    4
   (6 rows)

ROW_NUMBER()
------------

Description: The **ROW_NUMBER** function is used for generating consecutive sequence numbers for the values in each group. The same values have different sequence numbers.

Return type: bigint

Example:

::

   SELECT id, classid, score,ROW_NUMBER() OVER(ORDER BY score DESC) FROM score ORDER BY score DESC;
    id | classid | score | row_number
   ----+---------+-------+------------
     1 |       1 |    95 |          1
     2 |       2 |    95 |          2
     5 |       2 |    88 |          3
     3 |       2 |    85 |          4
     6 |       1 |    70 |          5
     4 |       1 |    70 |          6
   (6 rows)

DENSE_RANK()
------------

Description: The **DENSE_RANK** function is used for generating consecutive sequence numbers for the values in each group. The same values have the same rank value number and the same sequence number.

Return type: bigint

Example:

::

   SELECT id, classid, score,DENSE_RANK() OVER(ORDER BY score DESC) FROM score;
    id | classid | score | dense_rank
   ----+---------+-------+------------
     1 |       1 |    95 |          1
     2 |       2 |    95 |          1
     5 |       2 |    88 |          2
     3 |       2 |    85 |          3
     6 |       1 |    70 |          4
     4 |       1 |    70 |          4
   (6 rows)

PERCENT_RANK()
--------------

Description: The **PERCENT_RANK** function is used for generating corresponding sequence numbers for the values in each group. That is, the function calculates the value according to the formula Sequence number = (**Rank** - 1)/(**Total rows** - 1). **Rank** is the corresponding sequence number generated based on the **RANK** function for the value and **Total rows** is the total number of elements in a group.

Return type: double precision

Example:

::

   SELECT id, classid, score,PERCENT_RANK() OVER(ORDER BY score DESC) FROM score;
    id | classid | score | percent_rank
   ----+---------+-------+--------------
     1 |       1 |    95 |            0
     2 |       2 |    95 |            0
     3 |       2 |    85 |           .6
     4 |       1 |    70 |           .8
     5 |       2 |    88 |           .4
     6 |       1 |    70 |           .8
   (6 rows)

CUME_DIST()
-----------

Description: The **CUME_DIST** function is used for generating accumulative distribution sequence numbers for the values in each group. That is, the function calculates the value according to the following formula: Sequence number = Number of rows preceding or peer with current row/Total rows.

Return type: double precision

Example:

::

   SELECT id,classid,score,CUME_DIST() OVER(ORDER BY score DESC) FROM score;
    id | classid | score |    cume_dist
   ----+---------+-------+------------------
     1 |       1 |    95 | .333333333333333
     2 |       2 |    95 | .333333333333333
     5 |       2 |    88 |               .5
     3 |       2 |    85 | .666666666666667
     4 |       1 |    70 |                1
     6 |       1 |    70 |                1
   (6 rows)

NTILE(num_buckets integer)
--------------------------

Description: The **NTILE** function is used for equally allocating sequential data sets to the buckets whose quantity is specified by **num_buckets** according to **num_buckets integer** and allocating the bucket number to each row. Divide the partition as equally as possible.

Return type: integer

Example:

::

   SELECT id,classid,score,NTILE(3) OVER(ORDER BY score DESC) FROM score;
    id | classid | score | ntile
   ----+---------+-------+-------
     1 |       1 |    95 |     1
     2 |       2 |    95 |     1
     5 |       2 |    88 |     2
     3 |       2 |    85 |     2
     4 |       1 |    70 |     3
     6 |       1 |    70 |     3
   (6 rows)

LAG(value any [, offset integer [, default any ]])
--------------------------------------------------

Description: The **LAG** function is used for generating lag values for the corresponding values in each group. That is, the value of the row obtained by moving forward the row corresponding to the current value by **offset** (integer) is the sequence number. If the row does not exist after the moving, the result value is the default value. If omitted, **offset** defaults to **1** and **default** to **null**.

Return type: same as the parameter type

Example:

::

   SELECT id,classid,score,LAG(id,3) OVER(ORDER BY score DESC) FROM score;
    id | classid | score | lag
   ----+---------+-------+-----
     1 |       1 |    95 |
     2 |       2 |    95 |
     5 |       2 |    88 |
     3 |       2 |    85 |   1
     4 |       1 |    70 |   2
     6 |       1 |    70 |   5
   (6 rows)

LEAD(value any [, offset integer [, default any ]])
---------------------------------------------------

Description: The **LEAD** function is used for generating leading values for the corresponding values in each group. That is, the value of the row obtained by moving backward the row corresponding to the current value by **offset** (integer) is the sequence number. If the number of rows after the moving exceeds the total number for the current group, the result value is the default value. If omitted, **offset** defaults to **1** and **default** to **null**.

Return type: same as the parameter type

Example:

::

   SELECT id,classid,score,LEAD(id,3) OVER(ORDER BY score DESC) FROM score;
    id | classid | score | lead
   ----+---------+-------+------
     1 |       1 |    95 |    3
     2 |       2 |    95 |    4
     5 |       2 |    88 |    6
     3 |       2 |    85 |
     4 |       1 |    70 |
     6 |       1 |    70 |
   (6 rows)

FIRST_VALUE(value any)
----------------------

Description: The **FIRST_VALUE** function is used for returning the first value of each group.

Return type: same as the parameter type

Example:

::

   SELECT id,classid,score,FIRST_VALUE(id) OVER(ORDER BY score DESC) FROM score;
    id | classid | score | first_value
   ----+---------+-------+-------------
     1 |       1 |    95 |           1
     2 |       2 |    95 |           1
     5 |       2 |    88 |           1
     3 |       2 |    85 |           1
     4 |       1 |    70 |           1
     6 |       1 |    70 |           1
   (6 rows)

LAST_VALUE(value any)
---------------------

Description: Returns the last value of each group.

Return type: same as the parameter type

Example:

::

   SELECT id,classid,score,LAST_VALUE(id) OVER(ORDER BY score DESC) FROM score;
    id | classid | score | last_value
   ----+---------+-------+------------
     1 |       1 |    95 |          2
     2 |       2 |    95 |          2
     5 |       2 |    88 |          5
     3 |       2 |    85 |          3
     4 |       1 |    70 |          6
     6 |       1 |    70 |          6
   (6 rows)

NTH_VALUE(value any, nth integer)
---------------------------------

Description: The *n*\ th row for a group is the returned value. If the row does not exist, **NULL** is returned by default.

Return type: same as the parameter type

Example:

::

   SELECT id,classid,score,NTH_VALUE(id,3) OVER(ORDER BY score DESC) FROM score;
    id | classid | score | nth_value
   ----+---------+-------+-----------
     1 |       1 |    95 |
     2 |       2 |    95 |
     5 |       2 |    88 |         5
     3 |       2 |    85 |         5
     4 |       1 |    70 |         5
     6 |       1 |    70 |         5
   (6 rows)
