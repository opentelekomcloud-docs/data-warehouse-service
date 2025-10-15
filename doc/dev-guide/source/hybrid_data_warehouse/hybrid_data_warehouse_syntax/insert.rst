:original_name: dws_04_1030.html

.. _dws_04_1030:

INSERT
======

Function
--------

Insert one or more rows of data into an HStore table.

Precautions
-----------

-  If the data to be inserted at a time is greater than or equal to the value of the table-level parameter **DELTAROW_THRESHOLD**, the data is directly inserted into the primary table to generate a compression unit (CU).
-  If the data to be inserted is smaller than **DELTAROW_THRESHOLD**, a record of the type **I** will be inserted into the delta table. The data will be serialized and stored in the **values** field of the record.
-  CUIDs are allocated to the data in the delta table and the primary table in a unified manner.
-  The data inserted into the delta table depends on AUTOVACUUM to merge to primary table CUs.

Syntax
------

::

   INSERT [/*+ plan_hint */] [ IGNORE | OVERWRITE ] INTO table_name [ AS alias ] [ ( column_name [, ...] ) ]
       { DEFAULT VALUES
       | VALUES {( { expression | DEFAULT } [, ...] ) }[, ...] | query }

Parameters
----------

-  **table_name**

   Specifies the name of the target table.

   Value range: an existing table name

-  **AS**

   Specifies an alias for the target table *table_name*. *alias* indicates the alias name.

-  **column_name**

   Specifies the name of a column in a table.

-  **query**

   Specifies a query statement (**SELECT** statement) that uses the query result as the inserted data.

Example
-------

Create the **reason_t1** table.

::

   -- Create the reason_t1 table.
   CREATE TABLE reason_t1
   (
       TABLE_SK          INTEGER               ,
       TABLE_ID          VARCHAR(20)           ,
       TABLE_NA          VARCHAR(20)
   )WITH(ORIENTATION=COLUMN, ENABLE_HSTORE=ON);

Insert a record into a table.

::

   INSERT INTO reason_t1(TABLE_SK, TABLE_ID, TABLE_NA) VALUES (1, 'S01', 'StudentA');

Insert records into the table.

::

   INSERT INTO reason_t1 VALUES (1, 'S01', 'StudentA'),(2, 'T01', 'TeacherA'),(3, 'T02', 'TeacherB');
   SELECT * FROM reason_t1 ORDER BY 1;
    TABLE_SK | TABLE_ID | TABLE_NAME
   ----------+----------+------------
           1 |      S01 |   StudentA
           2 |      T01 |   TeacherA
           3 |      T02 |   TeacherB
   (3 rows)
