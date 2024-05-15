:original_name: dws_06_0253.html

.. _dws_06_0253:

REVOKE
======

Function
--------

**REVOKE** revokes rights from one or more roles.

Precautions
-----------

If a non-owner user of an object attempts to **REVOKE** rights on the object, the command is executed based on the following rules:

-  If the user has no right whatsoever on the object, the command will fail outright.
-  If some permissions are available, the command proceeds, but it revokes only those rights for which the user has grant options.
-  The **REVOKE ALL PRIVILEGES** forms will issue an error message if no grant options are held, while the other forms will issue a warning if grant options for any of the rights named in the command are not held.
-  Do not perform **REVOKE** to a table partition. Performing **REVOKE** to a partitioned table incurs an alarm.

Syntax
------

-  Revoke the permission of specified table and view.

   ::

      REVOKE [ GRANT OPTION FOR ]
          { { SELECT | INSERT | UPDATE | DELETE | TRUNCATE | REFERENCES | TRIGGER | ANALYZE | ANALYSE | VACUUM | ALTER | DROP }[, ...]
          | ALL [ PRIVILEGES ] }
          ON { [ TABLE ] table_name [, ...]
             | ALL TABLES IN SCHEMA schema_name [, ...] }
          FROM { [ GROUP ] role_name | PUBLIC } [, ...]
          [ CASCADE | RESTRICT ];

-  Revoke the permission of specified fields on the table.

   ::

      REVOKE [ GRANT OPTION FOR ]
          { {{ SELECT | INSERT | UPDATE | REFERENCES } ( column_name [, ...] )}[, ...]
          | ALL [ PRIVILEGES ] ( column_name [, ...] ) }
          ON [ TABLE ] table_name [, ...]
          FROM { [ GROUP ] role_name | PUBLIC } [, ...]
          [ CASCADE | RESTRICT ];

-  Revoke the permission of a specified database.

   ::

      REVOKE [ GRANT OPTION FOR ]
          { { CREATE | CONNECT | TEMPORARY | TEMP } [, ...]
          | ALL [ PRIVILEGES ] }
          ON DATABASE database_name [, ...]
          FROM { [ GROUP ] role_name | PUBLIC } [, ...]
          [ CASCADE | RESTRICT ];

-  Revoke the permission of a specified function.

   ::

      REVOKE [ GRANT OPTION FOR ]
          { EXECUTE | ALL [ PRIVILEGES ] }
          ON { FUNCTION {function_name ( [ {[ argmode ] [ arg_name ] arg_type} [, ...] ] )} [, ...]
             | ALL FUNCTIONS IN SCHEMA schema_name [, ...] }
          FROM { [ GROUP ] role_name | PUBLIC } [, ...]
          [ CASCADE | RESTRICT ];

-  Revoke the permission of a specified large object.

   ::

      REVOKE [ GRANT OPTION FOR ]
          { { SELECT | UPDATE } [, ...] | ALL [ PRIVILEGES ] }
          ON LARGE OBJECT loid [, ...]
          FROM { [ GROUP ] role_name | PUBLIC } [, ...]
          [ CASCADE | RESTRICT ];

-  Revoke the permission on a specified sequence.

   ::

      REVOKE [ GRANT OPTION FOR ]
          { { SELECT | UPDATE | USAGE } [, ...] | ALL [ PRIVILEGES ] }
          ON SEQUENCE sequence_name [, ...]
          FROM { [ GROUP ] role_name | PUBLIC } [, ...]
          [ CASCADE | RESTRICT ];

-  Revoke the permission of a specified schema.

   ::

      REVOKE [ GRANT OPTION FOR ]
          { { CREATE | USAGE | ALTER | DROP } [, ...] | ALL [ PRIVILEGES ] }
          ON SCHEMA schema_name [, ...]
          FROM { [ GROUP ] role_name | PUBLIC } [, ...]
          [ CASCADE | RESTRICT ];

-  Revoke the permission of a specified sub-cluster.

   ::

      REVOKE [ GRANT OPTION FOR ]
          { CREATE | USAGE | COMPUTE | ALL [ PRIVILEGES ] }
          ON NODE GROUP group_name [, ...]
          FROM { [ GROUP ] role_name | PUBLIC } [, ...]
          [ CASCADE | RESTRICT ];

-  Revoke the permission of roles based on roles.

   ::

      REVOKE [ ADMIN OPTION FOR ]
          role_name [, ...] FROM role_name [, ...]
          [ CASCADE | RESTRICT ];

-  Revoke the sysadmin permission of roles.

   ::

      REVOKE ALL { PRIVILEGES | PRIVILEGE } FROM role_name;

Parameter Description
---------------------

The keyword **PUBLIC** indicates an implicitly defined group that contains all roles.

See :ref:`Parameter Description <en-us_topic_0000001188270556__s226158f44a8f4b908e69a283aeb813cd>` of the **GRANT** command for the meaning of the privileges and related parameters.

Permissions of a role include the permissions directly granted to the role, permissions inherited from the parent role, and permissions granted to **PUBLIC**. Therefore, revoking the **SELECT** permission for an object from **PUBLIC** does not necessarily mean that the **SELECT** permission for the object has been revoked from all roles, because the **SELECT** permission directly granted to roles and inherited from parent roles still remains. Similarly, if the **SELECT** permission is revoked from a user but is not revoked from **PUBLIC**, the user can still run the **SELECT** statement.

If **GRANT OPTION FOR** is specified, only the grant option for the right is revoked, not the right itself.

If user A holds the **UPDATE** rights on a table and the **WITH GRANT OPTION** and has granted them to user B, the rights that user B holds are called dependent rights. If the rights or the grant option held by user A is revoked, the dependent rights still exist. Those dependent rights are also revoked if **CASCADE** is specified.

A user can only revoke rights that were granted directly by that user. If, for example, user A has granted a right with grant option (**WITH ADMIN OPTION**) to user B, and user B has in turned granted it to user C, then user A cannot revoke the right directly from C. However, user A can revoke the grant option held by user B and use **CASCADE**. In this manner, the rights held by user C are automatically revoked. For another example, if both user A and user B have granted the same right to C, A can revoke his own grant but not B's grant, so C will still effectively have the right.

If the role executing **REVOKE** holds rights indirectly via more than one role membership path, it is unspecified which containing role will be used to execute the command. In such cases, it is best practice to use **SET ROLE** to become the specific role you want to do the **REVOKE** as, and then execute REVOKE. Failure to do so may lead to deleting rights not intended to delete, or not deleting any rights at all.

Examples
--------

Create user **jim**:

::

   CREATE USER jim PASSWORD '{Password}';

Create a schema:

.. code-block::

   CREATE SCHEMA tpcds;

Create a database:

.. code-block::

   CREATE DATABASE mydatabase OWNER jim;

Create a table:

::

   CREATE TABLE IF NOT EXISTS tpcds.reason(r_reason_sk int,r_reason_id int,r_reason_desc int);

Create a view:

::

   CREATE VIEW myview AS select * from tpcds.reason;

Revoke all permissions of user **jim**:

::

   REVOKE ALL PRIVILEGES FROM jim;

Revoke the permissions granted in a specified schema:

::

   REVOKE USAGE,CREATE ON SCHEMA tpcds FROM jim;

Revoke the **CONNECT** privilege from user **jim**:

::

   REVOKE CONNECT ON DATABASE mydatabase FROM jim;

Revoke the membership of role **dbadmin** from user **jim**:

::

   REVOKE dbadmin FROM jim;

Revoke all the privileges of user **jim** for the **myView** view:

::

   REVOKE ALL PRIVILEGES ON myView FROM jim;

Revoke the public insert permission on the **customer_t1** table.

::

   REVOKE INSERT ON tpcds.reason FROM PUBLIC;

Revoke the query permissions for **r_reason_sk** and **r_reason_id** in the **tpcds.reason** table from user **jim**.

::

   REVOKE select (r_reason_sk, r_reason_id) ON tpcds.reason FROM jim;

Revoke a function permission from user **jim**.

::

   CREATE FUNCTION func_add_sql(integer, integer) RETURNS integer
       AS 'select $1 + $2;'
       LANGUAGE SQL
       IMMUTABLE
       RETURNS NULL ON NULL INPUT;
   REVOKE execute ON FUNCTION func_add_sql(integer, integer) FROM jim CASCADE;

Links
-----

:ref:`GRANT <dws_06_0250>`
