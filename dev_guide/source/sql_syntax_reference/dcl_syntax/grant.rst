:original_name: dws_06_0250.html

.. _dws_06_0250:

GRANT
=====

Function
--------

**GRANT** grants permissions to roles and users.

**GRANT** is used in the following scenarios:

-  **Granting system permissions to roles or users**

   System permissions are also called user attributes, including **SYSADMIN**, **CREATEDB**, **CREATEROLE**, **AUDITADMIN**, and **LOGIN**.

   They can be specified only by the **CREATE ROLE** or **ALTER ROLE** syntax. The **SYSADMIN** permission can be granted and revoked using **GRANT ALL PRIVILEGE** and **REVOKE ALL PRIVILEGE**, respectively. System permissions cannot be inherited by a user from a role, and cannot be granted using **PUBLIC**.

-  **Granting database object permissions to roles or users**

   Grant permissions related to database objects (tables, views, specified columns, databases, functions, and schemas) to specified roles or users.

   **GRANT** grants specified database object permissions to one or more roles. These permissions are appended to those already granted, if any.

   The key word **PUBLIC** indicates that the permissions are to be granted to all roles, including those that might be created later. **PUBLIC** can be regarded as an implicitly defined group including all roles. Any particular role will have the sum of permissions granted directly to it using **GRANT**, permissions granted to any role it is presently a member of, and permissions granted to **PUBLIC**.

   If **WITH GRANT OPTION** is specified, the recipient of a permission can in turn grant it to others. This option cannot be granted to **PUBLIC**. Only GaussDB(DWS) supports this operation.

   GaussDB(DWS) grants the permissions for objects of certain types to **PUBLIC**. By default, permissions for tables, table columns, sequences, external data sources, external servers, schemas, and tablespace are not granted to **PUBLIC**. However, permissions for the following objects are granted to **PUBLIC**: **CONNECT** and **CREATE TEMP TABLE** permissions for databases, **EXECUTE** permission for functions, and **USAGE** permission for languages and data types (including domains). An object owner can revoke the default permissions granted to **PUBLIC** and grant permissions to other users as needed. For security purposes, you are advised to create an object and set permissions for it in the same transaction so that other users do not have time windows to use the object. In addition, you can run the **ALTER DEFAULT PRIVILEGES** statement to modify the initial default permissions.

-  **Granting a role's or user's permissions to other roles or users**

   Grant a role's or user's permissions to one or more roles or users. In this case, every role or user can be regarded as a set of one or more database permissions.

   If **WITH ADMIN OPTION** is specified, the member can in turn grant permissions in the role to others, and revoke permissions in the role as well. If a role or user granted with certain permissions is changed or revoked, the permissions inherited from the role or user also change.

   A database administrator can grant permissions to and revoke them from any role or user. Roles having **CREATEROLE** permission can grant or revoke membership in any role that is not an administrator.

Precautions
-----------

None

Syntax
------

-  Grant the table or view access permission to a specified role or user. Do not perform **GRANT** on a table partition. Otherwise, an alarm will be generated.

   ::

      GRANT { { SELECT | INSERT | UPDATE | DELETE | TRUNCATE | REFERENCES | TRIGGER | ANALYZE | ANALYSE | VACUUM | ALTER | DROP } [, ...]
            | ALL [ PRIVILEGES ] }
          ON { [ TABLE ] table_name [, ...]
             | ALL TABLES IN SCHEMA schema_name [, ...] }
          TO { [ GROUP ] role_name | PUBLIC } [, ...]
          [ WITH GRANT OPTION ];

-  Grant the column access permission to a specified role or user.

   ::

      GRANT { {{ SELECT | INSERT | UPDATE | REFERENCES } ( column_name [, ...] )} [, ...]
            | ALL [ PRIVILEGES ] ( column_name [, ...] ) }
          ON [ TABLE ] table_name [, ...]
          TO { [ GROUP ] role_name | PUBLIC } [, ...]
          [ WITH GRANT OPTION ];

-  Grant the database access permission to a specified role or user.

   ::

      GRANT { { CREATE | CONNECT | TEMPORARY | TEMP } [, ...]
            | ALL [ PRIVILEGES ] }
          ON DATABASE database_name [, ...]
          TO { [ GROUP ] role_name | PUBLIC } [, ...]
          [ WITH GRANT OPTION ];

-  Grant the domain access permission to a specified role or user.

   ::

      GRANT { USAGE | ALL [ PRIVILEGES ] }
          ON DOMAIN domain_name [, ...]
          TO { [ GROUP ] role_name | PUBLIC } [, ...]
          [ WITH GRANT OPTION ];

   .. note::

      The current version does not support granting the domain access permission.

-  Grant the external data source access permission to a specified role or user.

   ::

      GRANT { USAGE | ALL [ PRIVILEGES ] }
          ON FOREIGN DATA WRAPPER fdw_name [, ...]
          TO { [ GROUP ] role_name | PUBLIC } [, ...]
          [ WITH GRANT OPTION ];

-  Grant the external server access permission to a specified role or user.

   ::

      GRANT { USAGE | ALL [ PRIVILEGES ] }
          ON FOREIGN SERVER server_name [, ...]
          TO { [ GROUP ] role_name | PUBLIC } [, ...]
          [ WITH GRANT OPTION ];

-  Grant the function access permission to a specified role or user.

   ::

      GRANT { EXECUTE | ALL [ PRIVILEGES ] }
          ON { FUNCTION {function_name ( [ {[ argmode ] [ arg_name ] arg_type} [, ...] ] )} [, ...]
             | ALL FUNCTIONS IN SCHEMA schema_name [, ...] }
          TO { [ GROUP ] role_name | PUBLIC } [, ...]
          [ WITH GRANT OPTION ];

-  Grant the procedural language access permission to a specified role or user.

   ::

      GRANT { USAGE | ALL [ PRIVILEGES ] }
          ON LANGUAGE lang_name [, ...]
          TO { [ GROUP ] role_name | PUBLIC } [, ...]
          [ WITH GRANT OPTION ];

   .. note::

      The current version does not support granting the procedural language access permission.

-  Grant the large object access permission to a specified role or user.

   ::

      GRANT { { SELECT | UPDATE } [, ...] | ALL [ PRIVILEGES ] }
          ON LARGE OBJECT loid [, ...]
          TO { [ GROUP ] role_name | PUBLIC } [, ...]
          [ WITH GRANT OPTION ];

   .. note::

      The current version does not support granting the large object access permission.

-  Grant the sequence access permission to a specified role or user.

   ::

      GRANT { { SELECT | UPDATE | USAGE } [, ...] | ALL [ PRIVILEGES ] }
          ON { SEQUENCE sequence_name [, ...]
               | ALL SEQUENCES IN SCHEMA schema_name [, ...] }
          TO { [ GROUP ] role_name | PUBLIC } [, ...]
          [ WITH GRANT OPTION ];

-  Grant the sub-cluster access permission to a specified role or user. Common users cannot perform **GRANT** or **REVOKE** operations on node groups.

   ::

      GRANT { CREATE | USAGE | COMPUTE | ALL [ PRIVILEGES ] }
          ON NODE GROUP group_name [, ...]
          TO { [ GROUP ] role_name | PUBLIC } [, ...]
          [ WITH GRANT OPTION ];

-  Grant the schema access permission to a specified role or user.

   ::

      GRANT { { CREATE | USAGE | ALTER | DROP } [, ...] | ALL [ PRIVILEGES ] }
          ON SCHEMA schema_name [, ...]
          TO { [ GROUP ] role_name | PUBLIC } [, ...]
          [ WITH GRANT OPTION ];

   .. note::

      When you grant table or view rights to other users, you also need to grant the USAGE permission for the schema that the tables and views belong to. Without this permission, the users granted with the table or view rights can only see the object names, but cannot access them.

-  Grant the type access permission to a specified role or user.

   ::

      GRANT { USAGE | ALL [ PRIVILEGES ] }
          ON TYPE type_name [, ...]
          TO { [ GROUP ] role_name | PUBLIC } [, ...]
          [ WITH GRANT OPTION ];

   .. note::

      The current version does not support granting the type access permission.

-  Grant a role's rights to other users or roles.

   ::

      GRANT role_name [, ...]
         TO role_name [, ...]
         [ WITH ADMIN OPTION ];

-  Grant the SYSADMIN permission to a specified role.

   ::

      GRANT ALL { PRIVILEGES | PRIVILEGE }
         TO role_name;

.. _en-us_topic_0000001188270556__s226158f44a8f4b908e69a283aeb813cd:

Parameter Description
---------------------

**GRANT** grants the following permissions:

-  **SELECT**

   Allows **SELECT** from any column, or the specific columns listed, of the specified table, view, or sequence.

-  **INSERT**

   Allows **INSERT** of a new row into the specified table.

-  **UPDATE**

   Allows **UPDATE** of any column, or the specific columns listed, of the specified table. **SELECT ... FOR UPDATE** and **SELECT ... FOR SHARE** also require this permission on at least one column, in addition to the SELECT permission.

-  **DELETE**

   Allows **DELETE** of a row from the specified table.

-  **TRUNCATE**

   Allows **TRUNCATE** on the specified table.

-  **REFERENCES**

   To create a foreign key constraint, it is necessary to have this permission on both the referencing and referenced columns.

-  **TRIGGER**

   To create a trigger, you must have the TRIGGER permission on the table or view.

-  **ANALYZE \| ANALYSE**

   To perform the ANALYZE \| ANALYSE operation on a table to collect statistics data, you must have the ANALYZE \| ANALYSE permission on the table.

-  **CREATE**

   -  For databases, allows new schemas to be created within the database.
   -  For schemas, allows new objects to be created within the schema. To rename an existing object, you must own the object and have this permission for the schema where the object is located.
   -  For sub-clusters, allows tables to be created.

-  **CONNECT**

   Allows the user to connect to the specified database.

-  **TEMPORARY \| TEMP**

   Allows temporary tables to be created when the specified database is used.

-  **EXECUTE**

   Allows the use of the specified function and the use of any operators that are implemented on top of the function.

-  **USAGE**

   -  For procedural languages, allows the use of the specified language for the creation of functions in that language.
   -  For schemas, allows access to objects contained in the specified schema. Without this permission, it is still possible to see the object names.
   -  For sequences, allows the use of the **NEXTVAL** function.
   -  For sub-clusters, allows users who can access objects contained in the specified schema to access tables in a specified sub-cluster.

-  **COMPUTE**

   Allows users to perform elastic computing in a computing sub-cluster that they have the compute permission on.

-  **ALL PRIVILEGES**

   Grants all of the available permissions at once. Only system administrators have permission to run **GRANT ALL PRIVILEGES**.

-  **WITH GRANT OPTION**

   Specifies whether permission transfer is allowed. If **WITH GRANT OPTION** is specified, the recipient of a permission can in turn grant it to others. This option cannot be granted to **PUBLIC**.

   .. note::

      -  **WITH GRANT OPTION** cannot be used with **NODE GROUP**.
      -  When using **WITH GRANT OPTION**, ensure that **enable_grant_option** is set to **on**.

-  **WITH ADMIN OPTION**

   Specifies whether permission transfer is allowed. If **WITH ADMIN OPTION** is specified, members of a role can grant membership of the role to others.

**GRANT** parameters are as follows:

-  **role_name**

   Specifies an existing user name.

-  **table_name**

   Specifies an existing table name.

-  **column_name**

   Specifies an existing column name.

-  **schema_name**

   Specifies an existing schema name.

-  **database_name**

   Specifies an existing database name.

-  **function_name**

   Specifies an existing function name.

-  **sequence_name**

   Specifies an existing sequence name.

-  **domain_name**

   Specifies an existing domain type.

-  **fdw_name**

   Specifies an existing foreign data wrapper name.

-  **lang_name**

   Specifies an existing language name.

-  **type_name**

   Specifies an existing type name.

-  **group_name**

   Specifies an existing sub-cluster name.

-  **argmode**

   Specifies the parameter mode.

   Value range: a string. It must comply with the naming convention.

-  **arg_name**

   Indicates the parameter name.

   Value range: a string. It must comply with the naming convention.

-  **arg_type**

   Specifies the parameter type.

   Value range: a string. It must comply with the naming convention.

-  **loid**

   Identifier of the large object that includes this page

   Value range: a string. It must comply with the naming convention.

Examples
--------

Create two users:

::

   CREATE USER joe PASSWORD '{Password}';
   CREATE USER kim PASSWORD '{Password}';

Create a schema:

::

   CREATE SCHEMA tpcds;

Create a table:

::

   CREATE TABLE IF NOT EXISTS tpcds.reason(r_reason_sk int,r_reason_id int,r_reason_desc int);

-  **Grant system permissions to a user or role.**

   -  Grant all available permissions of user **sysadmin** to user **joe**:

      ::

         GRANT ALL PRIVILEGES TO joe;

      Afterward, user **joe** has the sysadmin permissions.

-  **Grant object permissions to a user or role.**

   -  Grant the SELECT permission on the **tpcds.reason** table to user **joe**:

      ::

         GRANT SELECT ON TABLE tpcds.reason TO joe;

   -  Grant all permissions of the **tpcds.reason** table to user **kim**.

      ::

         GRANT ALL PRIVILEGES ON tpcds.reason TO kim;

      After the granting succeeds, user **kim** has all the permissions of the **tpcds.reason** table, including the add, delete, modify, and query permissions.

   -  Grant the permission to use the **tpcds** schema to user **joe**.

      ::

         GRANT USAGE ON SCHEMA tpcds TO joe;

      After the authorization is successful, user **joe** has the **USAGE** permission of the schema and can access the objects contained in the schema.

   -  Grant the query permission for the **r_reason_sk**, **r_reason_id**, and **r_reason_desc** columns and the update permission for the **r_reason_desc** column in the **tpcds.reason** table to user **joe**:

      ::

         GRANT select (r_reason_sk,r_reason_id,r_reason_desc),update (r_reason_desc) ON tpcds.reason TO joe;

      After the granting succeeds, user **joe** immediately has the query permission of the **r_reason_sk** and **r_reason_id** columns in the **tpcds.reason** table.

      ::

         GRANT select (r_reason_sk, r_reason_id) ON tpcds.reason TO joe ;

   -  Grant the **EXECUTE** permission of the **func_add_sql** function to user **joe**.

      ::

         CREATE FUNCTION func_add_sql(integer, integer) RETURNS integer
             AS 'select $1 + $2;'
             LANGUAGE SQL
             IMMUTABLE
             RETURNS NULL ON NULL INPUT;
         GRANT EXECUTE ON FUNCTION func_add_sql(integer, integer) TO joe;

   -  Grant the **UPDATE** permission of the sequence **serial** to user **joe**.

      ::

         CREATE SEQUENCE serial START 101 CACHE 20;
         GRANT UPDATE ON SEQUENCE serial TO joe;

   -  Grant the **gaussdb** database connection permission and schema creation permission to user **joe**:

      ::

         GRANT create,connect on database gaussdb TO joe ;

   -  Grant the **tpcds** schema access permission and object creation permission to **joe**, but do not enable it to grant these permissions to others:

      ::

         GRANT USAGE,CREATE ON SCHEMA tpcds TO joe;

-  **Grant the permissions of a user or role to other users or roles.**

   -  Grant the permissions of user **joe** to user **kim**, and allow **kim** to grant these permissions to others:

      ::

         GRANT joe TO kim WITH ADMIN OPTION;

   -  Grant the permissions of user **joe** to user **kim**:

      ::

         GRANT joe TO kim;

Helpful Links
-------------

:ref:`REVOKE <dws_06_0253>`, :ref:`ALTER DEFAULT PRIVILEGES <dws_06_0244>`
