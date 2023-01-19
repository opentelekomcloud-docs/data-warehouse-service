:original_name: dws_06_0186.html

.. _dws_06_0186:

CREATE USER
===========

Function
--------

**CREATE USER** creates a user.

Important Notes
---------------

-  A user created using the **CREATE USER** statement has the **LOGIN** permission by default.
-  A schema named after the user is automatically created in the database where the statement is executed, but not in other databases. You can run the **CREATE SCHEMA** statement to create such a schema for the user in other databases.
-  The owner of an object created by a system administrator in a schema with the same name as a common user is the common user, not the system administrator.
-  Users other than system administrators cannot create objects in a schema named after a user, unless the users are granted with the role permissions of that schema. For details, see **After the all Permission Is Granted to the Schema of a User, the Error Message "ERROR: current user does not have privilege to role tom" Persists During Table Creation** in *Troubleshooting*.

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
       | PASSWORD EXPIRATOIN period

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
   -  Be enclosed by single or double quotation marks.

   Value range: a string

For details on other parameters, see :ref:`CREATE ROLE Parameter Description <en-us_topic_0000001145710797__sd3fd937137c548e2ae142614383082aa>`.

Example
-------

Create user **jim**.

::

   CREATE USER jim PASSWORD '{password}';

The following statements are equivalent to the above.

::

   CREATE USER kim IDENTIFIED BY '{password}';

For a user having the **Create Database** permission, add the **CREATEDB** keyword.

::

   CREATE USER dim CREATEDB PASSWORD '{password}';

Links
-----

:ref:`ALTER USER <dws_06_0149>`, :ref:`CREATE ROLE <dws_06_0172>`, :ref:`DROP USER <dws_06_0214>`
