:original_name: dws_06_0149.html

.. _dws_06_0149:

ALTER USER
==========

Function
--------

**ALTER USER** modifies the attributes of a database user.

Precautions
-----------

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
          | PASSWORD EXPIRATOIN period

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

-  **new_password**

   Indicates a new password.

   A password must:

   -  Differ from the old password.
   -  Contain at least eight characters. This is the default length.
   -  Differ from the user name or the user name spelled backwards.
   -  Contains at least three of the following four character types: uppercase letters, lowercase letters, digits, and special characters, including: ``~!@#$%^&*()-_=+\|[{}];:,<.>/?.`` If you use characters other than the four types, a warning is displayed, but you can still create the password.

   Value range: a string

-  **old_password**

   Indicates the old password.

-  **ACCOUNT LOCK \| ACCOUNT UNLOCK**

   -  **ACCOUNT LOCK**: locks an account to forbid login to databases.
   -  **ACCOUNT UNLOCK**: unlocks an account to allow login to databases.

-  **PGUSER**

   **PGUSER** of a user cannot be modified in the current version.

For details about other parameters, see "Parameter Description" in :ref:`CREATE ROLE <dws_06_0172>` and :ref:`ALTER ROLE <dws_06_0134>`.

Example
-------

Change the login password of user **jim**.

::

   ALTER USER jim IDENTIFIED BY '{password}' REPLACE '{old_password}';

Add the **CREATEROLE** permission to user **jim**.

::

   ALTER USER jim CREATEROLE;

Set **enable_seqscan** to **on** (the setting will take effect in the next session).

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
