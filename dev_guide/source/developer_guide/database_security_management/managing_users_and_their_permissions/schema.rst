:original_name: dws_04_0059.html

.. _dws_04_0059:

Schema
======

Schemas function as models. Schema management allows multiple users to use the same database without mutual impacts, to organize database objects as manageable logical groups, and to add third-party applications to the same schema without causing conflicts.

Each database has one or more schemas. Each schema contains tables and other types of objects. When a database is created, a schema named **public** is created by default, and all users have permissions for this schema. You can group database objects by schema. A schema is similar to an OS directory but cannot be nested.

The same database object name can be used in different schemas of the same database without causing conflicts. For example, both **a_schema** and **b_schema** can contain a table named **mytable**. Users with required permissions can access objects across multiple schemas of the same database.

If a user is created, a schema named after the user will also be created in the current database.

Database objects are generally created in the first schema in a database search path. For details about the first schema and how to change the schema order, see :ref:`Search Path <en-us_topic_0000001145495017__s54bcf3a1631c42058bbbe25df4b8523b>`.

Creating, Modifying, and Deleting Schemas
-----------------------------------------

-  To create a schema, use **CREATE SCHEMA**. Any user can create a schema.

-  To change the name or owner of a schema, use **ALTER SCHEMA**. Only the schema owner can do so.

-  To delete a schema and its objects, use **DROP SCHEMA**. Only the schema owner can do so.

-  To create a table in a schema, use the *schema_name*\ **.**\ *table_name* format to specify the table. If *schema_name* is not specified, the table will be created in the first schema in :ref:`Search Path <en-us_topic_0000001145495017__s54bcf3a1631c42058bbbe25df4b8523b>`.

-  To view the owner of a schema, perform the following join query on the system catalogs **PG_NAMESPACE** and **PG_USER**. Replace *schema_name* in the statement with the name of the schema to be queried.

   ::

      SELECT s.nspname,u.usename AS nspowner FROM pg_namespace s, pg_user u WHERE nspname='schema_name' AND s.nspowner = u.usesysid;

-  To view a list of all schemas, query the system catalog **PG_NAMESPACE**.

   ::

      SELECT * FROM pg_namespace;

-  To view a list of tables in a schema, query the system catalog **PG_TABLES**. For example, the following query will return a table list from **PG_CATALOG** in the schema.

   ::

      SELECT distinct(tablename),schemaname from pg_tables where schemaname = 'pg_catalog';

.. _en-us_topic_0000001145495017__s54bcf3a1631c42058bbbe25df4b8523b:

Search Path
-----------

A search path is defined in the :ref:`search_path <en-us_topic_0000001145894759__sc0cb61109de045049d5802f668be2392>` parameter. The parameter value is a list of schema names separated by commas (,). If no target schema is specified during object creation, the object will be added to the first schema listed in the search path. If there are objects with the same name across different schemas and no schema is specified for an object query, the object will be returned from the first schema containing the object in the search path.

-  To view the current search path, use **SHOW**.

   ::

      SHOW SEARCH_PATH;
       search_path
      ----------------
       "$user",public
      (1 row)

   The default value of **search_path** is **"$user",public**. **$user** indicates the name of the schema with the same name as the current session user. If the schema does not exist, **$user** will be ignored. By default, after a user connects to a database that has schemas with the same name, objects will be added to all the schemas. If there are no such schemas, objects will be added to only to the **public** schema.

-  To change the default schema of the current session, run the **SET** statement.

   Run the following command to set **search_path** to **myschema** and **public** (**myschema** will be searched first):

   ::

      SET SEARCH_PATH TO myschema, public;
      SET
