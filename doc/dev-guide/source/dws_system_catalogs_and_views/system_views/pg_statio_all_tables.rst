:original_name: dws_04_0776.html

.. _dws_04_0776:

PG_STATIO_ALL_TABLES
====================

**PG_STATIO_ALL_TABLES** displays I/O statistics about all tables (including TOAST tables) in the current database.

.. table:: **Table 1** PG_STATIO_ALL_TABLES columns

   +-----------------+--------+------------------------------------------------------------------------------+
   | Column          | Type   | Description                                                                  |
   +=================+========+==============================================================================+
   | relid           | OID    | Table OID                                                                    |
   +-----------------+--------+------------------------------------------------------------------------------+
   | schemaname      | Name   | Schema name of the table                                                     |
   +-----------------+--------+------------------------------------------------------------------------------+
   | relname         | Name   | Name of a table                                                              |
   +-----------------+--------+------------------------------------------------------------------------------+
   | heap_blks_read  | Bigint | Number of disks read from this table                                         |
   +-----------------+--------+------------------------------------------------------------------------------+
   | heap_blks_hit   | Bigint | Number of buffer hits in this table                                          |
   +-----------------+--------+------------------------------------------------------------------------------+
   | idx_blks_read   | Bigint | Number of disk blocks read from the index in this table                      |
   +-----------------+--------+------------------------------------------------------------------------------+
   | idx_blks_hit    | Bigint | Number of buffer hits in all indexes on this table                           |
   +-----------------+--------+------------------------------------------------------------------------------+
   | toast_blks_read | Bigint | Number of disk blocks read from the TOAST table (if any) in this table       |
   +-----------------+--------+------------------------------------------------------------------------------+
   | toast_blks_hit  | Bigint | Number of buffer hits in the TOAST table (if any) in this table              |
   +-----------------+--------+------------------------------------------------------------------------------+
   | tidx_blks_read  | Bigint | Number of disk blocks read from the TOAST table index (if any) in this table |
   +-----------------+--------+------------------------------------------------------------------------------+
   | tidx_blks_hit   | Bigint | Number of buffer hits in the TOAST table index (if any) in this table        |
   +-----------------+--------+------------------------------------------------------------------------------+
