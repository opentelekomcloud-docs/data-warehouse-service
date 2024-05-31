:original_name: dws_06_0187.html

.. _dws_06_0187:

CREATE VIEW
===========

Function
--------

**CREATE VIEW** creates a view. A view is a virtual table, not a base table. A database only stores the definition of a view and does not store its data. The data is still stored in the original base table. If data in the base table changes, the data in the view changes accordingly. In this sense, a view is like a window through which users can know their interested data and data changes in the database.

Precautions
-----------

-  After the base table on which a view depends is renamed, you need to manually rebuild the view.
-  You can use **WITH (security_barriers)** to create a relatively secure view. This prevents attackers from printing hidden base table data by using the **RAISE** statement of low-cost functions.
-  When the **view_independent** GUC parameter is enabled, columns can be deleted from common views. Note that if a column-level constraint exists, the corresponding column cannot be deleted.

Syntax
------

::

   CREATE [ OR REPLACE ] [ TEMP | TEMPORARY ] VIEW view_name [ ( column_name [, ...] ) ]
       [ WITH ( {view_option_name [= view_option_value]} [, ... ] ) ]
       AS query;

Parameters
----------

-  **OR REPLACE**

   Redefines a view if there is already a view.

-  **TEMP \| TEMPORARY**

   Creates a temporary view.

-  **view_name**

   Specifies the name of a view to be created. It is optionally schema-qualified.

   Value range: a string. It must comply with the naming convention.

-  **column_name**

   Specifies an optional list of names to be used for columns of the view. If not given, the column names are deduced from the query.

   Value range: a string. It must comply with the naming convention.

-  **view_option_name [= view_option_value]**

   This clause specifies optional parameters for a view.

   Currently, the only parameter supported by **view_option_name** is **security_barrier**, which should be enabled when a view is intended to provide row-level security.

   Value range: boolean type. It can be **TRUE** or **FALSE**.

-  **query**

   A **SELECT** or **VALUES** statement which will provide the columns and rows of the view.

   .. important::

      Duplicate CTE names are not supported when the view decoupling function is enabled. The following shows an example:

      ::

         CREATE TABLE t1(a1 INT, b1 INT);
         CREATE TABLE t2(a2 INT, b2 INT, c2 INT);
         CREATE OR REPLACE VIEW v1 AS WITH tmp AS (SELECT * FROM t2) ,tmp1 AS (SELECT b2,c2 FROM tmp WHERE b2 = (WITH RECURSIVE tmp(aa, bb) AS (SELECT a1,b1 FROM t1) SELECT bb FROM tmp WHERE aa = c2)) SELECT c2 FROM tmp1;

Examples
--------

Create a view consisting of columns whose **spcname** is **pg_default**:

::

   CREATE VIEW myView AS
       SELECT * FROM pg_tablespace WHERE spcname = 'pg_default';

Run the following command to redefine the existing view **myView** and create a view consisting of columns whose **spcname** is **pg_global**:

::

   CREATE OR REPLACE VIEW myView AS
       SELECT * FROM pg_tablespace WHERE spcname = 'pg_global';

Create a view consisting of rows with **c_customer_sk** smaller than 150:

::

   CREATE VIEW tpcds.customer_details_view_v1 AS
       SELECT * FROM tpcds.customer
       WHERE c_customer_sk < 150;

Updatable Views
---------------

After the **enable_view_update** parameter is enabled, the **INSERT**, **UPDATE**, **DELETE**, and **MERGE INTO** statements can be used to update simple views. Only 8.1.2 or later supports updates using the **MERGE INTO** statement.

Views that meet all the following conditions can be updated:

-  The FROM clause in the view definition contains only one common table, which cannot be a system table, foreign table, delta table, TOAST table, or error table.
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
