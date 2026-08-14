:original_name: dws_06_0187.html

.. _dws_06_0187:

CREATE VIEW
===========

Function
--------

**CREATE VIEW**: creates a view.

A view is a logical virtual table defined based on SQL queries. It only stores the view definition and does not physically store data. When the data in the base table changes, the view query result is automatically updated. The view provides a dynamic data window to help users efficiently focus on the target data.

Syntax
------

::

   CREATE [ OR REPLACE ] [ TEMP | TEMPORARY ] VIEW view_name [ ( column_name [, ...] ) ]
       [ WITH ( {view_option_name [= view_option_value]} [, ... ] ) ]
       AS query;

.. note::

   -  When the **view_independent** GUC parameter is enabled, columns can be deleted from common views. However, if a column-level constraint exists, the column cannot be deleted.

   -  Duplicate CTE names are not supported when the view decoupling function is enabled. Example:

      ::

         CREATE TABLE t1(a1 INT, b1 INT);
         CREATE TABLE t2(a2 INT, b2 INT, c2 INT);
         CREATE OR REPLACE VIEW v1 AS WITH tmp AS (SELECT * FROM t2) ,tmp1 AS (SELECT b2,c2 FROM tmp WHERE b2 = (WITH RECURSIVE tmp(aa, bb) AS (SELECT a1,b1 FROM t1) SELECT bb FROM tmp WHERE aa = c2)) SELECT c2 FROM tmp1;

Parameter Description
---------------------

.. table:: **Table 1** CREATE VIEW parameters

   +------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------+
   | Parameter                                                  | Description                                                                                                                                                                                       | Value Range                                                                                                         |
   +============================================================+===================================================================================================================================================================================================+=====================================================================================================================+
   | OR REPLACE                                                 | If a view with the same name already exists, redefine the view.                                                                                                                                   | ``-``                                                                                                               |
   +------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------+
   | TEMP \| TEMPORARY                                          | Creates a temporary view.                                                                                                                                                                         | ``-``                                                                                                               |
   +------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------+
   | view_name                                                  | Specifies the name of a view to be created. It is optionally schema-qualified.                                                                                                                    | A string, which must comply with the :ref:`naming convention <en-us_topic_0000001811634529__section1475018612353>`. |
   +------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------+
   | column_name                                                | Specifies an optional list of names to be used for columns of the view. If the list is not provided, you can obtain the value from the queried column name.                                       | A string, which must comply with the :ref:`naming convention <en-us_topic_0000001811634529__section1475018612353>`. |
   +------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------+
   | WITH ( {view_option_name [= view_option_value]} [, ... ] ) | Specifies the optional parameters for a view.                                                                                                                                                     | If security_barrier is of the boolean type, the options are as follows:                                             |
   |                                                            |                                                                                                                                                                                                   |                                                                                                                     |
   |                                                            | Currently, **view_option_name** can only be set to **security_barrier**.                                                                                                                          | -  TRUE                                                                                                             |
   |                                                            |                                                                                                                                                                                                   | -  FALSE                                                                                                            |
   |                                                            | You can use **WITH (security_barriers)** to create a relatively secure view. This prevents attackers from printing hidden base table data by using the **RAISE** statement of low-cost functions. |                                                                                                                     |
   +------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------+
   | query                                                      | A **SELECT** or **VALUES** statement which will provide the columns and rows of the view.                                                                                                         | ``-``                                                                                                               |
   +------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------+

Examples
--------

Create the base table **customer** and insert data into the table.

::

   DROP TABLE IF EXISTS customer CASCADE;

   CREATE TABLE customer
   (id int, name varchar(20) , age int)
   with (orientation = column,COMPRESSION=MIDDLE)
   distribute by hash(id);

   INSERT INTO customer VALUES (1,'lily',10),(2, 'lucy',12),(3,'lilei',15);

Create the view **customer_details_view_v1** based on the base table **customer** to view the data whose **age** is less than **12**.

::

   DROP VIEW IF EXISTS customer_details_view_v1;
   CREATE VIEW customer_details_view_v1 AS
   SELECT * FROM customer
   WHERE age < 12;

Updatable Views
---------------

To enable the updatable view function, set the GUC parameter **enable_view_update** (default value: **off**). After this parameter is enabled, the system allows the INSERT, UPDATE, DELETE, and MERGE INTO statements to update simple views.

Views that meet all the following conditions can be updated:

-  The FROM clause in the view definition contains only one common table, which cannot be a system table, foreign table, DFS table, delta table, TOAST table, or error table.
-  The view contains updatable columns, which are simple references to the updatable columns of the base table.
-  The view definition does not contain the WITH, DISTINCT, GROUP BY, ORDER BY, FOR UPDATE, FOR SHARE, HAVING, TABLESAMPLE, LIMIT or OFFSET clause.
-  The view definition does not contain the UNION, INTERSECT, or EXCEPT operation.
-  The selection list of the view definition does not contain aggregate functions, window functions, or functions that return collections.
-  Ensure that there is no trigger whose trigger occasion is **INSTEAD OF** in the view for **INSERT**, **UPDATE**, and **DELETE** statements. For the **MERGE INTO** statement, neither the view nor the underlying table can have triggers.
-  The view definition does not contain sublinks.
-  The view definition does not contain functions whose attribute is **VOLATILE**. The values of such functions can be changed during a table scan.
-  The view definition does not set an alias for the column where the distribution key of the table resides, or name a common column as the distribution key column.
-  When the RETURNING clause is used in the view update operation, columns in the view definition only come from the base table.

If the definition of the updatable view contains a WHERE condition, the condition restricts the UPDATE and DELETE statements from modifying rows on the base table. If the WHERE condition is not met after the UPDATE statement is executed, the updated rows cannot be queried in the view. Similarly, If the WHERE condition is not met after the INSERT statement is executed, the inserted data cannot be queried in the view. To insert, update, or delete data in a view, you must have the corresponding permission on the view and tables.

Helpful Links
-------------

:ref:`ALTER VIEW <dws_06_0150>`, :ref:`DROP VIEW <dws_06_0215>`
