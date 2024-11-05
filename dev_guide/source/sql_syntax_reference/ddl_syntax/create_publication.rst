:original_name: dws_06_0285.html

.. _dws_06_0285:

CREATE PUBLICATION
==================

Function
--------

Adds a new publication to the current database. The publication name must be different from the name of any existing publication in the current database. A publication is essentially the replication of data changes in a set of tables achieved by logical replication.

Precautions
-----------

-  This statement is supported by clusters of version 8.2.0.100 or later.
-  If neither **FOR TABLE** nor **FOR ALL TABLES** is specified, a publication starts with a set of empty tables. Tables can be added later.
-  Creating a publication does not start replication. It defines only one group and filtering logic for future subscribers. To create a publication, the caller must have CREATE permission on the current database.
-  To add a table to a publication, the caller must have ownership of the table. To use FOR ALL TABLES and FOR ALL TABLES IN SCHEMA clauses, the caller must have system administrator permissions.
-  Do not add a table to the same publication by using FOR TABLE and FOR ALL TABLES IN SCHEMA.

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

Parameter Description
---------------------

-  **name**

   Specifies the name of a new publication.

   Value range: A string. It must comply with the naming convention.

-  **FOR ALL TABLES**

   Marks a publication as replications of all fines-grained DR primary table changes in the database, including tables to be created.

-  **FOR TABLE**

   Specifies the list of tables to be added to a publication. Only the fine-grained DR primary table can be a part of the publication.

-  **table_name**

   Name of the table to be added to the publication, which can include the schema name.

   Value range: A string. It must comply with the naming convention.

-  **FOR ALL TABLES IN SCHEMA**

   Marks a publication as replications of all fines-grained DR primary table changes a specified schema list, including tables to be created.

-  **schema_name**

   The name of the schema to be added to the publication.

   Value range: A string. It must comply with the naming convention.

-  .. _en-us_topic_0000001460561352__li11304141792615:

   **WITH ( publication_parameter [=value] [, ... ] )**

   Specifies optional parameters for a publication. The following parameters are available:

   -  **publish**

      Specifies the DML operations that will be published to the subscriber.

      Value range: A string. Separate the operations by commas (,). The available operations are insert, update, delete, and truncate.

      By default, all actions are released. Therefore, the default value of this option is **insert**, **update**, **delete**, and **truncate**.

Examples
--------

-  Create a publication for all changes of two tables and two schemas.

   Create sample table **tpcds.ship_mode_t1**.

   .. code-block::

      CREATE TABLE tpcds.ship_mode_t1
      (
          SM_SHIP_MODE_SK           INTEGER               NOT NULL,
          SM_SHIP_MODE_ID           CHAR(16)              NOT NULL,
          SM_TYPE                   CHAR(30)                      ,
          SM_CODE                   CHAR(10)                      ,
          SM_CARRIER                CHAR(20)                      ,
          SM_CONTRACT               CHAR(20)
      ) WITH (ORIENTATION = COLUMN,enable_disaster_cstore='on')
      DISTRIBUTE BY HASH(SM_SHIP_MODE_SK);

   Create sample table **tpcds.customer_address_p1**.

   .. code-block::

      CREATE TABLE tpcds.customer_address_p1
      (
          CA_ADDRESS_SK             INTEGER               NOT NULL,
          CA_ADDRESS_ID             CHAR(16)              NOT NULL,
          CA_STREET_NUMBER          CHAR(10)                      ,
          CA_STREET_NAME            VARCHAR(60)                   ,
          CA_STREET_TYPE            CHAR(15)                      ,
          CA_SUITE_NUMBER           CHAR(10)                      ,
          CA_CITY                   VARCHAR(60)                   ,
          CA_COUNTY                 VARCHAR(30)                   ,
          CA_STATE                  CHAR(2)                       ,
          CA_ZIP                    CHAR(10)                      ,
          CA_COUNTRY                VARCHAR(20)                   ,
          CA_GMT_OFFSET             DECIMAL(5,2)                  ,
          CA_LOCATION_TYPE          CHAR(20)
      ) WITH (ORIENTATION = COLUMN,enable_disaster_cstore='on')
      DISTRIBUTE BY HASH(CA_ADDRESS_SK);

   Create sample schema **myschema1**.

   .. code-block::

      CREATE SCHEMA myschema1;

   Create sample schema **myschema2**.

   .. code-block::

      CREATE SCHEMA myschema2;

   Create a publication for all changes of two tables and two schemas.

   .. code-block::

      CREATE PUBLICATION mypublication FOR TABLE users, departments, ALL TABLES IN SCHEMA myschema1, myschema2;

-  Create a publication for all changes in all tables.

   .. code-block::

      CREATE PUBLICATION alltables FOR ALL TABLES;

Helpful Links
-------------

:ref:`ALTER PUBLICATION <dws_06_0284>` :ref:`DROP PUBLICATION <dws_06_0286>`
