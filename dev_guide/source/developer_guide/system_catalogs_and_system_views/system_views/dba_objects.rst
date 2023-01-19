:original_name: dws_04_0667.html

.. _dws_04_0667:

DBA_OBJECTS
===========

**DBA_OBJECTS** displays all database objects in the database. It is accessible only to users with system administrator rights.

.. table:: **Table 1** **DBA_OBJECTS** columns

   +---------------+--------------------------+--------------------------------------------+
   | Name          | Type                     | Description                                |
   +===============+==========================+============================================+
   | owner         | name                     | Owner of the object                        |
   +---------------+--------------------------+--------------------------------------------+
   | object_name   | name                     | Object name                                |
   +---------------+--------------------------+--------------------------------------------+
   | object_id     | oid                      | OID of the object                          |
   +---------------+--------------------------+--------------------------------------------+
   | object_type   | name                     | Type of the object                         |
   +---------------+--------------------------+--------------------------------------------+
   | namespace     | oid                      | Namespace containing the object            |
   +---------------+--------------------------+--------------------------------------------+
   | created       | timestamp with time zone | Object creation time                       |
   +---------------+--------------------------+--------------------------------------------+
   | last_ddl_time | timestamp with time zone | The last time when an object was modified. |
   +---------------+--------------------------+--------------------------------------------+

.. important::

   For details about the value ranges of **last_ddl_time** and **last_ddl_time**, see :ref:`PG_OBJECT <dws_04_0601>`.
