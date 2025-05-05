:original_name: dws_03_0200.html

.. _dws_03_0200:

How Does GaussDB(DWS) Isolate Workloads?
========================================

Workload Isolation
------------------

In GaussDB(DWS), you can isolate workloads through database and schema configurations. The differences are:

-  Databases cannot communicate with each other and share very few resources. Their connections and permissions can be isolated.
-  Schemas share more resources than databases do. User permissions on schemas and subordinate objects can be flexibly configured using the **GRANT** and **REVOKE** syntax.

You are advised to use schemas to isolate services for convenience and resource sharing. It is recommended that system administrators create schemas and databases and then assign required permissions to users.


.. figure:: /_static/images/en-us_image_0000001389253162.png
   :alt: **Figure 1** Used for permission control.

   **Figure 1** Used for permission control.

DATABASE
--------

A database is a physical collection of database objects. Resources of different databases are completely isolated (except some shared objects). Databases are used to isolate workloads. Objects in different databases cannot access each other. For example, objects in Database B cannot be accessed in Database A. Therefore, when logging in to a cluster, you must connect to the specified database.

SCHEMA
------

In a database, database objects are logically divided and isolated based on schemas.

With permission management, you can access and operate objects in different schemas in the same session. Schemas contain objects that applications may access, such as tables, indexes, data in various types, functions, and operators.

Database objects with the same name cannot exist in the same schema, but object names in different schemas can be the same.

::

   gaussdb=> CREATE SCHEMA myschema;
   CREATE SCHEMA
   gaussdb=> CREATE SCHEMA myschema_1;
   CREATE SCHEMA

   gaussdb=> CREATE TABLE myschema.t1(a int, b int) DISTRIBUTE BY HASH(b);
   CREATE TABLE
   gaussdb=> CREATE TABLE myschema.t1(a int, b int) DISTRIBUTE BY HASH(b);
   ERROR:  relation "t1" already exists
   gaussdb=> CREATE TABLE myschema_1.t1(a int, b int) DISTRIBUTE BY HASH(b);
   CREATE TABLE

Schemas logically divide workloads. These workloads are interdependent with the schemas. Therefore, if a schema contains objects, deleting it will cause errors with dependency information displayed.

::

   gaussdb=> DROP SCHEMA myschema_1;
   ERROR: cannot drop schema myschema_1 because other objects depend on it
   Detail: table myschema_1.t1 depends on schema myschema_1
   Hint: Use DROP ... CASCADE to drop the dependent objects too.

When a schema is deleted, the **CASCADE** option is used to delete the objects that depend on the schema.

::

   gaussdb=> DROP SCHEMA myschema_1 CASCADE;
   NOTICE:  drop cascades to table myschema_1.t1
   gaussdb=> DROP SCHEMA

USER/ROLE
---------

Users and roles are used to implement permission control on the database server (cluster). They are the owners and executors of cluster workloads and manage all object permissions in clusters. A role is not confined in a specific database. However, when it logs in to the cluster, it must explicitly specify a user name to ensure the transparency of the operation. A user's permissions to a database can be specified through permission management.

A user is the subject of permissions. Permission management is actually the process of deciding whether a user is allowed to perform operations on database objects.

Permissions Management
----------------------

Permission management in GaussDB(DWS) falls into three categories:

-  System permission

   System permissions are also called user attributes, including **SYSADMIN**, **CREATEDB**, **CREATEROLE**, **AUDITADMIN**, and **LOGIN**.

   They can be specified only by the **CREATE ROLE** or **ALTER ROLE** syntax. The **SYSADMIN** permission can be granted and revoked using **GRANT ALL PRIVILEGE** and **REVOKE ALL PRIVILEGE**, respectively. System permissions cannot be inherited by a user from a role, and cannot be granted using **PUBLIC**.

-  Permissions

   Grant a role's or user's permissions to one or more roles or users. In this case, every role or user can be regarded as a set of one or more database permissions.

   If **WITH ADMIN OPTION** is specified, the member can in turn grant permissions in the role to others, and revoke permissions in the role as well. If a role or user granted with certain permissions is changed or revoked, the permissions inherited from the role or user also change.

   A database administrator can grant permissions to and revoke them from any role or user. Roles having **CREATEROLE** permission can grant or revoke membership in any role that is not an administrator.

-  Object permission

   Permissions on a database object (table, view, column, database, function, schema, or tablespace) can be granted to a role or user. The **GRANT** command can be used to grant permissions to a user or role. These permissions granted are added to the existing ones.

Schema Isolation Example
------------------------

Example 1:

By default, the owner of a schema has all permissions on objects in the schema, including the delete permission. The owner of a database has all permissions on objects in the database, including the delete permission. Therefore, you are advised to strictly control the creation of databases and schemas. Create databases and schemas as an administrator and assign related permissions to users.

#. Assign the permission to create schemas in the **testdb** database to user **user_1** as user **dbadmin**.

   ::

      testdb=> GRANT CREATE ON DATABASE testdb to user_1;
      GRANT

#. Switch to user **user_1**.

   ::

      testdb=> SET SESSION AUTHORIZATION user_1 PASSWORD '********';
      SET

   Create a schema named **myschema_2** in the **testdb** database as **user_1**.

   ::

      testdb=> CREATE SCHEMA myschema_2;
      CREATE SCHEMA

#. Switch to the administrator **dbadmin**.

   ::

      testdb=> RESET SESSION AUTHORIZATION;
      RESET

   Create **table t1** in schema **myschema_2** as the administrator **dbadmin**.

   ::

      testdb=> CREATE TABLE myschema_2.t1(a int, b int) DISTRIBUTE BY HASH(b);
      CREATE TABLE

#. Switch to user **user_1**.

   ::

      testdb=> SET SESSION AUTHORIZATION user_1 PASSWORD '********';
      SET

   Delete table **t1** created by administrator **dbadmin** in schema **myschema_2** as user **user_1**.

   ::

      testdb=> drop table myschema_2.t1;
      DROP TABLE

Example 2:

Due to the logical isolation of schemas, database objects need to be verified at both the schema level and the object level.

#. Grant the permission on the **myschema.t1** table to **user_1**.

   ::

      gaussdb=> GRANT SELECT ON TABLE myschema.t1 TO user_1;
      GRANT

#. Switch to user **user_1**.

   ::

      SET SESSION AUTHORIZATION user_1 PASSWORD '********';
      SET

   Query the table **myschema.t1**.

   ::

      gaussdb=> SELECT * FROM myschema.t1;
      ERROR:  permission denied for schema myschema
      LINE 1: SELECT * FROM myschema.t1;

#. Switch to the administrator **dbadmin**.

   ::

      gaussdb=> RESET SESSION AUTHORIZATION;
      RESET

   Grant the permission on the **myschema.t1** table to user **user_1**.

   ::

      gaussdb=> GRANT USAGE ON SCHEMA myschema TO user_1;
      GRANT

#. Switch to user **user_1**.

   ::

      gaussdb=> SET SESSION AUTHORIZATION user_1 PASSWORD '********';
      SET

   Query the table **myschema.t1**.

   ::

      gaussdb=> SELECT * FROM myschema.t1;
       a | b
      ---+---
      (0 rows)
