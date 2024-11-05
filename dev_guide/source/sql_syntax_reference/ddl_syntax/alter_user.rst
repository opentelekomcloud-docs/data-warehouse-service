:original_name: dws_06_0149.html

.. _dws_06_0149:

ALTER USER
==========

Function
--------

Modifies the attributes of a database user.

Important Notes
---------------

Session parameters modified by **ALTER USER** apply to a specified user and take effect in the next session.

Syntax
------

-  Modify user rights or other information.

   ::

      ALTER USER user_name [ [ WITH ] option [ ... ] ];

   The **option** clause is as follows:

   ::

      { CREATEDB | NOCREATEDB }
          | { CREATEROLE | NOCREATEROLE }
          | { INHERIT | NOINHERIT }
          | { AUDITADMIN | NOAUDITADMIN }
          | { SYSADMIN | NOSYSADMIN }
          | { USEFT | NOUSEFT }
          | { LOGIN | NOLOGIN }
          | { REPLICATION | NOREPLICATION }
          | {INDEPENDENT | NOINDEPENDENT}
          | {VCADMIN | NOVCADMIN}
          | CONNECTION LIMIT connlimit
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

-  Change the user name.

   ::

      ALTER USER user_name
          RENAME TO new_name;

-  Change the value of a specified parameter associated with the user.

   ::

      ALTER USER user_name
          SET configuration_parameter { { TO | = } { value | DEFAULT } | FROM CURRENT };

-  Reset the value of a specified parameter associated with the user.

   ::

      ALTER USER user_name
          RESET { configuration_parameter | ALL };

Parameters
----------

-  **user_name**

   Specifies the current user name.

   Value range: an existing user name

-  **new_name**

   Specifies the new name of a user.

   Value range: a string. It must comply with the naming convention and can contain a maximum of 63 characters.

-  **CREATEDB \| NOCREATEDB**

   Determines whether a new user can create a database.

   A new user does not have the permission to create a database by default.

   Value range: If not specified, **NOCREATEDB** is the default.

-  **CREATEROLE \| NOCREATEROLE**

   Determines whether a user can create new users or roles (execute **CREATE ROLE** and **CREATE USER**). A user with the **CREATEROLE** permission can also modify and delete other users.

   Value range: If not specified, **NOCREATEROLE** is the default.

-  **INHERIT \| NOINHERIT**

   Determines whether a user inherits the permissions of its group. You are not advised to execute them.

-  **AUDITADMIN \| NOAUDITADMIN**

   Defines whether a user has the audit administrator attribute.

   If not specified, **NOAUDITADMIN** is the default.

-  **SYSADMIN \| NOSYSADMIN**

   Determines whether a new user is a system administrator. A user with the **SYSADMIN** attribute has the highest permission in the system.

   Value range: If not specified, **NOSYSADMIN** is the default.

-  **USEFT \| NOUSEFT**

   Determines whether a new role can perform operations on foreign tables, such as creating, deleting, modifying, and reading/witting foreign tables.

   The new user does not have the permission to perform operations on foreign tables.

   The default value is **NOUSEFT**, indicating that new users cannot perform operations on foreign tables by default.

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

-  **ENCRYPTED \| UNENCRYPTED**

   Determines whether the password stored in the system will be encrypted. (If neither is specified, the password status is determined by **password_encryption_type**.) According to product security requirements, the password must be stored encrypted. Therefore, **UNENCRYPTED** is forbidden in GaussDB(DWS). If the password is SHA256-encrypted, it will be stored as-is, regardless of whether **ENCRYPTED** or **UNENCRYPTED** is specified (since the system cannot decrypt the specified encrypted password). This allows reloading of the encrypted password during dump/restore.

   -  password

      Specifies the login password.

      The password must contain at least eight characters by default and cannot be the same as the username or the username spelled backwards. The password must contain at least three of the four types of characters: uppercase letters (A-Z), lowercase letters (a-z), digits (0-9), and non-alphanumeric characters (``~!@#$ %^&*()-_=+\|[{}];:,<.>/?``) If you use characters other than the four types, a warning is displayed, but you can still create the password.

      Value range: a string

   -  DISABLE

      By default, you can change your password unless it is disabled. Use this parameter to disable the password of a user. After the password of a user is disabled, the password will be deleted from the system. The user can connect to the database only through external authentication, for example, IAM authentication, Kerberos authentication, or LDAP authentication. Only administrators can enable or disable a password. Common users cannot disable the password of an initial user. To enable a password, run **ALTER USER** and specify the password.

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

-  **ACCOUNT LOCK \| ACCOUNT UNLOCK**

   -  **ACCOUNT LOCK**: locks an account to forbid login to databases.
   -  **ACCOUNT UNLOCK**: unlocks an account to allow login to databases.

-  **PGUSER**

   This attribute is used to be compatible with open-source Postgres communication. An open-source Postgres client interface (Postgres 9.2.19 is recommended) can use a database user having this attribute to connect to the database.

   **PGUSER** of a user cannot be modified in the current version.

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

Change the login password of user **jim**.

::

   ALTER USER jim IDENTIFIED BY '{password}' REPLACE '{old_password}';

Add the **CREATEROLE** permission to user **jim**.

::

   ALTER USER jim CREATEROLE;

Set the **enable_seqscan** parameter associated with user **jim** to **on**. The setting takes effect in the next session.

::

   ALTER USER jim SET enable_seqscan TO on;

Reset the **enable_seqscan** parameter for user **jim**.

::

   ALTER USER jim RESET enable_seqscan;

Lock the **jim** account.

::

   ALTER USER jim ACCOUNT LOCK;

Links
-----

:ref:`CREATE ROLE <dws_06_0172>`, :ref:`CREATE USER <dws_06_0186>`, :ref:`DROP USER <dws_06_0214>`
