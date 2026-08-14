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

Parameter Description
---------------------

.. table:: **Table 1** DROP OWNED parameters

   +-----------------------+-----------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------+
   | Parameter             | Description                                                                                         | Value Range                                                                        |
   +=======================+=====================================================================================================+====================================================================================+
   | name                  | Name of the role whose objects are to be deleted and whose permissions are to be revoked.           | An existing role name.                                                             |
   +-----------------------+-----------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------+
   | CASCADE \| RESTRICT   | Specifies how to process related data in the dependent object when a delete operation is performed. | -  **CASCADE**: automatically deletes objects that depend on the affected objects. |
   |                       |                                                                                                     | -  **RESTRICT** (default): refuses to delete objects with dependent objects.       |
   +-----------------------+-----------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------+

Example
-------

Create role **u1** and a table owned by **u1**.

::

   CREATE ROLE u1 IDENTIFIED BY '{password}';
   CREATE TABLE u1_table (id int);
   ALTER TABLE u1_table OWNER TO u1;

Remove all database objects owned by role **u1**.

::

   DROP OWNED BY u1;
