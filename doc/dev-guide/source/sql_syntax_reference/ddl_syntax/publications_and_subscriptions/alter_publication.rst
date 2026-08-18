:original_name: dws_06_0284.html

.. _dws_06_0284:

ALTER PUBLICATION
=================

Function
--------

**ALTER PUBLICATION** modifies the publication attributes.

Precautions
-----------

-  This statement is supported by version 8.2.0.100 or later clusters.
-  This statement can be used by the owner of a publication and the system administrator only.
-  To alter the owner of a publication, you must also be a direct or indirect member of the new owning role, and that role must have CREATE permissions on the current database.
-  In a publication with **FOR ALL TABLES**, the new publication owner must be the system administrator.
-  An administrator can change the owner relationship of any publication.

Syntax
------

-  Add objects to a publication.

   ::

      ALTER PUBLICATION name ADD publication_object [, ...]

-  Delete objects from a publication.

   ::

      ALTER PUBLICATION name DROP publication_object [, ...]

-  Replace the current object with a specified object.

   ::

      ALTER PUBLICATION name SET publication_object [, ...]

-  Set publication parameters. For parameters not specified, retain their original values.

   ::

      ALTER PUBLICATION name SET ( publication_parameter [= value] [, ... ] )

-  Change the publication owner.

   ::

      ALTER PUBLICATION name OWNER TO new_owner

-  Rename the publication.

   ::

      ALTER PUBLICATION name RENAME TO new_name

The syntax of using **publication_object** is as follows:

.. code-block::

   TABLE table_name [, ...]
   | ALL TABLES IN SCHEMA schema_name [, ... ]

Parameter Description
---------------------

.. table:: **Table 1** ALTER PUBLICATION parameters

   +--------------------------------------------------+------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                                        | Description                                                                  | Value Range                                                                                                                                            |
   +==================================================+==============================================================================+========================================================================================================================================================+
   | name                                             | Specifies the publication name to be modified.                               | Name of an existing publication.                                                                                                                       |
   +--------------------------------------------------+------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_name                                       | Specifies the table name.                                                    | Name of an existing table.                                                                                                                             |
   +--------------------------------------------------+------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
   | schema_name                                      | Specifies the schema name.                                                   | Name of an existing schema.                                                                                                                            |
   +--------------------------------------------------+------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
   | SET ( publication_parameter [= value] [, ... ] ) | Modifies the publication parameters initially set by **CREATE PUBLICATION**. | For details about the parameters, see the :ref:`Parameter Description <en-us_topic_0000001811634585__section1549681213574>` in **CREATE PUBLICATION**. |
   +--------------------------------------------------+------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
   | new_owner                                        | Specifies the new owner of the publication.                                  | Name of an existing user.                                                                                                                              |
   +--------------------------------------------------+------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
   | new_name                                         | Specifies the new name of the publication.                                   | A string compliant with the :ref:`identifier naming rules <en-us_topic_0000001811634529__section1475018612353>`.                                       |
   +--------------------------------------------------+------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+

Examples
--------

-  Create a schema, a sample table, and sample data.

   .. code-block::

      DROP SCHEMA IF EXISTS pub_demo;
      CREATE SCHEMA pub_demo;

      DROP TABLE IF EXISTS pub_demo.sales;
      CREATE TABLE pub_demo.sales
       (
          sale_id    INTEGER,
          product    VARCHAR(50),
          amount     INTEGER NOT NULL,
          sale_date  DATE
      ) WITH (ORIENTATION = COLUMN,enable_disaster_cstore='on') DISTRIBUTE BY HASH ( sale_id);

      DROP TABLE IF EXISTS pub_demo.sales;
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

   Create a publication named **pub_sales**.

   .. code-block::

      CREATE PUBLICATION pub_sales FOR TABLE pub_demo.sales;

-  Add a table to the publication.

   .. code-block::

      ALTER PUBLICATION pub_sales ADD TABLE pub_demo.customers;
      ALTER PUBLICATION pub_sales ADD TABLE pub_demo.customers;

-  Delete a table from the publication.

   .. code-block::

      ALTER PUBLICATION pub_sales DROP ALL TABLES IN SCHEMA pub_demo;
      ALTER PUBLICATION pub_sales DROP TABLE pub_demo.customers;

-  Reset the publication object.

   .. code-block::

      ALTER PUBLICATION pub_sales SET TABLE pub_demo.customers;

-  Add all tables in a schema to the publication.

   .. code-block::

      ALTER PUBLICATION pub_sales ADD ALL TABLES IN SCHEMA public;

-  Modify the publication parameters to publish only the INSERT operation.

   .. code-block::

      ALTER PUBLICATION pub_sales SET (publish = 'insert');

-  Change the publication owner.

   .. code-block::

      CREATE ROLE new_publisher PASSWORD '********';
      GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA pub_demo TO new_publisher;
      ALTER PUBLICATION pub_sales OWNER TO new_publisher;

-  Change the publication name.

   .. code-block::

      ALTER PUBLICATION pub_sales RENAME TO pub_sales_new;

Helpful Links
-------------

:ref:`CREATE PUBLICATION <dws_06_0285>` and :ref:`DROP PUBLICATION <dws_06_0286>`
