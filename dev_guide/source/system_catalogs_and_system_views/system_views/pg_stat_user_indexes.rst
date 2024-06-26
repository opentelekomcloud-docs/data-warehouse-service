:original_name: dws_04_0765.html

.. _dws_04_0765:

PG_STAT_USER_INDEXES
====================

**PG_STAT_USER_INDEXES** displays information about the index status of user-defined ordinary tables and TOAST tables.

.. table:: **Table 1** PG_STAT_USER_INDEXES columns

   +---------------+--------+-----------------------------------------------------------+
   | Name          | Type   | Description                                               |
   +===============+========+===========================================================+
   | relid         | oid    | Table OID for the index                                   |
   +---------------+--------+-----------------------------------------------------------+
   | indexrelid    | oid    | OID of this index                                         |
   +---------------+--------+-----------------------------------------------------------+
   | schemaname    | name   | Name of the schema this index is in                       |
   +---------------+--------+-----------------------------------------------------------+
   | relname       | name   | Name of the table for this index                          |
   +---------------+--------+-----------------------------------------------------------+
   | indexrelname  | name   | Name of this index                                        |
   +---------------+--------+-----------------------------------------------------------+
   | idx_scan      | bigint | Number of index scans                                     |
   +---------------+--------+-----------------------------------------------------------+
   | idx_tup_read  | bigint | Number of index entries returned by scans on this index   |
   +---------------+--------+-----------------------------------------------------------+
   | idx_tup_fetch | bigint | Number of rows that have live data fetched by index scans |
   +---------------+--------+-----------------------------------------------------------+
