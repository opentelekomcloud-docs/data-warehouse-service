:original_name: dws_04_1035.html

.. _dws_04_1035:

SELECT
======

Function
--------

Read data from an HStore table.

Precautions
-----------

-  Currently, neither column-store tables and HStore tables support the **SELECT FOR UPDATE** syntax.

-  When a SELECT query is performed on an HStore table, the system will scan the data in column-store primary table CUs, the delta table, and the update information in each row in the memory. The three types of information will be combined before returned.

-  If data is queried based on the primary key index or unique index,

   For traditional column-store tables, the unique index stores both the data location information (blocknum, offset) of the row-store Delta table and the data location information (cuid, offset) of the column-store primary table. After the data is merged to the primary table, a new index tuple will be inserted, and the index will keep bloating.

   For HStore tables, global CUIDs are allocated in a unified manner. Therefore, only cuid and offset are stored in index tuples. After data is merged, no new index tuples will be generated.

Syntax
------

::

   [ WITH [ RECURSIVE ] with_query [, ...] ]
   SELECT [/*+ plan_hint */] [ ALL | DISTINCT [ ON ( expression [, ...] ) ] ]
   { * | {expression [ [ AS ] output_name ]} [, ...] }
   [ FROM from_item [, ...] ]
   [ WHERE condition ]
   [ GROUP BY grouping_element [, ...] ]
   [ HAVING condition [, ...] ]
   [ { UNION | INTERSECT | EXCEPT | MINUS } [ ALL | DISTINCT ] select ]
   [ ORDER BY {expression [ [ ASC | DESC | USING operator ] | nlssort_expression_clause ] [ NULLS { FIRST | LAST } ]} [, ...] ]
   [ { [ LIMIT { count | ALL } ] [ OFFSET start [ ROW | ROWS ] ] } | { LIMIT start, { count | ALL } } ]

Parameters
----------

-  **DISTINCT [ ON ( expression [, ...] ) ]**

   Removes all duplicate rows from the **SELECT** result set.

   **ON ( expression [, ...] )** is only reserved for the first row among all the rows with the same result calculated using given expressions.

-  **SELECT list**

   Indicates columns to be queried. Some or all columns (using wildcard character \*) can be queried.

   You may use the **AS output_name** clause to give an alias for an output column. The alias is used for the displaying of the output column.

-  **FROM** clause

   Indicates one or more source tables for **SELECT**.

   The **FROM** clause can contain the following elements:

-  **WHERE clause**

   The **WHERE** clause forms an expression for row selection to narrow down the query range of **SELECT**. The condition is any expression that evaluates to a result of Boolean type. Rows that do not satisfy this condition will be eliminated from the output.

   In the **WHERE** clause, you can use the operator (+) to convert a table join to an outer join. However, this method is not recommended because it is not the standard SQL syntax and may raise syntax compatibility issues during platform migration. There are many restrictions on using the operator (+):

-  **GROUP BY clause**

   Condenses query results into a single row all selected rows that share the same values for the grouped expressions.

-  **HAVING clause**

   Selects special groups by working with the **GROUP BY** clause. The **HAVING** clause compares some attributes of groups with a constant. Only groups that matching the logical expression in the **HAVING** clause are extracted.

-  **ORDER BY** clause

   Sorts data retrieved by **SELECT** in descending or ascending order. If the **ORDER BY** expression contains multiple columns:

Example
-------

Create the **reason_select** table and insert data into the table.

::

   CREATE TABLE reason_select
   (
     r_reason_sk integer,
     r_reason_id integer,
     r_reason_desc character(100)
   )WITH(ORIENTATION = COLUMN, ENABLE_HSTORE=ON);
   INSERT INTO reason_select values(3, 1,'reason 1'),(10, 2,'reason 2'),(4, 3,'reason 3'),(10, 4,'reason 4');

Perform the GROUP BY operation.

::

   SELECT COUNT(*), r_reason_sk FROM reason_select GROUP BY r_reason_sk;

Perform the HAVING filtering operation.

::

   SELECT COUNT(*) c,r_reason_sk FROM reason_select GROUP BY r_reason_sk HAVING c > 1;

Perform the ORDER BY operation.

::

   SELECT * FROM reason_select ORDER BY r_reason_sk;
