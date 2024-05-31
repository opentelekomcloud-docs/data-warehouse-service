:original_name: dws_04_1034.html

.. _dws_04_1034:

MERGE INTO
==========

Function
--------

The **MERGE INTO** statement is used to conditionally match data in a target table with that in a source table. If data matches, **UPDATE** is executed on the target table; if data does not match, **INSERT** is executed. You can use this syntax to run **UPDATE** and **INSERT** at a time for convenience.

Precautions
-----------

In concurrent MERGE INTO scenarios, the update operations triggered on the same CU will cause lock conflicts in traditional column-store tables and result in low performance. For HStore tables, the operations can be concurrently performed, and the MERGE INTO performance can be more than 100 times that of column-store tables.

Syntax
------

::

   MERGE INTO table_name [ [ AS ] alias ]
   USING { { table_name | view_name } | subquery } [ [ AS ] alias ]
   ON ( condition )
   [
     WHEN MATCHED THEN
     UPDATE SET { column_name = { expression | DEFAULT } |
             ( column_name [, ...] ) = ( { expression | DEFAULT } [, ...] ) } [, ...]
     [ WHERE condition ]
   ]
   [
     WHEN NOT MATCHED THEN
     INSERT { DEFAULT VALUES |
     [ ( column_name [, ...] ) ] VALUES ( { expression | DEFAULT } [, ...] ) [, ...] [ WHERE condition ] }
   ];

Parameters
----------

-  **INTO** clause

   Specifies the target table that is being updated or has data being inserted.

   -  **talbe_name**

      Specifies the name of the target table.

   -  **alias**

      Specifies the alias for the target table.

      Value range: a string. It must comply with the naming convention.

-  **USING** clause

   Specifies the source table, which can be a table, view, or subquery.

-  **ON** clause

   Specifies the condition used to match data between the source and target tables. Columns in the condition cannot be updated. The **ON** association condition can be **ctid**, **xc_node_id**, or **tableoid**.

-  **WHEN MATCHED** clause

   Performs **UPDATE** if data in the source table matches that in the target table based on the condition.

   .. note::

      Distribution columns, system catalogs, and system columns cannot be updated.

-  **WHEN NOT MATCHED** clause

   Specifies that the INSERT operation is performed if data in the source table does not match that in the target table based on the condition.

   .. note::

      -  An **INSERT** clause can contain only one **VALUES**.
      -  The sequence of **WHEN NOT MATCHED** and **WHEN NOT MATCHED** clauses can be exchanged. One of them can be omitted, but they cannot be omitted at the same time.
      -  Two **WHEN MATCHED** or **WHEN NOT MATCHED** clauses cannot be specified at the same time.

Example
-------

Create a target for **MERGE INTO**.

::

   CREATE TABLE target(a int, b int)WITH(ORIENTATION = COLUMN, ENABLE_HSTORE = ON);
   INSERT INTO target VALUES(1, 1),(2, 2);

Create a data source table.

::

   CREATE TABLE source(a int, b int)WITH(ORIENTATION = COLUMN, ENABLE_HSTORE = ON);
   INSERT INTO source VALUES(1, 1),(2, 2),(3, 3),(4, 4),(5, 5);

Run the **MERGE INTO** command.

::

   MERGE INTO target t
   USING source s
   ON (t.a = s.a)
   WHEN MATCHED THEN
     UPDATE SET t.b = t.b + 1
   WHEN NOT MATCHED THEN
     INSERT VALUES (s.a, s.b) WHERE s.b % 2 = 0;
