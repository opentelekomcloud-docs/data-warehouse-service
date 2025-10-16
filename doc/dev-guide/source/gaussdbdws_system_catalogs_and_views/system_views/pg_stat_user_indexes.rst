:original_name: dws_04_0765.html

.. _dws_04_0765:

PG_STAT_USER_INDEXES
====================

**PG_STAT_USER_INDEXES** displays information about the index status of user-defined ordinary tables and TOAST tables.

.. table:: **Table 1** PG_STAT_USER_INDEXES columns

   +---------------+--------+-----------------------------------------------------------+
   | Column        | Type   | Description                                               |
   +===============+========+===========================================================+
   | relid         | OID    | Table OID for the index                                   |
   +---------------+--------+-----------------------------------------------------------+
   | indexrelid    | OID    | OID of this index                                         |
   +---------------+--------+-----------------------------------------------------------+
   | schemaname    | Name   | Name of the schema this index is in                       |
   +---------------+--------+-----------------------------------------------------------+
   | relname       | Name   | Name of the table for this index                          |
   +---------------+--------+-----------------------------------------------------------+
   | indexrelname  | Name   | Name of this index                                        |
   +---------------+--------+-----------------------------------------------------------+
   | idx_scan      | Bigint | Number of index scans                                     |
   +---------------+--------+-----------------------------------------------------------+
   | idx_tup_read  | Bigint | Number of index entries returned by scans on this index   |
   +---------------+--------+-----------------------------------------------------------+
   | idx_tup_fetch | Bigint | Number of rows that have live data fetched by index scans |
   +---------------+--------+-----------------------------------------------------------+
