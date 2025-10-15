:original_name: dws_04_1033.html

.. _dws_04_1033:

UPSERT
======

Function
--------

HStore is compatible with the **UPSERT** syntax. You can add one or more rows to a table. When a row duplicates an existing primary key or unique key value, the row will be ignored or updated.

Precautions
-----------

-  The **UPSERT** statement of updating data upon conflict can be executed only when the target table contains a primary key or unique index.
-  Similar to column storage, an update operation performed using **UPSERT** on an HStore table in the current version involves DELETE and INSERT.
-  In concurrent UPSERT scenarios, operations on the same CU will cause lock conflicts in traditional column-store tables and result in low performance. For HStore tables, the operations can be concurrently performed, and the upsert performance can be more than 100 times that of column-store tables.

Syntax
------

.. table:: **Table 1** UPSERT syntax

   +--------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------+--------------------------------------------------------------+
   | Syntax                                                                                                 | Update Data Upon Conflict                                       | Ignore Data Upon Conflict                                    |
   +========================================================================================================+=================================================================+==============================================================+
   | Syntax 1: No index is specified.                                                                       | .. code-block::                                                 | .. code-block::                                              |
   |                                                                                                        |                                                                 |                                                              |
   |                                                                                                        |    INSERT INTO ON DUPLICATE KEY UPDATE                          |    INSERT IGNORE                                             |
   |                                                                                                        |                                                                 |    INSERT INTO ON CONFLICT DO NOTHING                        |
   +--------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------+--------------------------------------------------------------+
   | Syntax 2: The unique key constraint can be inferred from the specified column name or constraint name. | .. code-block::                                                 | .. code-block::                                              |
   |                                                                                                        |                                                                 |                                                              |
   |                                                                                                        |    INSERT INTO ON CONFLICT(...) DO UPDATE SET                   |    INSERT INTO ON CONFLICT(...) DO NOTHING                   |
   |                                                                                                        |    INSERT INTO ON CONFLICT ON CONSTRAINT con_name DO UPDATE SET |    INSERT INTO ON CONFLICT ON CONSTRAINT con_name DO NOTHING |
   +--------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------+--------------------------------------------------------------+

Parameters
----------

In syntax 1, no index is specified. The system checks for conflicts on all primary keys or unique indexes. If a conflict exists, the system ignores or updates the corresponding data.

In syntax 2, a specified index is used for conflict check. The primary key or unique index is inferred from the column name, the expression that contains column names, or the constraint name specified in the **ON CONFLICT** clause.

-  Unique index inference

   Syntax 2 infers the primary key or unique index by specifying the column name or constraint name. You can specify a single column name or multiple column names by using an expression. Example: **column1, column2, column3**

-  **UPDATE** clause

   The **UPDATE** clause can use **VALUES(colname)** or **EXCLUDED.colname** to reference inserted data. **EXCLUDED** indicates the rows that should be excluded due to conflicts.

-  **WHERE** clause

   -  The **WHERE** clause is used to determine whether a specified condition is met when data conflict occurs. If yes, update the conflict data. Otherwise, ignore it.
   -  Only syntax 2 of **Update Data Upon Conflict** can specify the **WHERE** clause, that is, **INSERT INTO ON CONFLICT(...) DO UPDATE SET WHERE**.

Example
-------

Create table **reason_upsert** and insert data into it.

::

   CREATE TABLE reason_upsert
   (
     a    int primary key,
     b    int,
     c    int
   )WITH(ORIENTATION=COLUMN, ENABLE_HSTORE=ON);
   INSERT INTO reason_upsert VALUES (1, 2, 3);

Ignore conflicting data.

::

   INSERT INTO reason_upsert  VALUES (1, 4, 5),(2, 6, 7) ON CONFLICT(a) DO NOTHING;

Update conflicting data.

::

   INSERT INTO reason_upsert  VALUES (1, 4, 5),(3, 8, 9) ON CONFLICT(a) DO UPDATE SET b = EXCLUDED.b, c = EXCLUDED.c;
