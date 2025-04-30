:original_name: dws_04_0789.html

.. _dws_04_0789:

PG_TOTAL_SCHEMA_INFO
====================

PG_TOTAL_SCHEMA_INFO displays the storage usage of all schemas in each database. This view is valid only if use_workload_manager is set to **on**.

+--------------+--------+---------------------------------------------------------------------------+
| Column       | Type   | Description                                                               |
+==============+========+===========================================================================+
| schemaid     | OID    | Schema OID                                                                |
+--------------+--------+---------------------------------------------------------------------------+
| schemaname   | Text   | Schema name                                                               |
+--------------+--------+---------------------------------------------------------------------------+
| databaseid   | OID    | Database OID                                                              |
+--------------+--------+---------------------------------------------------------------------------+
| databasename | Name   | Database name                                                             |
+--------------+--------+---------------------------------------------------------------------------+
| usedspace    | Bigint | Size of the permanent table storage space used by the schema, in bytes.   |
+--------------+--------+---------------------------------------------------------------------------+
| permspace    | Bigint | Upper limit of the permanent table storage space of the schema, in bytes. |
+--------------+--------+---------------------------------------------------------------------------+
