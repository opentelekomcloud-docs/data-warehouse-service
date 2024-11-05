:original_name: dws_06_0186.html

.. _dws_06_0186:

CREATE USER
===========

Function
--------

Creates a user.

Important Notes
---------------

-  A user created using the **CREATE USER** statement has the **LOGIN** permission by default.
-  A schema named after the user is automatically created in the database where the statement is executed, but not in other databases. You can run the **CREATE SCHEMA** statement to create such a schema for the user in other databases.
-  The owner of an object created by a system administrator in a schema with the same name as a common user is the common user, not the system administrator.
-  Users other than system administrators cannot create objects in a schema named after a user, unless the users are granted with the role permissions of that schema. For details, see **After the all Permission Is Granted to the Schema of a User, the Error Message "ERROR: current user does not have privilege to role tom" Persists During Table Creation** in the *Data Warehouse Service (DWS) Troubleshooting*.

Syntax
------

::

   CREATE USER user_name [ [ WITH ] option [ ... ] ] [ ENCRYPTED | UNENCRYPTED ] { PASSWORD | IDENTIFIED BY } { 'password' | DISABLE };

The **option** clause is used for setting information including permissions and attributes.

::

   {SYSADMIN | NOSYSADMIN}
       | {AUDITADMIN | NOAUDITADMIN}
       | {CREATEDB | NOCREATEDB}
       | {USEFT | NOUSEFT}
       | {CREATEROLE | NOCREATEROLE}
       | {INHERIT | NOINHERIT}
       | {LOGIN | NOLOGIN}
       | {REPLICATION | NOREPLICATION}
       | {INDEPENDENT | NOINDEPENDENT}
       | {VCADMIN | NOVCADMIN}
       | CONNECTION LIMIT connlimit
       | VALID BEGIN 'timestamp'
       | VALID UNTIL 'timestamp'
       | RESOURCE POOL 'respool'
       | USER GROUP 'groupuser'
       | PERM SPACE 'spacelimit'
       | TEMP SPACE 'tmpspacelimit'
       | SPILL SPACE 'spillspacelimit'
       | NODE GROUP logic_cluster_name
       | IN ROLE role_name [, ...]
       | IN GROUP role_name [, ...]
       | ROLE role_name [, ...]
       | ADMIN role_name [, ...]
       | USER role_name [, ...]
       | SYSID uid
       | DEFAULT TABLESPACE tablespace_name
       | PROFILE DEFAULT
       | PROFILE profile_name
       | PGUSER
       | AUTHINFO 'authinfo'
       | PASSWORD EXPIRATION period

Parameters
----------

-  **user_name**

   Specifies the user name.

   Value range: a string. It must comply with the naming convention. A value can contain a maximum of 63 characters.

-  **password**

   Specifies the login password.

   A password must:

   -  Contain at least eight characters. This is the default length.
   -  Differ from the user name or the user name spelled backwards.
   -  Contains at least three of the following four character types: uppercase letters, lowercase letters, digits, and special characters, including: ``~!@#$%^&*()-_=+\|[{}];:,<.>/?.`` If you use characters other than the four types, a warning is displayed, but you can still create the password.

   Value range: a string

-  **DISABLE**

   By default, you can change your password unless it is disabled. Use this parameter to disable the password of a user. After the password of a user is disabled, the password will be deleted from the system. The user can connect to the database only through external authentication, for example, IAM authentication, Kerberos authentication, or LDAP authentication. Only administrators can enable or disable a password. Common users cannot disable the password of an initial user. To enable a password, run **ALTER USER** and specify the password.

-  **ENCRYPTED \| UNENCRYPTED**

   Determines whether the password stored in the system will be encrypted. (If neither is specified, the password status is determined by **password_encryption_type**.) According to product security requirements, the password must be stored encrypted. Therefore, **UNENCRYPTED** is forbidden in GaussDB(DWS). If the password is SHA256-encrypted, it will be stored as-is, regardless of whether **ENCRYPTED** or **UNENCRYPTED** is specified (since the system cannot decrypt the specified encrypted password). This allows reloading of the encrypted password during dump/restore.

-  **SYSADMIN \| NOSYSADMIN**

   Determines whether a new user is a system administrator. A user with the **SYSADMIN** attribute has the highest permission in the system.

   Value range: If not specified, **NOSYSADMIN** is the default.

-  **AUDITADMIN \| NOAUDITADMIN**

   Defines whether a user has the audit administrator attribute.

   If not specified, **NOAUDITADMIN** is the default.

-  **CREATEDB \| NOCREATEDB**

   Determines whether a new user can create a database.

   A new user does not have the permission to create a database by default.

   Value range: If not specified, **NOCREATEDB** is the default.

-  **USEFT \| NOUSEFT**

   Determines whether a new role can perform operations on foreign tables, such as creating, deleting, modifying, and reading/witting foreign tables.

   The new user does not have the permission to perform operations on foreign tables.

   The default value is **NOUSEFT**.

-  **CREATEROLE \| NOCREATEROLE**

   Determines whether a user can create a role or user (that is, execute CREATE ROLE and CREATE USER). A user with the CREATEROLE permission can also modify and delete other users or roles.

   Value range: If not specified, **NOCREATEROLE** is the default.

-  **INHERIT \| NOINHERIT**

   Determines whether a user "inherits" the permissions of users in its group. You are not advised to execute them.

-  **LOGIN \| NOLOGIN**

   Only users with the **LOGIN** attribute can log in to the database.

   Value range: If not specified, **NOLOGIN** is the default.

-  **REPLICATION \| NOREPLICATION**

   Determines whether a user is allowed to initiate streaming replication or put the system in and out of backup mode. A user with the REPLICATION attribute is only used for replication.

   If not specified, **NOREPLICATION** is the default.

-  **INDEPENDENT \| NOINDEPENDENT**

   Defines private and independent users. For a user with the **INDEPENDENT** attribute, administrators' rights to control and access this role are separated. Specific rules are as follows:

   -  Administrators have no rights to add, delete, query, modify, copy, or authorize the corresponding table objects without the authorization from the **INDEPENDENT** user.
   -  Without the authorization of the **INDEPENDENT** user, the administrator has no right to modify its inheritance relationship.
   -  The administrator does not have the permission to change the owner of the table object of an **INDEPENDENT** user.
   -  The administrator does not have the permission to remove the **INDEPENDENT** attribute of an **INDEPENDENT** user.
   -  The administrator does not have the permission to change the database password of an **INDEPENDENT** user. An **INDEPENDENT** must manage its own password. If the password is lost, it cannot be reset.
   -  The **SYSADMIN** attribute of a user cannot be changed to the **INDEPENDENT** attribute.

-  **VCADMIN \| NOVCADMIN**

   Defines a logical cluster administrator. A logical cluster administrator has the following more permissions than common users:

   -  Create, modify, and delete resource pools in the associated logical cluster.
   -  Grant the access permission for the associated logical cluster to other users or roles, or reclaim the access permission from those users or roles.

-  **CONNECTION LIMIT**

   Specifies the number of concurrent connections that can be used by a user on a single CN.

   Value range: Integer, **>=-1**. The default value is **-1**, which means unlimited.

   .. important::

      To ensure the proper running of a cluster, the minimum value of **CONNECTION LIMIT** is the number of CNs in the cluster, because when a cluster runs ANALYZE on a CN, other CNs will connect with the running CN for metadata synchronization. For example, if there are three CNs in the cluster, set **CONNECTION LIMIT** to **3** or a larger value.

-  **VALID BEGIN**

   Sets the timestamp when a user takes effect. If this clause is omitted, there is no restriction on when the user takes effect.

-  **VALID UNTIL**

   Sets the timestamp when a user expires. If this clause is omitted, there is no restriction on when the user expires.

-  **RESOURCE POOL**

   Sets the name of resource pool used by a user, and the name belongs to the system catalog: **pg_resource_pool**.

-  **USER GROUP 'groupuser'**

   Creates a sub-user.

-  **PERM SPACE**

   Sets the storage space of the user permanent table.

   **space_limit**: specifies the upper limit of the storage space of the permanent table. Value range: A string consists of an integer and unit. The unit can be K/M/G/T/P currently. **0** indicates no limits.

-  **TEMP SPACE**

   Sets the storage space of the user temporary table.

   **tmpspacelimit**: specifies the storage space limit of the temporary table. Value range: A string consists of an integer and unit. The unit can be K/M/G/T/P currently. **0** indicates no limits.

-  **SPILL SPACE**

   Sets the operator disk flushing space of the user.

   **spillspacelimit**: specifies the operator spilling space limit. Value range: A string consists of an integer and unit. The unit can be K/M/G/T/P currently. **0** indicates no limits.

-  **NODE GROUP**

   Specifies the name of the logical cluster associated with a user. If the name contains uppercase characters or special characters, enclose the name with double quotation marks.

-  **IN ROLE**

   The new user immediately has the permissions of users listed in the **IN ROLE** clause. You are not advised to execute them.

-  **IN GROUP**

   Indicates an obsolete spelling of **IN ROLE**. You are not advised to execute them.

-  **ROLE**

   The **ROLE** clause lists one or more existing users. They are automatically added as members of the new user and have all the permissions of the new user.

-  **ADMIN**

   The ADMIN clause is similar to the ROLE clause. The difference is that the user after **ADMIN** can grant the permissions of the new user to other users.

-  **USER**

   Indicates an obsolete spelling of the **ROLE** clause.

-  **SYSID**

   The **SYSID** clause is ignored.

-  **DEFAULT TABLESPACE**

   The **DEFAULT TABLESPACE** clause is ignored.

-  **PROFILE**

   The **PROFILE** clause is ignored.

-  **PGUSER**

   This attribute is used to be compatible with open-source Postgres communication. An open-source Postgres client interface (Postgres 9.2.19 is recommended) can use a database user having this attribute to connect to the database.

   .. important::

      This attribute only ensures compatibility with the connection process. Incompatibility caused by kernel differences between this product and Postgres cannot be solved using this attribute.

      Users having the **PGUSER** attribute are authenticated in a way different from other users. Error information reported by the open-source client may cause the attribute to be enumerated. Therefore, you are advised to use a client of this product. Example:

      ::

         # normaluser is a user that does not have the PGUSER attribute. psql is the Postgres client tool.
         pg@dws04:~> psql -d postgres -p 8000 -h 10.11.12.13 -U normaluser
         psql: authentication method 10 not supported

         # pguser is a user having the PGUSER attribute.
         pg@dws04:~> psql -d postgres -p 8000 -h 10.11.12.13 -U pguser
         Password for user pguser:

-  **AUTHINFO 'authinfo'**

   This attribute is used to specify the user authentication type. **authinfo** is the description character string, which is case sensitive. Only the LDAP type is supported. Its description character string is **ldap**. LDAP authentication is an external authentication mode. Therefore, **PASSWORD DISABLE** must be specified.

   .. important::

      -  Additional information about LDAP authentication can be added to **authinfo**, for example, **fulluser** in LDAP authentication, which is equivalent to **ldapprefix**\ +\ **username**\ +\ **ldapsuffix**. If the content of **authinfo** is **ldap**, the user authentication type is LDAP. In this case, the **ldapprefix** and **ldapsuffix** information is provided by the corresponding record in the **pg_hba.conf** file.
      -  When executing the **ALTER ROLE** command, users are not allowed to change the authentication type. Only LDAP users are allowed to modify LDAP attributes.

-  **PASSWORD EXPIRATION period**

   Number of days before the login password of the role expires. The user needs to change the password in time before the login password expires. If the login password expires, the user cannot log in to the system. In this case, the user needs to ask the administrator to set a new login password.

   Value range: an integer ranging from -1 to 999. The default value is **-1**, indicating that there is no restriction. The value **0** indicates that the login password expires immediately.

Example
-------

Create user **jim**:

::

   CREATE USER jim PASSWORD '{password}';

The following statements are equivalent to the above:

::

   CREATE USER kim IDENTIFIED BY '{password}';

For a user having the **Create Database** permission, add the **CREATEDB** keyword:

::

   CREATE USER dim CREATEDB PASSWORD '{password}';

Links
-----

:ref:`ALTER USER <dws_06_0149>`, :ref:`CREATE ROLE <dws_06_0172>`, :ref:`DROP USER <dws_06_0214>`
