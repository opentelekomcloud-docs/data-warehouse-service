:original_name: dws_06_0285.html

.. _dws_06_0285:

CREATE PUBLICATION
==================

Function
--------

**CREATE PUBLICATION** adds a new publication to the current database.

A publication is essentially a group of tables whose data changes are intended to be replicated through logical replication. The database automatically traces and captures data operations (INSERT, UPDATE, DELETE, and TRUNCATE) on these source tables, and continuously transmits the change data to all configured and subscribed destination databases through the logical replication mechanism. In this way, data is synchronized from the source database to the destination databases.

Precautions
-----------

-  This statement is supported by version 8.2.0.100 or later clusters.
-  To create a publication, you must have the CREATE permission on the current database.
-  The name of a newly created publication must not be the same as any existing publication name in the current database.
-  To add a table to a publication, you must own the table. To use **FOR ALL TABLES** and **FOR ALL TABLES IN SCHEMA** clauses, you must have system administrator permissions.
-  If neither **FOR TABLE** nor **FOR ALL TABLES** is specified when a publication is created, the publication is a group of empty tables. You can add tables to the publication using the :ref:`ALTER PUBLICATION <dws_06_0284>` command later.
-  **CREATE PUBLICATION** does not start replication. It only defines a grouping and filtering logic for future subscribers.
-  A to-be-published table cannot be added to a given publication both using **FOR TABLE** and **FOR ALL TABLES IN SCHEMA**.

Syntax
------

::

   CREATE PUBLICATION name
       [ FOR ALL TABLES
         | FOR publication_object [, ... ] ]
       [ WITH ( publication_parameter [=value] [, ... ] ) ];

The syntax of using **publication_object** is as follows:

.. code-block::

   TABLE table_name [, ...]
   | ALL TABLES IN SCHEMA schema_name [, ... ]

.. _en-us_topic_0000001811634585__section1549681213574:

Parameter Description
---------------------

.. table:: **Table 1** CREATE PUBLICATION parameters

   +--------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                                        | Description                                                                                                                                | Value Range                                                                                                                                                         |
   +==================================================+============================================================================================================================================+=====================================================================================================================================================================+
   | name                                             | Specifies the name of a new publication.                                                                                                   | A string, which must comply with the naming convention. For details, see :ref:`Identifier Naming Conventions <en-us_topic_0000001811634529__section1475018612353>`. |
   +--------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | FOR ALL TABLES                                   | Marks a publication as replications of all fines-grained DR primary table changes in the database, including tables to be created.         | ``-``                                                                                                                                                               |
   +--------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | FOR TABLE                                        | Specifies the list of tables to be added to a publication. Only the fine-grained DR primary table can be a part of the publication.        | ``-``                                                                                                                                                               |
   +--------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_name                                       | Specifies the name of the table to be added to the publication, which can include the schema name.                                         | A valid table name.                                                                                                                                                 |
   +--------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | FOR ALL TABLES IN SCHEMA                         | Marks a publication as replications of all fines-grained DR primary table changes a specified schema list, including tables to be created. | ``-``                                                                                                                                                               |
   +--------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | schema_name                                      | The name of the schema to be added to the publication.                                                                                     | A valid schema name.                                                                                                                                                |
   +--------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | WITH ( publication_parameter [=value] [, ... ] ) | Specifies optional parameters for a publication. Currently, only the **publish** parameter is supported.                                   | ``-``                                                                                                                                                               |
   |                                                  |                                                                                                                                            |                                                                                                                                                                     |
   |                                                  | **publish:** Specifies the DML operations that will be published to the subscriber.                                                        |                                                                                                                                                                     |
   |                                                  |                                                                                                                                            |                                                                                                                                                                     |
   |                                                  | -  Value range: A string. Separate the operations by commas (,). The allowed operations are INSERT, UPDATE, DELETE, and TRUNCATE.          |                                                                                                                                                                     |
   |                                                  | -  Default value: **insert, update, delete, truncate**, indicating that all operations are published by default.                           |                                                                                                                                                                     |
   +--------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _en-us_topic_0000001811634585__section1853433744413:

Examples
--------

-  Create a schema, a sample table, and sample data.

   .. code-block::

      CREATE SCHEMA pub_demo;

      CREATE TABLE pub_demo.sales
       (
          sale_id    INTEGER,
          product    VARCHAR(50),
          amount     INTEGER NOT NULL,
          sale_date  DATE
      ) WITH (ORIENTATION = COLUMN,enable_disaster_cstore='on') DISTRIBUTE BY HASH ( sale_id);

      CREATE TABLE pub_demo.customers
      (
          cust_id   INTEGER,
          cust_name VARCHAR(50),
          region    VARCHAR(20)
      ) WITH (ORIENTATION = COLUMN,enable_disaster_cstore='on') DISTRIBUTE BY HASH ( cust_id);

      INSERT INTO pub_demo.sales VALUES (1, 'Laptop', 500, '2026-01-15');
      INSERT INTO pub_demo.sales VALUES (2, 'Mouse',  900,  '2026-01-16');
      INSERT INTO pub_demo.customers VALUES (101, 'Alice', 'East');
      INSERT INTO pub_demo.customers VALUES (102, 'Bob',   'West');

-  Create a publication to publish all tables.

   ::

      CREATE PUBLICATION pub_all_tables FOR ALL TABLES;

-  Create a publication to publish the table **sales**.

   ::

      CREATE PUBLICATION pub_sales_only FOR TABLE pub_demo.sales;

-  Create a publication to publish all operations (INSERT, UPDATE, DELETE, and TRUNCATE) on the table **sales**. By default, all operations are included. You can also explicitly declare the operations.

   ::

      CREATE PUBLICATION pub_sales_all_ops
      FOR TABLE pub_demo.sales
      WITH (publish = 'insert, update, delete, truncate');

-  Create a publication to publish the INSERT operation on the table **customers**.

   ::

      CREATE PUBLICATION pub_cust_insert
      FOR TABLE pub_demo.customers
      WITH (publish = 'insert');

Helpful Links
-------------

:ref:`ALTER PUBLICATION <dws_06_0284>`, :ref:`DROP PUBLICATION <dws_06_0286>`
