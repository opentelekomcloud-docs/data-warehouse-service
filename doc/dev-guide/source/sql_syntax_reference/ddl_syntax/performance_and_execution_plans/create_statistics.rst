:original_name: dws_06_0364.html

.. _dws_06_0364:

CREATE STATISTICS
=================

Function
--------

Creates an extended statistics object for a table in the current database. The object is owned by the user who executes the command. This syntax is supported only by clusters of version 8.2.1.200 or later (not supported by clusters of version 8.3.0).

Precautions
-----------

-  Currently, this command can only be used to create expression statistics.
-  Expression statistics cannot be created for temporary tables.
-  Expression statistics cannot be created on partitions of a table.
-  Statistics cannot be created for expressions that contain aggregate functions, subqueries, window functions, and return sets.

Syntax
------

::

   CREATE STATISTICS [ IF NOT EXISTS ] statistics_name
       ON ( expression ) [, ...]
       FROM table_name

Parameter Description
---------------------

.. table:: **Table 1** CREATE STATISTICS parameters

   +-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Description                                                                                                                                          | Value Range                                                                                                                     |
   +=================+======================================================================================================================================================+=================================================================================================================================+
   | statistics_name | Indicates the name of the new statistics object.                                                                                                     | A string, which must comply with the :ref:`identifier naming conventions <en-us_topic_0000001811634529__section1475018612353>`. |
   +-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------+
   | expression      | Indicates the expression on which statistics are collected. One or more expressions are supported. Multiple expressions are separated by commas (,). | ``-``                                                                                                                           |
   +-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------+
   | table_name      | Indicates the name of the table whose statistics are to be collected.                                                                                | ``-``                                                                                                                           |
   +-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------+

Examples
--------

Creates a sample table and insert data into it.

.. code-block::

   CREATE TABLE t1_row(id int, dt timestamp) WITH (orientation = row) DISTRIBUTE BY HASH (id);
   INSERT INTO t1_row values (generate_series(1, 721), generate_series('2023-01-01'::timestamp, '2023-01-31'::timestamp, '1 hour'::interval));

Create an expression statistics object for the table and run the ANALYZE command to collect statistics.

.. code-block::

   CREATE STATISTICS s1_t1_row ON EXTRACT(HOUR FROM dt), EXTRACT(DAY FROM dt) FROM t1_row;
   ANALYZE t1_row;

View the query plan of the statement.

.. code-block::

   SET enable_fast_query_shipping = OFF;
   EXPLAIN VERBOSE SELECT * FROM t1_row WHERE EXTRACT(HOUR FROM dt) BETWEEN 0 AND 12;

Helpful Links
-------------

:ref:`DROP STATISTICS <dws_06_0365>`
