:original_name: dws_04_1031.html

.. _dws_04_1031:

DELETE
======

Function
--------

Delete data from an HStore table.

Precautions
-----------

-  To delete all the data from a table, you are advised to use the **TRUNCATE** syntax to improve performance and reduce table bloating.
-  If a single record is deleted from an HStore table, a record of the type **D** will be inserted into the delta table. The memory update chain will also be updated to manage concurrency.
-  If multiple records are deleted from an HStore table at a time, a record of the type **D** will be inserted for the consecutive deleted records in each CU.
-  In concurrent deletion scenarios, operations on the same CU will get queued in traditional column-store tables and result in low performance. For HStore tables, the operations can be concurrently performed, and the deletion performance can be more than 100 times that of column-store tables.
-  The syntax is fully compatible with column storage. For more information, see the **UPDATE** syntax.

Syntax
------

.. code-block:: text

   DELETE FROM [ ONLY ] table_name [ * ] [ [ AS ] alias ]
       [ USING using_list ]
       [ WHERE condition ]

Parameters
----------

-  **ONLY**

   If **ONLY** is specified, only that table is deleted. If **ONLY** is not specified, this table and all its sub-tables are deleted.

-  **table_name**

   Specifies the name (optionally schema-qualified) of a target table.

   Value range: an existing table name

-  **alias**

   Specifies the alias for the target table.

   Value range: a string. It must comply with the naming convention.

-  **using_list**

   Specifies the **USING** clause.

-  **condition**

   Specifies an expression that returns a value of type boolean. Only rows for which this expression returns **true** will be deleted.

Example
-------

Create the **reason_t2** table.

::

   CREATE TABLE reason_t2
   (
       TABLE_SK          INTEGER               ,
       TABLE_ID          VARCHAR(20)           ,
       TABLE_NA          VARCHAR(20)
   )WITH(ORIENTATION=COLUMN, ENABLE_HSTORE=ON);
   INSERT INTO reason_t2 VALUES (1, 'S01', 'StudentA'),(2, 'T01', 'TeacherA'),(3, 'T02', 'TeacherB');

Use the **WHERE** condition for deletion.

.. code-block:: text

   DELETE FROM reason_t2 WHERE TABLE_SK = 2;
   DELETE FROM reason_t2 AS rt2 WHERE rt2.TABLE_SK = 2;

Use the **IN** syntax for deletion.

.. code-block:: text

   DELETE FROM reason_t2 WHERE TABLE_SK in (1,3);
