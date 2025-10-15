:original_name: dws_04_0601.html

.. _dws_04_0601:

PG_OBJECT
=========

**PG_OBJECT** records the user creation, creation time, last modification time, and last analyzing time of objects of specified types (types existing in **object_type**).

.. table:: **Table 1** PG_OBJECT columns

   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
   | Column                | Type                     | Description                                                                                                                                         |
   +=======================+==========================+=====================================================================================================================================================+
   | object_oid            | OID                      | Object identifier.                                                                                                                                  |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
   | object_type           | Char                     | Object type:                                                                                                                                        |
   |                       |                          |                                                                                                                                                     |
   |                       |                          | -  **r** indicates a table, which can be an ordinary table or a temporary table.                                                                    |
   |                       |                          | -  **i** indicates an index.                                                                                                                        |
   |                       |                          | -  **s** indicates a sequence.                                                                                                                      |
   |                       |                          | -  **v** indicates a view.                                                                                                                          |
   |                       |                          | -  **p** indicates a stored procedure and function.                                                                                                 |
   |                       |                          | -  **f** indicates a foreign table.                                                                                                                 |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
   | creator               | OID                      | ID of the creator.                                                                                                                                  |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
   | ctime                 | Timestamp with time zone | Object creation time.                                                                                                                               |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
   | mtime                 | Timestamp with time zone | Time when the object was last modified. By default, the **ALTER**, **COMMENT**, **GRANT/REVOKE**, and **TRUNCATE** operations are recorded.         |
   |                       |                          |                                                                                                                                                     |
   |                       |                          | **object_mtime_record_mode** can be used to control whether **ALTER**, **COMMENT**, **GRANT**/**REVOKE**, and **TRUNCATE** operations are recorded. |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
   | last_analyze_time     | Timestamp with time zone | Time when an object is analyzed for the last time.                                                                                                  |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+

.. important::

   -  Only normal user operations are recorded. Operations before the object upgrade and during the **initdb** process cannot be recorded.
   -  **ctime** and **mtime** are the start time of the transaction.
   -  The time of object modification due to capacity expansion is also recorded.
