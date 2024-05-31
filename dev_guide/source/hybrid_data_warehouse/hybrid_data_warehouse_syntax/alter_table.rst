:original_name: dws_04_1036.html

.. _dws_04_1036:

ALTER TABLE
===========

Function
--------

Modify a table, including modifying the definition of a table, renaming a table, renaming a specified column in a table, adding or updating multiple columns, and changing a column-store table to an HStore table.

Precautions
-----------

-  You can set **enable_hstore** by using **ALTER** to change a column-store table to an HStore table, or to change it back. If **enable_delta** is set to **on**, **enable_hstore** cannot be set to **on**.
-  For some ALTER operations (such as modifying column types, merging partitions, adding NOT NULL constraints, and adding primary key constraints), HStore tables need to merge data to the primary table and then perform ALTER, which may cause extra performance overhead. The impact on performance depends on the data volume in the delta table.
-  When you add a column, do not use **ALTER** to specify other operations (for example, modifying the column type). An **ALTER** statement with only the **ADD COLUMN** parameter can achieve high performance, because it does not require FULL MERGE.
-  The storage parameter **ORIENTATION** cannot be modified.

Modifying Table Attributes
--------------------------

Syntax:

::

   ALTER TABLE [ IF EXISTS ] <table_name> SET  ( {ENABLE_HSTORE = ON} [, ... ] );

To change a column-store table to an HStore table, run the following command:

::

   CREATE TABLE alter_test(a int, b int) WITH(ORIENTATION = COLUMN);
   ALTER TABLE alter_test SET (ENABLE_HSTORE = ON);

.. important::

   To use HStore tables, set the following parameters, or the HStore performance will deteriorate severely. The recommended settings are as follows:

   **autovacuum_max_workers_hstore=3, autovacuum_max_workers=6, autovacuum=true**

Adding a Column
---------------

Syntax:

::

   ALTER TABLE [ IF EXISTS ] <table_name> ADD COLUMN <new_column> <data_type> [ DEFAULT <default_value>];

Example:

Create the **alter_test2** table and add a column to it.

::

   CREATE TABLE alter_test2(a int, b int) WITH(ORIENTATION = COLUMN,ENABLE_HSTORE = ON);
   ALTER TABLE alter_test ADD COLUMN c int;

.. note::

   When adding a column, you are not advised to use **ALTER** to specify other operations in the same SQL statement.

Renaming
--------

Syntax:

::

   ALTER TABLE [ IF EXISTS ] <table_name> RENAME TO <new_table_name>;

Example:

Create table **alter_test3** and rename it as **alter_new**.

::

   CREATE TABLE alter_test3(a int, b int) WITH(ORIENTATION = COLUMN,ENABLE_HSTORE = ON);
   ALTER TABLE alter_test3 RENAME TO alter_new;
