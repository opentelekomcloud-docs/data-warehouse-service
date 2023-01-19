:original_name: dws_06_0239.html

.. _dws_06_0239:

SELECT INTO
===========

Function
--------

**SELECT INTO** defines a new table based on a query result and insert data obtained by query to the new table.

Different from **SELECT**, data found by **SELECT INTO** is not returned to the client. The table columns have the same names and data types as the output columns of the **SELECT**.

Precautions
-----------

**CREATE TABLE AS** provides functions similar to **SELECT INTO** in functions and provides a superset of functions provided by **SELECT INTO**. You are advised to use **CREATE TABLE AS**, because **SELECT INTO** cannot be used in a stored procedure.

Syntax
------

::

   [ WITH [ RECURSIVE ] with_query [, ...] ]
   SELECT [ ALL | DISTINCT [ ON ( expression [, ...] ) ] ]
       { * | {expression [ [ AS ] output_name ]} [, ...] }
       INTO [ UNLOGGED ] [ TABLE ] new_table
       [ FROM from_item [, ...] ]
       [ WHERE condition ]
       [ GROUP BY expression [, ...] ]
       [ HAVING condition [, ...] ]
       [ WINDOW {window_name AS ( window_definition )} [, ...] ]
       [ { UNION | INTERSECT | EXCEPT | MINUS } [ ALL | DISTINCT ] select ]
       [ ORDER BY {expression [ [ ASC | DESC | USING operator ] | nlssort_expression_clause ] [ NULLS { FIRST | LAST } ]} [, ...] ]
       [ { [ LIMIT { count | ALL } ] [ OFFSET start [ ROW | ROWS ] ] } | { LIMIT start, { count | ALL } } ]
       [ FETCH { FIRST | NEXT } [ count ] { ROW | ROWS } ONLY ]
       [ {FOR { UPDATE | SHARE } [ OF table_name [, ...] ] [ NOWAIT ]} [...] ];

Parameter Description
---------------------

**INTO [ UNLOGGED ] [ TABLE ] new_table**

**UNLOGGED** indicates that the table is created as an unlogged table. Data written to unlogged tables is not written to the write-ahead log, which makes them considerably faster than ordinary tables. However, they are not crash-safe: an unlogged table is automatically truncated after a crash or unclean shutdown. The contents of an unlogged table are also not replicated to standby servers. Any indexes created on an unlogged table are automatically unlogged as well.

**new_table** specifies the name of a new table, which can be schema-qualified.

.. note::

   For details about other **SELECT INTO** parameters, see :ref:`Parameter Description <en-us_topic_0000001098990922__s3d562432879c4244bcdbfdf9f30bcc5e>` in **SELECT**.

Example
-------

Add values that are less than 5 in the **r_reason_sk** column in the **tpcds.reason** table to the new table.

::

   SELECT * INTO tpcds.reason_t1 FROM tpcds.reason WHERE r_reason_sk < 5;
   INSERT 0 6

Delete the **tpcds.reason_t1** table.

::

   DROP TABLE tpcds.reason_t1;

Helpful Links
-------------

:ref:`SELECT <dws_06_0238>`
