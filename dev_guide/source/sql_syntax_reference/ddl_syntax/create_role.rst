:original_name: dws_06_0172.html

.. _dws_06_0172:

CREATE ROLE
===========

Function
--------

Create a role.

A role is an entity that has own database objects and permissions. In different environments, a role can be considered a user, a group, or both.

Important Notes
---------------

-  **CREATE ROLE** adds a role to a database. The role does not have the login permission.
-  Only the user who has the **CREATE ROLE** permission or a system administrator is allowed to create roles.

Syntax
------

::

   CREATE ROLE role_name [ [ WITH ] option [ ... ] ] [ ENCRYPTED | UNENCRYPTED ] { PASSWORD | IDENTIFIED BY } { 'password' | DISABLE };

The syntax of role information configuration clause **option** is as follows:

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
       | ADMIN rol e_name [, ...]
       | USER role_name [, ...]
       | SYSID uid
       | DEFAULT TABLESPACE tablespace_name
       | PROFILE DEFAULT
       | PROFILE profile_name
       | PGUSER
       | AUTHINFO 'authinfo'
       | PASSWORD EXPIRATION period

.. _en-us_topic_0000001811634513__sd3fd937137c548e2ae142614383082aa:

Parameters
----------

-  **role_name**

   Role name

   Value range: a string. It must comply with the naming convention. and can contain a maximum of 63 characters.

-  **password**

   Specifies the login password.

   A password must:

   -  Contain at least eight characters. This is the default length.
   -  Differ from the user name or the user name spelled backwards.
   -  Contains at least three of the following four character types: uppercase letters, lowercase letters, digits, and special characters, including: ``~!@#$%^&*()-_=+\|[{}];:,<.>/?.`` If you use characters other than the four types, a warning is displayed, but you can still create the password.

   Value range: a string

-  **DISABLE**

   By default, you can change your password unless it is disabled. Use this parameter to disable the password of a user. After the password of a user is disabled, the password will be deleted from the system. The user can connect to the database only through external authentication, for example, IAM, Kerberos, LDAP, or oneaccess. Only administrators can enable or disable a password. Common users cannot disable the password of an initial user. To enable a password, run **ALTER USER** and specify the password.

-  **ENCRYPTED \| UNENCRYPTED**

   Determines whether the password stored in the system will be encrypted. (If neither is specified, the password status is determined by **password_encryption_type**.) According to product security requirements, the password must be stored encrypted. Therefore, **UNENCRYPTED** is forbidden in GaussDB(DWS). If the password is encrypted using SHA256, it will be stored as it is, regardless of whether it is specified as **ENCRYPTED** or **UNENCRYPTED**. This is because the system cannot decrypt the specified encrypted password. This allows reloading of the encrypted password during dump/restore.

-  **SYSADMIN \| NOSYSADMIN**

   Determines whether a new role is a system administrator. Roles having the **SYSADMIN** attribute have the highest permission.

   Value range: If not specified, **NOSYSADMIN** is the default.

-  **AUDITADMIN \| NOAUDITADMIN**

   Determines whether a role has the audit and management attributes.

   If not specified, **NOAUDITADMIN** is the default.

-  **CREATEDB \| NOCREATEDB**

   Defines a role's ability to create databases.

   A new role does not have the permission to create databases.

   Value range: If not specified, **NOCREATEDB** is the default.

-  **USEFT \| NOUSEFT**

   Determines whether a new role can perform operations on foreign tables, such as creating, deleting, modifying, and reading/witting foreign tables.

   A new role does not have permissions for these operations.

   The default value is **NOUSEFT**.

   .. note::

      In cluster 8.2.0 or later, if **foreign_table_options** of the GUC parameter **security_enable_options** is enabled in security mode, foreign table operations are permitted and the **useft** permission does not need to be granted to users.

-  **CREATEROLE \| NOCREATEROLE**

   Determines whether a role will be permitted to create new roles (that is, execute **CREATE ROLE** and **CREATE USER**). A role with the **CREATEROLE** permission can also modify and delete other roles.

   Value range: If not specified, **NOCREATEROLE** is the default.

-  **INHERIT \| NOINHERIT**

   Determines whether a role "inherits" the permissions of roles it is a member of. You are not advised to execute them.

-  **LOGIN \| NOLOGIN**

   Determines whether a role is allowed to log in to a database. A role having the **LOGIN** attribute can be thought of as a user.

   Value range: If not specified, **NOLOGIN** is the default.

-  **REPLICATION \| NOREPLICATION**

   Determines whether a role is allowed to initiate streaming replication or put the system in and out of backup mode. A role having the **REPLICATION** attribute is a highly privileged role, and should only be used on roles used for replication.

   If not specified, **NOREPLICATION** is the default.

-  **INDEPENDENT \| NOINDEPENDENT**

   Defines private, independent roles. For a role with the **INDEPENDENT** attribute, administrators' rights to control and access this role are separated. Specific rules are as follows:

   -  Administrators have no rights to add, delete, query, modify, copy, or authorize the corresponding table objects without the authorization from the INDEPENDENT role.
   -  Administrators have no rights to modify the inheritance relationship of the INDEPENDENT role without the authorization from this role.
   -  Administrators have no rights to modify the owner of the table objects for the INDEPENDENT role.
   -  Administrators have no rights to delete the INDEPENDENT attribute of the INDEPENDENT role.
   -  Administrators have no rights to change the database password of the INDEPENDENT role. The INDEPENDENT role must manage its own password, which cannot be reset if lost.
   -  The **SYSADMIN** attribute of a user cannot be changed to the **INDEPENDENT** attribute.

-  **VCADMIN \| NOVCADMIN**

   Defines the role of a logical cluster administrator. A logical cluster administrator has the following more permissions than common users:

   -  Create, modify, and delete resource pools in the associated logical cluster.
   -  Grant the access permission for the associated logical cluster to other users or roles, or reclaim the access permission from those users or roles.

-  **CONNECTION LIMIT**

   Indicates how many concurrent connections the role can use on a single CN.

   Value range: Integer, **>=-1**. The default value is **-1**, which means unlimited.

   .. important::

      To ensure the proper running of a cluster, the minimum value of **CONNECTION LIMIT** is the number of CNs in the cluster, because when a cluster runs **ANALYZE** on a CN, other CNs will connect to the running CN for metadata synchronization. For example, if there are three CNs in the cluster, set **CONNECTION LIMIT** to **3** or a larger value.

-  **VALID BEGIN**

   Sets a date and time when the role's password becomes valid. If this clause is omitted, the password will be valid for all time.

-  **VALID UNTIL**

   Sets a date and time after which the role's password is no longer valid. If this clause is omitted, the password will be valid for all time.

-  **RESOURCE POOL**

   Sets the name of resource pool used by the role, and the name belongs to the system catalog: **pg_resource_pool**.

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

   Lists one or more existing roles whose permissions will be inherited by a new role. You are not advised to execute them.

-  **IN GROUP**

   Indicates an obsolete spelling of **IN ROLE**. You are not advised to execute them.

-  **ROLE**

   Lists one or more existing roles which are automatically added as members of the new role.

-  **ADMIN**

   Is similar to **ROLE**. However, the roles after **ADMIN** can grant rights of new roles to other roles.

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

   This attribute is used to specify the role authentication type. **authinfo** is the description character string, which is case sensitive. Currently, only the LDAP and OneAcess type is supported. Its description character string is **ldap** and **oneaccess**. LDAP and **OneAcess** are external authentication modes. Therefore, **PASSWORD DISABLE** must be specified.

   .. important::

      -  Additional information about LDAP authentication can be added to **authinfo**, for example, **fulluser** in LDAP authentication, which is equivalent to **ldapprefix**\ +\ **username**\ +\ **ldapsuffix**. If the content of **authinfo** is **ldap**, the role authentication type is LDAP. In this case, the **ldapprefix** and **ldapsuffix** information is provided by the corresponding record in the **pg_hba.conf** file.
      -  When the OneAccess authentication mode is supported, authinfo must contain **oneaccessClientId** and **domain** information in the '**oneaccessClientId=**\ *xxxx*, **domain=**\ *xxxx*' format. **oneaccessClientId** indicates the ID of the oneaccess user and contains a maximum of 256 characters. **domain** indicates the access domain name of the oneaccess user and contains a maximum of 64 characters.
      -  When executing the **ALTER ROLE** command, users are not allowed to change the authentication type. Only LDAP or OneAccess users are allowed to modify the LDAP or OneAccess attributes.
      -  OneAccess is identified by internal code logic and does not need to be configured in **pg_hba.conf**. The authentication information is provided by **authinfo**.

-  **PASSWORD EXPIRATION period**

   Number of days before the login password of the role expires. The user needs to change the password in time before the login password expires. If the login password expires, the user cannot log in to the system. In this case, the user needs to ask the administrator to set a new login password.

   Value range: an integer ranging from -1 to 999. The default value is **-1**, indicating that there is no restriction. The value **0** indicates that the login password expires immediately.

Examples
--------

Create a role named **manager**:

::

   CREATE ROLE manager IDENTIFIED BY '{password}';

Create a role with a validity period from January 1, 2015 to January 1, 2026:

::

   CREATE ROLE miriam WITH LOGIN PASSWORD '{password}' VALID BEGIN '2015-01-01' VALID UNTIL '2026-01-01';

-- Create a role. The authentication type is LDAP. Other LDAP authentication information is provided by **pg_hba.conf**:

::

   CREATE ROLE role1 WITH LOGIN AUTHINFO 'ldap' PASSWORD DISABLE;

-- Create a role. The authentication type is LDAP. The **fulluser** information for LDAP authentication is specified during the role creation. In this case, LDAP is case sensitive and must be enclosed in single quotation marks:

::

   CREATE ROLE role2 WITH LOGIN AUTHINFO 'ldapcn=role2,cn=user,dc=lework,dc=com' PASSWORD DISABLE;

Create a role and set the authentication type to **OneAccess**. The authinfo information of OneAccess authentication is specified when the role is created. In this case, **oneaccess** is case sensitive and must be enclosed in single quotation marks.

::

   CREATE ROLE role3 WITH LOGIN AUTHINFO 'oneaccessClientId=AbCd123,domain=example.exampleoneaccess.com' PASSWORD DISABLE;

-- Create a role and set the validity period of its login password to 30 days:

::

   CREATE ROLE role4 WITH LOGIN PASSWORD '{password}' PASSWORD EXPIRATION 30;

Links
-----

:ref:`SET ROLE <dws_06_0222>`, :ref:`ALTER ROLE <dws_06_0134>`, :ref:`DROP ROLE <dws_06_0203>`, :ref:`GRANT <dws_06_0250>`, :ref:`REVOKE <dws_06_0253>`
