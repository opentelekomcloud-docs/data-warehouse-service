:original_name: dws_04_0036.html

.. _dws_04_0036:

Creating and Managing Schemas
=============================

A schema is the logical organization of objects and data in a database. Schema management allows multiple users to use the same database without interfering with each other. Third-party applications can be added to corresponding schemas to avoid conflicts.

The same database object name can be used in different schemas in a database without causing conflicts. For example, both **a_schema** and **b_schema** can contain a table named **mytable**. Users with required permissions can access objects across multiple schemas in a database.

If a user is created, a schema named after the user will also be created in the current database.

The Default Schema **Public**
-----------------------------

Each database has a schema named **Public**. If you do not create any schema, the object will be created in the schema named public. All database roles (users) have the CREATE and USAGE permissions in the public schema. When creating a schema, you need to grant the access permission to users.

Creating a Schema
-----------------

-  Run the **CREATE SCHEMA** command to create a schema.

   ::

      CREATE SCHEMA myschema;

   To create or access an object in the schema, the object name in the command should be composed of the schema name and the object name, which are separated by a dot (.), for example, **myschema.table**.

-  Users can create a schema owned by others. For example, run the following command to create a schema named **myschema** and set the owner of the schema to user **jack**:

   ::

      CREATE SCHEMA myschema AUTHORIZATION jack;

   If **authorization username** is not specified, the schema owner is the user who runs the command.

Modifying a Schema
------------------

-  Run the **ALTER SCHEMA** command to change the schema name. Only the schema owner can change the schema name.

   ::

      ALTER SCHEMA schema_name RENAME TO new_name;

-  Run the **ALTER SCHEMA** command to change the schema owner.

   ::

      ALTER SCHEMA schema_name OWNER TO new_owner;

Setting the Schema Search Path
------------------------------

The GUC parameter **search_path** specifies the schema search sequence. The parameter value is a series of schema names separated by commas (,). If no schema is specified during object creation, the object will be added to the first schema displayed in the search path. If there are objects with the same name in different schemas and no schema is specified for an object query, the object will be returned from the first schema containing the object in the search path.

-  Run the **SHOW** command to view the current search path.

   ::

      SHOW SEARCH_PATH;
       search_path
      ----------------
       "$user",public
      (1 row)

   The default value of **search_path** is **"$user",public**. **$user** indicates the name of the schema with the same name as the current session user. If the schema does not exist, **$user** will be ignored. By default, after a user connects to a database that has schemas with the same name, objects will be added to all the schemas. If there are no such schemas, objects will be added to only to the **public** schema.

-  Run the **SET** command to modify the default schema of the current session. For example, if the search path is set to "**myschema**, **public**", **myschema** is searched first.

   ::

      SET SEARCH_PATH TO myschema, public;

   You can also run the **ALTER ROLE** command to set search_path for a role (user). For example:

   ::

      ALTER ROLE jack SET search_path TO myschema, public;

Using a Schema
--------------

If you want to create or access an object in a specified schema, the object name must contain the schema name. To be specific, the name consists of a schema name and an object name, which are separated by a dot (.).

-  Create a table **mytable** in **myschema**. Create a table in **schema_name.table_name** format.

   ::

      CREATE TABLE myschema.mytable(id int, name varchar(20));

-  Query all data in the table **mytable** in **myschema**.

   ::

      SELECT * FROM myschema.mytable;
       id | name
      ----+------
      (0 rows)

Viewing a Schema
----------------

-  Use the **current_schema()** function to view the current schema.

   ::

      SELECT current_schema();
       current_schema
      ----------------
       myschema
      (1 row)

-  To view the owner of a schema, perform the following join query on the system catalogs **PG_NAMESPACE** and **PG_USER**. Replace *schema_name* in the statement with the name of the schema to be queried.

   ::

      SELECT s.nspname,u.usename AS nspowner FROM PG_NAMESPACE s, PG_USER u WHERE nspname='schema_name' AND s.nspowner = u.usesysid;

-  To view a list of all schemas, query the system catalog **PG_NAMESPACE**.

   ::

      SELECT * FROM PG_NAMESPACE;

-  Use the **PGXC_TOTAL_SCHEMA_INFO** view to query the space usage of schemas in the cluster.

   ::

      SELECT * FROM PGXC_TOTAL_SCHEMA_INFO;

-  To view a list of tables in a schema, query the system catalog **PG_TABLES**. For example, the following query will return a table list from **PG_CATALOG** in the schema.

   ::

      SELECT distinct(tablename),schemaname FROM PG_TABLES where schemaname = 'pg_catalog';

Schema Permission Control
-------------------------

By default, a user can only access database objects in its own schema. To access objects in other schemas, the user must have the **usage** permission of the corresponding schema.

By granting the **CREATE** permission for a schema to a user, the user can create objects in this schema.

-  Grant the **usage** permission of **myschema** to user **jack**.

   ::

      GRANT USAGE ON schema myschema TO jack;

-  Run the following command to revoke the **USAGE** permission for **myschema** from **jack**:

   ::

      REVOKE USAGE ON schema myschema FROM jack;

Drop Schema
-----------

-  Run the **DROP SCHEMA** command to delete an empty schema (no database objects in the schema).

   ::

      DROP SCHEMA IF EXISTS myschema;

-  By default, a schema must be empty before being deleted. To delete a schema and all its objects (such as tables, data, and functions), use the **CASCADE** keyword.

   ::

      DROP SCHEMA myschema CASCADE;

System Schema
-------------

-  Each database has a **pg_catalog** schema, which contains system catalogs and all built-in data types, functions, and operators. **pg_catalog** is a part of the search path and has the second highest search priority. It is searched after the schema of temporary tables and before other schemas specified in **search_path**. This search order ensures that database built-in objects can be found. To use a custom object that has the same name as a built-in object, you can specify the schema of the custom object.
-  The **information_schema** consists of a collection of views that contain object information in a database. These views obtain system information from the system catalogs in a standardized way.
