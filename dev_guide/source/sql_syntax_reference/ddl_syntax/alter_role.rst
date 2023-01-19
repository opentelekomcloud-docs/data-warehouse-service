:original_name: dws_06_0134.html

.. _dws_06_0134:

ALTER ROLE
==========

Function
--------

**ALTER ROLE** changes the attributes of a role.

Important Notes
---------------

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
          | NODE GROUP logic_cluster_name
          | ACCOUNT { LOCK | UNLOCK }
          | PGUSER
          | AUTHINFO 'authinfo'
          | PASSWORD EXPIRATOIN period

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

-  **IN DATABASE database_name**

   Modifies the parameters of a role on a specified database.

-  **SET configuration_parameter**

   Sets parameters for a role. The session parameters modified using the **ALTER ROLE** command is only for a specific role and is valid in the next session triggered by the role.

   Valid value:

   Values of **configuration_parameter** and **value** are listed in :ref:`SET <dws_06_0220>`.

   **DEFAULT** clears the value of **configuration_parameter**. The value of the **configuration_parameter** parameter will inherit the default value of the new session generated for the role.

   **FROM CURRENT** uses the value of **configuration_parameter** of the current session.

-  **RESET configuration_parameter/ALL**

   The effect of clearing the **configuration_parameter** value is the same as setting it to **DEFAULT**.

   Value range: **ALL** indicates that all parameter values are cleared.

-  **ACCOUNT LOCK \| ACCOUNT UNLOCK**

   -  **ACCOUNT LOCK**: locks an account to forbid login to databases.
   -  **ACCOUNT UNLOCK**: unlocks an account to allow login to databases.

-  **PGUSER**

   **PGUSER** of a role cannot be modified in the current version.

For details about other parameters, see :ref:`Parameter Description <en-us_topic_0000001145710797__sd3fd937137c548e2ae142614383082aa>` in **CREATE ROLE**.

Example
-------

Change the password of role **manager**.

::

   ALTER ROLE manager IDENTIFIED BY '{password}' REPLACE '{old_password}';

Alter role **manager** to a system administrator.

::

   ALTER ROLE manager SYSADMIN;

Modify the **fulluser** information of the LDAP authentication role.

::

   ALTER ROLE role2 WITH LOGIN AUTHINFO 'ldapcn=role2,cn=user2,dc=func,dc=com' PASSWORD DISABLE;

Change the validity period of the login password of the role to 90 days.

::

   ALTER ROLE role3 PASSWORD EXPIRATION 90;

Links
-----

:ref:`CREATE ROLE <dws_06_0172>`, :ref:`DROP ROLE <dws_06_0203>`
