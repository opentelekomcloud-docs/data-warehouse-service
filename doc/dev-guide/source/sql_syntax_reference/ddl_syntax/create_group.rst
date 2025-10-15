:original_name: dws_06_0164.html

.. _dws_06_0164:

CREATE GROUP
============

Function
--------

**CREATE GROUP** creates a user group.

Precautions
-----------

**CREATE GROUP** is an alias for **CREATE ROLE**, and it is not a standard SQL command and not recommended. Users can use **CREATE ROLE** directly.

Syntax
------

::

   CREATE GROUP group_name [ [ WITH ] option [ ... ] ]
       [ ENCRYPTED | UNENCRYPTED ] { PASSWORD | IDENTIFIED BY } { 'password' | DISABLE };

The syntax of optional **action** clause is as follows:

::

   where option can be:
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
       | NODE GROUP logic_group_name
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

Parameter Description
---------------------

See :ref:`Parameters <en-us_topic_0000001811634513__sd3fd937137c548e2ae142614383082aa>` in **CREATE ROLE**.

Helpful Links
-------------

:ref:`ALTER GROUP <dws_06_0127>`, :ref:`DROP GROUP <dws_06_0194>`, :ref:`CREATE ROLE <dws_06_0172>`
