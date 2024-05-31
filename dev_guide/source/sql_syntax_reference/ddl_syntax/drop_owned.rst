:original_name: dws_06_0198.html

.. _dws_06_0198:

DROP OWNED
==========

Function
--------

**DROP OWNED** deletes the database objects of a database role.

Precautions
-----------

The role's permissions on all the database objects in the current database and shared objects (databases and tablespaces) are revoked.

Syntax
------

::

   DROP OWNED BY name [, ...] [ CASCADE | RESTRICT ];

Parameters
----------

-  **name**

   Name of the role whose objects are to be deleted and whose permissions are to be revoked.

-  **CASCADE \| RESTRICT**

   -  **CASCADE**: automatically deletes objects that depend on the affected objects.
   -  **RESTRICT** (default): refuses to delete objects with dependent objects.

Example
-------

Remove all database objects owned by role **u1**:

::

   DROP OWNED BY u1;
