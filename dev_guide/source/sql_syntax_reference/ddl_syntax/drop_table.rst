:original_name: dws_06_0208.html

.. _dws_06_0208:

DROP TABLE
==========

Function
--------

**DROP TABLE** deletes a specified table.

Precautions
-----------

-  Exercise caution when running the **DROP TABLE** statement. Ensure that the table can be deleted before running this statement. After you run the **DROP TABLE** statement to delete a table, data in the table cannot be restored.

-  **DROP TABLE** forcibly deletes a specified table. After a table is deleted, any indexes that exist for the table will be deleted; any functions or stored procedures that use this table cannot be run. Deleting a partitioned table also deletes all partitions in the table.
-  Only the table owner, schema owner, or a user granted with the **DROP** permission can run **DROP TABLE** on a table. A system administrator has this permission by default. To delete all the rows in a table but retain the table definition, use **TRUNCATE** or **DELETE**.

Syntax
------

::

   DROP TABLE [ IF EXISTS ]
       { [schema.]table_name } [, ...] [ CASCADE | RESTRICT ];

Parameter Description
---------------------

-  **IF EXISTS**

   Sends a notice instead of an error if the specified table does not exist.

-  **schema**

   Specifies the schema name.

-  **table_name**

   Specifies the name of the table.

-  **CASCADE \| RESTRICT**

   -  **CASCADE**: automatically deletes objects (such as views) that depend on the table to be deleted.
   -  **RESTRICT** (default): refuses to delete the table if any objects depend on it. This is the default.

Example
-------

Delete the **warehouse_t1** table:

::

   DROP TABLE tpcds.warehouse_t1;

Helpful Links
-------------

:ref:`ALTER TABLE <dws_06_0142>`, :ref:`12.101-RENAME TABLE <dws_06_0276>`, and :ref:`CREATE TABLE <dws_06_0177>`
