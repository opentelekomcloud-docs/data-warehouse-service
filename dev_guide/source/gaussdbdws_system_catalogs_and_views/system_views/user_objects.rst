:original_name: dws_04_0867.html

.. _dws_04_0867:

USER_OBJECTS
============

**USER_OBJECTS** displays all database objects accessible to the current user.

+---------------+--------------------------+----------------------------------------+
| Column        | Type                     | Description                            |
+===============+==========================+========================================+
| owner         | Name                     | Owner of the object                    |
+---------------+--------------------------+----------------------------------------+
| object_name   | Name                     | Object name                            |
+---------------+--------------------------+----------------------------------------+
| object_id     | OID                      | OID of the object                      |
+---------------+--------------------------+----------------------------------------+
| object_type   | Name                     | Type of the object                     |
+---------------+--------------------------+----------------------------------------+
| namespace     | OID                      | Namespace containing the object        |
+---------------+--------------------------+----------------------------------------+
| created       | Timestamp with time zone | Object creation time                   |
+---------------+--------------------------+----------------------------------------+
| last_ddl_time | Timestamp with time zone | Last time when the object was modified |
+---------------+--------------------------+----------------------------------------+

.. important::

   For details about the value ranges of **last_ddl_time** and **last_ddl_time**, see :ref:`PG_OBJECT <dws_04_0601>`.
