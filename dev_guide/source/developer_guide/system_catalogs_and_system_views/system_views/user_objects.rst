:original_name: dws_04_0867.html

.. _dws_04_0867:

USER_OBJECTS
============

**USER_OBJECTS** displays all database objects accessible to the current user.

.. table:: **Table 1** USER_OBJECTS columns

   +---------------+--------------------------+----------------------------------------------------------------------+
   | Name          | Type                     | Description                                                          |
   +===============+==========================+======================================================================+
   | object_name   | name                     | Object name                                                          |
   +---------------+--------------------------+----------------------------------------------------------------------+
   | object_id     | oid                      | OID of the object                                                    |
   +---------------+--------------------------+----------------------------------------------------------------------+
   | object_type   | name                     | Type of the object (**TABLE**, **INDEX**, **SEQUENCE**, or **VIEW**) |
   +---------------+--------------------------+----------------------------------------------------------------------+
   | namespace     | oid                      | Namespace that the object belongs to                                 |
   +---------------+--------------------------+----------------------------------------------------------------------+
   | created       | timestamp with time zone | Object creation time                                                 |
   +---------------+--------------------------+----------------------------------------------------------------------+
   | last_ddl_time | timestamp with time zone | The last time when an object was modified.                           |
   +---------------+--------------------------+----------------------------------------------------------------------+

.. important::

   For details about the value ranges of **last_ddl_time** and **last_ddl_time**, see :ref:`PG_OBJECT <dws_04_0601>`.
