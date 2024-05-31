:original_name: dws_04_0054.html

.. _dws_04_0054:

Permissions Management
======================

Permission Overview
-------------------

**Permissions** are used to control whether a user is allowed to access a database object (including schemas, tables, functions, and sequences) to perform operations such as adding, deleting, modifying, querying, and creating a database object.

Permission management in GaussDB(DWS) falls into three categories:

-  System permissions

   System permissions are also called user attributes, including **SYSADMIN**, **CREATEDB**, **CREATEROLE**, **AUDITADMIN**, and **LOGIN**.

   They can be specified only by the **CREATE ROLE** or **ALTER ROLE** syntax. The **SYSADMIN** permission can be granted and revoked using **GRANT ALL PRIVILEGE** and **REVOKE ALL PRIVILEGE**, respectively. System permissions cannot be inherited by a user from a role, and cannot be granted using **PUBLIC**.

-  Object permissions

   Permissions on a database object (table, view, column, database, function, schema, or tablespace) can be granted to a role or user. The **GRANT** command can be used to grant permissions to a user or role. These permissions granted are added to the existing ones.

-  Permissions

   Grant a role's or user's permissions to one or more roles or users. In this case, every role or user can be regarded as a set of one or more database permissions.

   If **WITH ADMIN OPTION** is specified, the member can in turn grant permissions in the role to others, and revoke permissions in the role as well. If a role or user granted with certain permissions is changed or revoked, the permissions inherited from the role or user also change.

   A database administrator can grant permissions to and revoke them from any role or user. Roles having **CREATEROLE** permission can grant or revoke membership in any role that is not an administrator.

Hierarchical Permission Management
----------------------------------

GaussDB(DWS) implements a hierarchical permission management on databases, schemas, and data objects.

-  Databases cannot communicate with each other and share very few resources. Their connections and permissions can be isolated. The database cluster has one or more named databases. Users and roles are shared within the entire cluster, but their data is not shared. That is, a user can connect to any database, but after the connection is successful, any user can access only the database declared in the connection request.
-  Schemas share more resources than databases do. User permissions on schemas and subordinate objects can be flexibly configured using the **GRANT** and **REVOKE** syntax. Each database has one or more schemas. Each schema contains various types of objects, such as tables, views, and functions. To access an object contained in a specified schema, a user must have the **USAGE** permission on the schema.
-  After an object is created, by default, only the object owner or system administrator can query, modify, and delete the object. To access a specific database object, for example, **table1**, other users must be granted the **CONNECT** permission of database, the **USAGE** permission of schema, and the **SELECT** permission of **table1**. To access an object at the bottom layer, a user must be granted the permission on the object at the upper layer. To create or delete a schema, you must have the **CREATE** permission on its database.


.. figure:: /_static/images/en-us_image_0000001526705437.png
   :alt: **Figure 1** Hierarchical Permission Management

   **Figure 1** Hierarchical Permission Management

Roles
-----

The permission management model of GaussDB(DWS) is a typical implementation of the role-based permission control (RBAC). It manages users, roles, and permissions through this model.

A role is a set of permissions.

-  The concept of "user" is equivalent to that of "role". The only difference is that "user" has the **login** permission while "role" has the **nologin** permission.
-  Roles are assigned with different permissions based on their responsibilities in the database system. A role is a set of database permissions and represents the behavior constraints of a database user or a group of data users.
-  Roles and users can be converted. You can use **ALTER** to assign the **login** permission to a role.
-  After a role is granted to a user through **GRANT**, the user will have all the permissions of the role. It is recommended that roles be used to efficiently grant permissions. For example, you can create different roles of design, development, and maintenance personnel, grant the roles to users, and then grant specific data permissions required by different users. When permissions are granted or revoked at the role level, these permission changes take effect for all the members of the role.
-  In non-separation-of-duty scenarios, a role can be created, modified, and deleted only by a system administrator or a user with the **CREATEROLE** attribute. In separation-of-duty scenarios, a role can be created, modified, and deleted only by a user with the **CREATEROLE** attribute.

To view all roles, query the system catalog **PG_ROLES**.

::

   SELECT * FROM PG_ROLES;

For details about how to create, modify, and delete a role, see **CREARE ROLE**/**ALTER ROLE**/**DROP ROLE** in *SQL Syntax Reference*.

Preset Roles
------------

GaussDB(DWS) provides a group of preset roles. Their names start with **gs_role\_**. These roles allow access to operations that require high permissions. You can grant these roles to other users or roles in the database for them to access or use specific information and functions. Exercise caution and ensure security when using preset roles.

The following table describes the permissions of preset roles.

.. table:: **Table 1** Permissions of preset roles

   +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Role                              | Permission                                                                                                                                                                                                                                         |
   +===================================+====================================================================================================================================================================================================================================================+
   | gs_role_signal_backend            | Invokes functions such as **pg_cancel_backend**, **pg_terminate_backend**, **pg_terminate_query**, **pg_cancel_query**, **pgxc_terminate_query**, and **pgxc_cancel_query** to cancel or terminate sessions, excluding those of the initial users. |
   +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | gs_role_read_all_stats            | Reads the system status view and uses various extension-related statistics, including information that is usually visible only to system administrators. For example:                                                                              |
   |                                   |                                                                                                                                                                                                                                                    |
   |                                   | Resource management views:                                                                                                                                                                                                                         |
   |                                   |                                                                                                                                                                                                                                                    |
   |                                   | -  pgxc_wlm_operator_history                                                                                                                                                                                                                       |
   |                                   | -  pgxc_wlm_operator_info                                                                                                                                                                                                                          |
   |                                   | -  pgxc_wlm_operator_statistics                                                                                                                                                                                                                    |
   |                                   | -  pgxc_wlm_session_info                                                                                                                                                                                                                           |
   |                                   | -  pgxc_wlm_session_statistics                                                                                                                                                                                                                     |
   |                                   | -  pgxc_wlm_workload_records                                                                                                                                                                                                                       |
   |                                   | -  pgxc_workload_sql_count                                                                                                                                                                                                                         |
   |                                   | -  pgxc_workload_sql_elapse_time                                                                                                                                                                                                                   |
   |                                   | -  pgxc_workload_transaction                                                                                                                                                                                                                       |
   |                                   |                                                                                                                                                                                                                                                    |
   |                                   | Status information views:                                                                                                                                                                                                                          |
   |                                   |                                                                                                                                                                                                                                                    |
   |                                   | -  pgxc_stat_activity                                                                                                                                                                                                                              |
   |                                   | -  pgxc_get_table_skewness                                                                                                                                                                                                                         |
   |                                   | -  table_distribution                                                                                                                                                                                                                              |
   |                                   | -  pgxc_total_memory_detail                                                                                                                                                                                                                        |
   |                                   | -  pgxc_os_run_info                                                                                                                                                                                                                                |
   |                                   | -  pg_nodes_memory                                                                                                                                                                                                                                 |
   |                                   | -  pgxc_instance_time                                                                                                                                                                                                                              |
   |                                   | -  pgxc_redo_stat                                                                                                                                                                                                                                  |
   +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | gs_role_analyze_any               | A user with the system-level **ANALYZE** permission can skip the schema permission check and perform **ANALYZE** on all tables.                                                                                                                    |
   +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | gs_role_vacuum_any                | A user with the system-level **VACUUM** permission can skip the schema permission check and perform **ANALYZE** on all tables.                                                                                                                     |
   +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

**Restrictions on using preset roles:**

-  **gs_role\_** is the name field dedicated to preset roles in the database. Do not create users or roles starting with **gs_role\_** or rename existing users or roles starting with **gs_role\_**.

-  Do not perform **ALTER** or **DROP** operations on preset roles.

-  By default, a preset role does not have the **LOGIN** permission, so there is no preset login password for the role.

-  The gsql meta-commands **\\du** and **\\dg** do not display information about preset roles. However, if **PATTERN** is specified, information about preset roles will be displayed.

-  If the separation of permissions is disabled, the system administrator and users with the **ADMIN OPTION** permission of preset roles are allowed to perform GRANT and REVOKE operations on preset roles. If the separation of permissions is enabled, the security administrator (with the **CREATEROLE** attribute) and users with the **ADMIN OPTION** permission of preset roles are allowed to perform GRANT and REVOKE operations on preset roles. Example:

   ::

      GRANT gs_role_signal_backend TO user1;
      REVOKE gs_role_signal_backend FROM user1;

Granting or Revoking Permissions
--------------------------------

A user who creates an object is the owner of this object. By default, :ref:`Separation of Permissions <dws_04_0056>` is disabled after cluster installation. A database system administrator has the same permissions as object owners.

After an object is created, only the object owner or system administrator can query, modify, and delete the object, and grant permissions for the object to other users through **GRANT** by default. To enable a user to use an object, the object owner or administrator can run the **GRANT** or **REVOKE** command to grant permissions to or revoke permissions from the user or role.

-  Run the **GRANT** statement to grant permissions.

   For example, grant the permission of schema **myschema** to role **u1**, and grant the **SELECT** permission of table **myschema.t1** to role **u1**.

   ::

      GRANT USAGE ON SCHEMA myschema TO u1;
      GRANT SELECT ON TABLE myschema.t1 to u1;

-  Run the **REVOKE** command to revoke a permission that has been granted.

   For example, revoke all permissions of user **u1** on the **myschema.t1** table.

   .. code-block::

      REVOKE ALL PRIVILEGES ON myschema.t1 FROM u1;
