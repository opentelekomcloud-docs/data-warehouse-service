:original_name: dws_06_0225.html

.. _dws_06_0225:

TRUNCATE
========

Function
--------

**TRUNCATE** quickly removes all rows from a database table.

It has the same effect as an unqualified DELETE, but since it does not actually scan the table, it is faster. This is most effective on large tables.

Differences Among TRUNCATE TABLE, DELETE TABLE, and DROP TABLE
--------------------------------------------------------------

.. table:: **Table 1** Differences among TRUNCATE TABLE, DELETE TABLE, and DROP TABLE

   +-----------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------+
   | Dimension       | TRUNCATE TABLE                                                                                                                                                                      | DELETE TABLE                                                                                   | DROP TABLE                                                                |
   +=================+=====================================================================================================================================================================================+================================================================================================+===========================================================================+
   | Syntax          | DDL                                                                                                                                                                                 | DML                                                                                            | DDL                                                                       |
   +-----------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------+
   | Deleted content | All data in the table is deleted, but the table schema is not deleted.                                                                                                              | Only the content is deleted and the definition is not deleted.                                 | Content and definition are deleted.                                       |
   +-----------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------+
   | Space release   | The space is released.                                                                                                                                                              | The space is not released.                                                                     | The space is released.                                                    |
   +-----------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------+
   | Execution Speed | Fast                                                                                                                                                                                | Slow                                                                                           | Fastest                                                                   |
   |                 |                                                                                                                                                                                     |                                                                                                |                                                                           |
   |                 | Data is deleted by releasing the data pages used to store table data. Only the page release is recorded in the transaction log. Less system and transaction log resources are used. | Each time a row is deleted, a record is generated for each deleted row in the transaction log. |                                                                           |
   +-----------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------+
   | Scenario        | You need to quickly delete data from a table while retaining the table schema, and the data volume is large.                                                                        | You need to delete data based on specific conditions and the data volume is under control.     | You need to delete the entire table, including the table schema and data. |
   +-----------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------+

Precautions
-----------

-  Exercise caution when running the **TRUNCATE TABLE** statement. Before running this statement, ensure that the table data can be deleted or has been backed up. After you run the **TRUNCATE TABLE** statement to delete table data, the data cannot be restored.

-  The **TRUNCATE** operation on global temporary tables only truncates data of the current session. Data of other sessions is not affected.
-  In the storage-compute decoupling architecture, it is not possible to perform the **TRUNCATE** operation on common tables and temporary tables simultaneously.

.. warning::

   -  Avoid performing **ALTER TABLE**, **ALTER TABLE PARTITION**, **DROP PARTITION**, and **TRUNCATE** operations during peak hours to prevent long SQL statements from blocking these operations or SQL services.
   -  For more information about development and design specifications, see "DWS Development and Design Proposal" in the *Data Warehouse Service (DWS) Developer Guide*.

Syntax
------

**TRUNCATE** empties a table or set of tables.

::

   TRUNCATE [ TABLE ] [ ONLY ] {[[database_name.]schema_name.]table_name [ * ]} [, ... ]
       [ CONTINUE IDENTITY ] [ CASCADE | RESTRICT ] ;

Parameter Description
---------------------

.. table:: **Table 2** TRUNCATE parameters

   +-----------------------+-----------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter             | Description                                                                                               | Value Range                                                                                                                                                             |
   +=======================+===========================================================================================================+=========================================================================================================================================================================+
   | ONLY                  | Specifies the range of tables to be cleared.                                                              | -  If **ONLY** is specified, only the specified table is cleared.                                                                                                       |
   |                       |                                                                                                           | -  If **ONLY** is not specified, the specified table and all its inherited tables (if any) are cleared.                                                                 |
   |                       | This parameter is reserved only for compatibility with PostgreSQL. DWS does not support inherited tables. |                                                                                                                                                                         |
   +-----------------------+-----------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | database_name         | Specifies the name of the database where the table to be cleared is located.                              | An existing database name.                                                                                                                                              |
   +-----------------------+-----------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | schema_name           | Specifies the schema name of the table to be cleared.                                                     | An existing schema name.                                                                                                                                                |
   +-----------------------+-----------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_name            | Specifies the name of the table to be cleared (which can be schema-qualified).                            | An existing table name.                                                                                                                                                 |
   +-----------------------+-----------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | CONTINUE IDENTITY     | Does not change the values of sequences.                                                                  | This parameter is set to the default value.                                                                                                                             |
   +-----------------------+-----------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | CASCADE \| RESTRICT   | (Optional) Specifies the method of clearing tables that have foreign key references.                      | -  **CASCADE**: automatically truncates all tables that have foreign-key references to any of the named tables, or to any tables added to the group due to **CASCADE**. |
   |                       |                                                                                                           | -  **RESTRICT**: refuses to truncate if any of the tables have foreign-key references from tables that are not listed in the command.                                   |
   |                       |                                                                                                           |                                                                                                                                                                         |
   |                       |                                                                                                           | The default value is RESTRICT.                                                                                                                                          |
   +-----------------------+-----------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Examples
--------

#. Creates the sample table **test_t** and insert data into it.

   ::

      DROP TABLE IF EXISTS test_t;
      CREATE TABLE test_t
      (
          col_id INT PRIMARY KEY,
          col_number INT NOT NULL,
          col_date DATE NOT NULL,
          col_price NUMERIC (10,2),
          col_status TEXT
      )
      WITH (ORIENTATION = COLUMN)
      DISTRIBUTE BY HASH (col_id);

      INSERT INTO test_t VALUES
      (01,300,'2025-03-01',99.20,'sold'),
      (02,400,'2025-04-01',95.50,'sold'),
      (03,450,'2025-05-01',420.50,'sold'),
      (04,100,'2025-06-01',100.85,'restock');

#. View data in the **test_t** table.

   ::

      SELECT * FROM test_t;

   |image1|

#. Clear the **test_t** table.

   ::

      TRUNCATE test_t;

   If the following information is displayed, the table is cleared successfully.

   |image2|

#. View the table definition. The TRUNCATE TABLE operation clears the table but retains the table definition.

   ::

      SELECT * FROM pg_get_tabledef('public.test_t');

   |image3|

.. |image1| image:: /_static/images/en-us_image_0000002624990338.png
.. |image2| image:: /_static/images/en-us_image_0000002624990554.png
.. |image3| image:: /_static/images/en-us_image_0000002624990560.png
