:original_name: dws_06_0225.html

.. _dws_06_0225:

TRUNCATE
========

Function
--------

Quickly removes all rows from a database table.

**TRUNCATE** has the same effect as an unqualified **DELETE** on each table, but it is faster since it does not actually scan the tables. This is most useful on large tables.

Precautions
-----------

Exercise caution when running the **TRUNCATE TABLE** statement. Before running this statement, ensure that the table data can be deleted. After you run the **TRUNCATE TABLE** statement to delete table data, the data cannot be restored.

TRUNCATE TABLE
--------------

-  **TRUNCATE TABLE** has the same function as a **DELETE** statement with no **WHERE** clause, emptying a table.
-  **TRUNCATE TABLE** uses less system and transaction log resources as compared with **DELETE**.

   -  **DELETE** deletes a row each time, and records the deletion of each row in the transaction log.
   -  **TRUNCATE TABLE** deletes all rows in a table by releasing the data page storing the table data, and records the releasing of the data page only in the transaction log.

-  The differences between **TRUNCATE**, **DELETE**, and **DROP** are as follows:

   -  **TRUNCATE TABLE** deletes content, releases space, but does not delete definitions.
   -  **DELETE TABLE** deletes content, but does not delete definitions nor release space.
   -  **DROP TABLE** deletes content and definitions, and releases space.

Syntax
------

**TRUNCATE** empties a table or set of tables.

::

   TRUNCATE [ TABLE ] [ ONLY ] {[[database_name.]schema_name.]table_name [ * ]} [, ... ]
       [ CONTINUE IDENTITY ] [ CASCADE | RESTRICT ];

Parameter Description
---------------------

-  **ONLY**

   If **ONLY** is specified, only the specified table is cleared. Otherwise, the table and all its subtables (if any) are cleared.

-  **database_name**

   Database name of the target table

-  **schema_name**

   Schema name of the target table

-  **table_name**

   Specifies the name (optionally schema-qualified) of a target table.

   Value range: an existing table name

-  **CONTINUE IDENTITY**

   Does not change the values of sequences. This is the default.

-  **CASCADE \| RESTRICT**

   -  **CASCADE**: automatically truncates all tables that have foreign-key references to any of the named tables, or to any tables added to the group due to **CASCADE**.
   -  **RESTRICT** (default): refuses to truncate if any of the tables have foreign-key references from tables that are not listed in the command.

Examples
--------

Clear a partitioned table:

::

   TRUNCATE TABLE tpcds.customer_address;
