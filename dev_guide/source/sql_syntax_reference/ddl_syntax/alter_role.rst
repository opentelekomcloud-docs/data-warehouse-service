:original_name: dws_06_0134.html

.. _dws_06_0134:

ALTER ROLE
==========

Function
--------

**ALTER ROLE** changes the attributes of a role.

Precautions
-----------

None

Syntax
------

-  Modifying the Rights of a Role

   ::

      ALTER ROLE role_name [ [ WITH ] option [ ... ] ];

   The **option** clause for granting rights is as follows:

   ::

      {CREATEDB | NOCREATEDB}
          | {CREATEROLE | NOCREATEROLE}
          | {INHERIT | NOINHERIT}
          | {AUDITADMIN | NOAUDITADMIN}
          | {SYSADMIN | NOSYSADMIN}
          | {USEFT | NOUSEFT}
          | {LOGIN | NOLOGIN}
          | {REPLICATION | NOREPLICATION}
          | {INDEPENDENT | NOINDEPENDENT}
          | {VCADMIN | NOVCADMIN}
          | CONNECTION LIMIT connlimit
          | [ ENCRYPTED | UNENCRYPTED ] PASSWORD 'password'
          | [ ENCRYPTED | UNENCRYPTED ] IDENTIFIED BY 'password' [ REPLACE 'old_password' ]
          | [ ENCRYPTED | UNENCRYPTED ] PASSWORD { 'password' | DISABLE }
          | [ ENCRYPTED | UNENCRYPTED ] IDENTIFIED BY { 'password' [ REPLACE 'old_password' ] | DISABLE }
          | VALID BEGIN 'timestamp'
          | VALID UNTIL 'timestamp'
          | RESOURCE POOL 'respool'
          | USER GROUP 'groupuser'
          | PERM SPACE 'spacelimit'
          | TEMP SPACE 'tmpspacelimit'
          | SPILL SPACE 'spillspacelimit'
          | NODE GROUP logic_cluster_name
          | ACCOUNT { LOCK | UNLOCK }
          | PGUSER
          | AUTHINFO 'authinfo'
          | PASSWORD EXPIRATION period

-  Rename a role.

   ::

      ALTER ROLE role_name
          RENAME TO new_name;

-  Set parameters for a role.

   ::

      ALTER ROLE role_name [ IN DATABASE database_name ]
          SET configuration_parameter {{ TO | = } { value | DEFAULT } | FROM CURRENT};

-  Reset parameters for a role.

   ::

      ALTER ROLE role_name
          [ IN DATABASE database_name ] RESET {configuration_parameter|ALL};

Parameters
----------

-  **role_name**

   Indicates a role name.

   Value range: an existing user name

-  **new_name**

   Indicates the new name of a role.

   Value range: a string. It must comply with the naming convention and can contain a maximum of 63 characters.

-  **CREATEDB \| NOCREATEDB**

   Defines a role's ability to create databases.

   A new role does not have the permission to create databases.

   Value range: If not specified, **NOCREATEDB** is the default.

-  **CREATEROLE \| NOCREATEROLE**

   Determines whether a role will be permitted to create new roles (that is, execute **CREATE ROLE** and **CREATE USER**). A role with the **CREATEROLE** permission can also modify and delete other roles.

   Value range: If not specified, **NOCREATEROLE** is the default.

-  **INHERIT \| NOINHERIT**

   Determines whether the role can inherit permissions of its group. You are not advised to use them.

-  **AUDITADMIN \| NOAUDITADMIN**

   Determines whether a role has the audit and management attributes.

   If not specified, **NOAUDITADMIN** is the default.

-  **SYSADMIN \| NOSYSADMIN**

   Determines whether a new role is a system administrator. Roles having the **SYSADMIN** attribute have the highest permission.

   Value range: If not specified, **NOSYSADMIN** is the default.

-  **USEFT \| NOUSEFT**

   Determines whether a new role can perform operations on foreign tables, such as creating, deleting, modifying, and reading/witting foreign tables.

   A new role does not have permissions for these operations.

   The default value is **NOUSEFT**, indicating that a new role cannot perform operations on foreign tables by default.

-  **LOGIN \| NOLOGIN**

   Determines whether a role is allowed to log in to a database. A role having the **LOGIN** attribute can be thought of as a user.

   Value range: If not specified, **NOLOGIN** is the default.

-  **REPLICATION \| NOREPLICATION**

   Determines whether a role is allowed to initiate streaming replication or put the system in and out of backup mode. A role having the **REPLICATION** attribute is a highly privileged role, and should only be used on roles used for replication.

   If not specified, **NOREPLICATION** is used by default.

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

   Indicates how many concurrent connections the role can make.

   Value range: Integer, >= -1. The default value is **-1**, which means unlimited.

   .. important::

      To ensure the proper running of a cluster, the minimum value of **CONNECTION LIMIT** is the number of CNs in the cluster, because when a cluster runs ANALYZE on a CN, other CNs will connect to the running CN for metadata synchronization. For example, if there are three CNs in the cluster, set **CONNECTION LIMIT** to **3** or a greater value.

-  **ENCRYPTED \| UNENCRYPTED**

   Determines whether the password stored in the system will be encrypted. (If neither is specified, the password status is determined by **password_encryption_type**.) According to product security requirements, the password must be stored encrypted. Therefore, **UNENCRYPTED** is forbidden in GaussDB(DWS). If the password is SHA256-encrypted, it will be stored as-is, regardless of whether **ENCRYPTED** or **UNENCRYPTED** is specified (since the system cannot decrypt the specified encrypted password). This allows reloading of the encrypted password during dump/restore.

   -  password

      Specifies the login password.

      The password must contain at least eight characters by default and cannot be the same as the username or the username spelled backwards. The password must contain at least three of the four types of characters: uppercase letters (A-Z), lowercase letters (a-z), digits (0-9), and non-alphanumeric characters (``~!@#$ %^&*()-_=+\|[{}];:,<.>/?``) If you use characters other than the four types, a warning is displayed, but you can still create the password.

      Value range: a string

   -  DISABLE

      By default, you can change your password unless it is disabled. Use this parameter to disable the password of a user. After the password of a user is disabled, the password will be deleted from the system. The user can connect to the database only through external authentication, for example, IAM authentication, Kerberos authentication, or LDAP authentication. Only administrators can enable or disable a password. Common users cannot disable the password of an initial user. To enable a password, run **ALTER USER** and specify the password.

-  **VALID BEGIN**

   Sets a date and time when the role's password becomes valid. If this clause is omitted, the password will be valid for all time.

-  **VALID UNTIL**

   Sets a date and time after which the role's password is no longer valid. If this clause is omitted, the password will be valid for all time.

-  **RESOURCE POOL**

   Sets the name of resource pool used by the role, and the name belongs to the system catalog: **pg_resource_pool**.

-  **USER GROUP 'groupuser'**

   Creates a sub-user.

-  **PERM SPACE**

   Sets the storage space of a user permanent table.

   **space_limit**: specifies the upper limit of the storage space of the permanent table. Value range: A string consists of an integer and unit. The unit can be K/M/G/T/P. **0** indicates no limits.

-  **TEMP SPACE**

   Sets the storage space of a user temporary table.

   **tmpspacelimit**: specifies the storage space limit of the temporary table. Value range: A string consists of an integer and unit. The unit can be K/M/G/T/P currently. **0** indicates no limits.

-  **SPILL SPACE**

   Sets the space limit for operator spilling.

   **spillspacelimit**: specifies the operator spilling space limit. Value range: A string consists of an integer and unit. The unit can be K/M/G/T/P. **0** indicates no limits.

-  **NODE GROUP**

   Specifies the name of the logical cluster associated with a user. If the name contains uppercase characters or special characters, enclose the name with double quotation marks.

-  **ACCOUNT LOCK \| ACCOUNT UNLOCK**

   -  **ACCOUNT LOCK**: locks an account to forbid login to databases.
   -  **ACCOUNT UNLOCK**: unlocks an account to allow login to databases.

-  **PGUSER**

   This attribute is used to be compatible with open-source Postgres communication. An open-source Postgres client interface (Postgres 9.2.19 is recommended) can use a database user having this attribute to connect to the database.

   **PGUSER** of a role cannot be modified in the current version.

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

   This attribute is used to specify the role authentication type. **authinfo** is the description character string, which is case sensitive. Only the LDAP type is supported. Its description character string is **ldap**. LDAP authentication is an external authentication mode. Therefore, **PASSWORD DISABLE** must be specified.

   .. important::

      -  Additional information about LDAP authentication can be added to **authinfo**, for example, **fulluser** in LDAP authentication, which is equivalent to **ldapprefix**\ +\ **username**\ +\ **ldapsuffix**. If the content of **authinfo** is **ldap**, the role authentication type is LDAP. In this case, the **ldapprefix** and **ldapsuffix** information is provided by the corresponding record in the **pg_hba.conf** file.
      -  When executing the **ALTER ROLE** command, users are not allowed to change the authentication type. Only LDAP users are allowed to modify LDAP attributes.

-  **PASSWORD EXPIRATION period**

   Number of days before the login password of the role expires. The user needs to change the password in time before the login password expires. If the login password expires, the user cannot log in to the system. In this case, the user needs to ask the administrator to set a new login password.

   Value range: an integer ranging from -1 to 999. The default value is **-1**, indicating that there is no restriction. The value **0** indicates that the login password expires immediately.

-  **IN DATABASE database_name**

   Modifies the parameters of a role on a specified database.

-  **SET configuration_parameter**

   Sets parameters for a role. The session parameters modified using the **ALTER ROLE** command is only for a specific role and is valid in the next session triggered by the role.

   Valid value:

   Values of **configuration_parameter** and **value** are listed in :ref:`SET <dws_06_0220>`.

   **DEFAULT** clears the value of **configuration_parameter**. The value of the **configuration_parameter** parameter will inherit the default value of the new session generated for the role.

   **FROM CURRENT** uses the value of **configuration_parameter** of the current session.

-  **RESET configuration_parameter|ALL**

   The effect of clearing the **configuration_parameter** value is the same as setting it to **DEFAULT**.

   Value range: **ALL** indicates that all parameter values are cleared.

Example
-------

Create example roles **r1**, **r2**, and **r3**:

::

   CREATE ROLE r1 IDENTIFIED BY '{Password}';
   CREATE ROLE r2 WITH LOGIN AUTHINFO 'ldapcn=r2,cn=user,dc=lework,dc=com' PASSWORD DISABLE;
   CREATE ROLE r3 WITH LOGIN PASSWORD '{Password}' PASSWORD EXPIRATION 30;

Modify the login permission of role **r1**:

::

   ALTER ROLE r1 login;

Change the password of role **r1**:

::

   ALTER ROLE r1 IDENTIFIED BY '{new_Password}' REPLACE '{Password}';

Alter role **manager** to the system administrator:

::

   ALTER ROLE r1 SYSADMIN;

Modify the **fulluser** information of the LDAP authentication role:

::

   ALTER ROLE r2 WITH LOGIN AUTHINFO 'ldapcn=role2,cn=user2,dc=func,dc=com' PASSWORD DISABLE;

Change the validity period of the login password of the role to 90 days:

::

   ALTER ROLE r3 PASSWORD EXPIRATION 90;

Links
-----

:ref:`CREATE ROLE <dws_06_0172>`, :ref:`DROP ROLE <dws_06_0203>`, :ref:`SET <dws_06_0220>`
