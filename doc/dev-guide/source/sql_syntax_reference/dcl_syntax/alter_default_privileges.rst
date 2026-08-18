:original_name: dws_06_0244.html

.. _dws_06_0244:

ALTER DEFAULT PRIVILEGES
========================

Function
--------

**ALTER DEFAULT PRIVILEGES** allows you to set the permissions that will be used for objects to be created. It does not affect permissions assigned to existing objects.

A user can modify only the default permissions of objects created by the user or the role to which the user belongs. These permissions can be set globally (all objects created in the database) or for objects in a specified schema.

You can use the **PG_DEFAULT_ACL** system catalog to view the default permissions of database users.

Precautions
-----------

-  Only the permissions for tables (including views), sequences, functions, and types (including domains) can be altered.
-  The VACUUM, DROP, and ALTER permissions on foreign tables cannot be granted to users.

Syntax
------

::

   ALTER DEFAULT PRIVILEGES
       [ FOR { ROLE | USER } target_role [, ...] ]
       [ IN SCHEMA schema_name [, ...] ]
       abbreviated_grant_or_revoke;

The **abbreviated_grant_or_revoke** clause is used to specify the objects for which permissions are granted or revoked. The options include:

.. code-block::

   grant_on_tables_clause
   grant_on_functions_clause
   grant_on_types_clause
   grant_on_sequences_clause
   revoke_on_tables_clause
   revoke_on_functions_clause
   revoke_on_types_clause
   revoke_on_sequences_clause

-  **grant_on_tables_clause** grants permissions on tables.

   ::

      GRANT { { SELECT | INSERT | UPDATE | DELETE | TRUNCATE | REFERENCES | TRIGGER | ANALYZE | ANALYSE | VACUUM | ALTER | DROP }
          [, ...] | ALL [ PRIVILEGES ] }
          ON TABLES
          TO { [ GROUP ] role_name | PUBLIC } [, ...]
          [ WITH GRANT OPTION ]

-  **grant_on_functions_clause** grants permissions on functions.

   ::

      GRANT { { EXECUTE | ALTER | DROP } [, ...] | ALL [ PRIVILEGES ] }
          ON FUNCTIONS
          TO { [ GROUP ] role_name | PUBLIC } [, ...]
          [ WITH GRANT OPTION ]

-  **grant_on_types_clause** grants permissions on types.

   ::

      GRANT { USAGE | ALL [ PRIVILEGES ] }
          ON TYPES
          TO { [ GROUP ] role_name | PUBLIC } [, ...]
          [ WITH GRANT OPTION ]

-  **grant_on_sequences_clause** grants permissions on sequences.

   ::

      GRANT { { USAGE | SELECT | UPDATE }
          [, ...] | ALL [ PRIVILEGES ] }
          ON SEQUENCES
          TO { [ GROUP ] role_name | PUBLIC } [, ...]
          [ WITH GRANT OPTION ]

-  **revoke_on_tables_clause** revokes permissions on table objects.

   ::

      REVOKE [ GRANT OPTION FOR ]
          { { SELECT | INSERT | UPDATE | DELETE | TRUNCATE | REFERENCES | TRIGGER | ANALYZE | ANALYSE | VACUUM | ALTER | DROP }
          [, ...] | ALL [ PRIVILEGES ] }
          ON TABLES
          FROM { [ GROUP ] role_name | PUBLIC } [, ...]
          [ CASCADE | RESTRICT | CASCADE CONSTRAINTS ]

-  **revoke_on_functions_clause** revokes permissions on functions.

   ::

      REVOKE [ GRANT OPTION FOR ]
          { { EXECUTE | ALTER | DROP } [, ...] | ALL [ PRIVILEGES ] }
          ON FUNCTIONS
          FROM { [ GROUP ] role_name | PUBLIC } [, ...]
          [ CASCADE | RESTRICT | CASCADE CONSTRAINTS ]

-  **revoke_on_types_clause** revokes permissions on types.

   ::

      REVOKE [ GRANT OPTION FOR ]
          { USAGE | ALL [ PRIVILEGES ] }
          ON TYPES
          FROM { [ GROUP ] role_name | PUBLIC } [, ...]
          [ CASCADE | RESTRICT | CASCADE CONSTRAINTS ]

-  **revoke_on_sequences_clause** revokes permissions on sequences.

   ::

      REVOKE [ GRANT OPTION FOR ]
          { { USAGE | SELECT | UPDATE }
          [, ...] | ALL [ PRIVILEGES ] }
          ON SEQUENCES
          FROM { [ GROUP ] role_name | PUBLIC } [, ...]
          [ CASCADE | RESTRICT | CASCADE CONSTRAINTS ]

Parameter Description
---------------------

.. table:: **Table 1** ALTER DEFAULT PRIVILEGES parameters

   +------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------+
   | Parameter                                | Description                                                                                                                                                                                                                                                                                | Value Range or Example                                                                                      |
   +==========================================+============================================================================================================================================================================================================================================================================================+=============================================================================================================+
   | FOR { ROLE \| USER } target_role [, ...] | **target_role** following **FOR ROLE/USER** can be a role or user name that will create objects in the future. That is, the role or user name corresponding to the owner of the objects to be created in the future. If **FOR ROLE/USER** is omitted, the current role or user is assumed. | Name of an existing role or user.                                                                           |
   |                                          |                                                                                                                                                                                                                                                                                            |                                                                                                             |
   |                                          | **target_role** must have the CREATE permissions for **schema_name**. You can use the **has_schema_privilege** function to check whether a role or user has the **CREATE** permission on a schema.                                                                                         | For example, grant the permission to user **jim** to access objects created by user **lily** in the future. |
   |                                          |                                                                                                                                                                                                                                                                                            |                                                                                                             |
   |                                          | ::                                                                                                                                                                                                                                                                                         | ::                                                                                                          |
   |                                          |                                                                                                                                                                                                                                                                                            |                                                                                                             |
   |                                          |    SELECT a.rolname, n.nspname FROM pg_authid as a, pg_namespace as n WHERE has_schema_privilege(a.oid, n.oid, 'CREATE');                                                                                                                                                                  |    ALTER DEFAULT PRIVILEGES FOR ROLE lily IN SCHEMA lily GRANT SELECT ON tables TO jim;                     |
   |                                          |                                                                                                                                                                                                                                                                                            |                                                                                                             |
   |                                          |                                                                                                                                                                                                                                                                                            | or                                                                                                          |
   |                                          |                                                                                                                                                                                                                                                                                            |                                                                                                             |
   |                                          |                                                                                                                                                                                                                                                                                            | ::                                                                                                          |
   |                                          |                                                                                                                                                                                                                                                                                            |                                                                                                             |
   |                                          |                                                                                                                                                                                                                                                                                            |    ALTER DEFAULT PRIVILEGES FOR USER lily IN SCHEMA lily GRANT SELECT ON tables TO jim;                     |
   +------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------+
   | schema_name                              | Name of an existing schema.                                                                                                                                                                                                                                                                | Name of an existing schema.                                                                                 |
   |                                          |                                                                                                                                                                                                                                                                                            |                                                                                                             |
   |                                          | If a schema name is specified, the default permissions of all objects created in this schema will be modified. If **IN SCHEMA** is omitted, global permissions are modified.                                                                                                               |                                                                                                             |
   |                                          |                                                                                                                                                                                                                                                                                            |                                                                                                             |
   |                                          | If the schema name is public, you need to specify **FOR USER target_role**.                                                                                                                                                                                                                |                                                                                                             |
   +------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------+
   | role_name                                | Name of an existing role whose permissions are to be granted or revoked.                                                                                                                                                                                                                   | Name of an existing role.                                                                                   |
   |                                          |                                                                                                                                                                                                                                                                                            |                                                                                                             |
   |                                          | .. note::                                                                                                                                                                                                                                                                                  |                                                                                                             |
   |                                          |                                                                                                                                                                                                                                                                                            |                                                                                                             |
   |                                          |    If you want to delete a role that is granted with default permissions, you must revoke the default permissions or use **DROP OWNED BY** to delete the default permission records of the role.                                                                                           |                                                                                                             |
   +------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------+

Examples
--------

-  **Prerequisites**: Create the test schema **tpcds**, tables, and users to set up sample data for the following examples.

   ::

      CREATE SCHEMA tpcds;
      CREATE TABLE tpcds.teama (a int,b int);
      CREATE TABLE tpcds.teamb (a int,b int);
      DROP USER IF EXISTS test1;
      DROP USER IF EXISTS test2;
      CREATE USER test1 PASSWORD '{Password}';
      CREATE USER test2 PASSWORD '{Password}';
      CREATE USER jack PASSWORD '{Password}';

-  Grant the **SELECT** permission on all the tables (and views) in **tpcds** to every user.

   ::

      ALTER DEFAULT PRIVILEGES IN SCHEMA tpcds GRANT SELECT ON TABLES TO PUBLIC;

-  Grant the **INSERT** permission on all the tables in **tpcds** to user **jack**.

   ::

      ALTER DEFAULT PRIVILEGES IN SCHEMA tpcds GRANT INSERT ON TABLES TO jack;

-  Revoke the preceding permissions.

   ::

      ALTER DEFAULT PRIVILEGES IN SCHEMA tpcds REVOKE SELECT ON TABLES FROM PUBLIC;
      ALTER DEFAULT PRIVILEGES IN SCHEMA tpcds REVOKE INSERT ON TABLES FROM jack;

-  Assume that there are two users **test1** and **test2**. If you require that user **test2** can query tables created by user **test1**, execute the following statements:

   #. Grant the schema permission of user **test1** to **test2**.

      ::

         GRANT usage, create ON SCHEMA test1 TO test2;

   #. Grant user **test2** the permission to query tables of user **test1**. **Note**: FOR USER can also be changed to FOR ROLE, but the effect is the same.

      ::

         ALTER DEFAULT PRIVILEGES FOR USER test1 IN SCHEMA test1 GRANT SELECT ON tables TO test2;

   #. Create a table as user **test1**.

      ::

         SET ROLE test1 password '{password}';
         CREATE TABLE test3( a int, b int);

   #. Enable the **test2** user to perform the query.

      ::

         SET ROLE test2 password '{password}';
         SELECT * FROM test1.test3;
          a | b
         ---+---
         (0 rows)

Helpful Links
-------------

:ref:`GRANT <dws_06_0250>`, :ref:`REVOKE <dws_06_0253>`
