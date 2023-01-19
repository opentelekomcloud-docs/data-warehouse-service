:original_name: dws_04_0776.html

.. _dws_04_0776:

PG_STATIO_ALL_TABLES
====================

**PG_STATIO_ALL_TABLES** contains one row for each table in the current database (including TOAST tables), showing I/O statistics about accesses to that specific table.

.. table:: **Table 1** PG_STATIO_ALL_TABLES columns

   +-----------------+--------+------------------------------------------------------------------------------+
   | Name            | Type   | Description                                                                  |
   +=================+========+==============================================================================+
   | relid           | oid    | Table OID                                                                    |
   +-----------------+--------+------------------------------------------------------------------------------+
   | schemaname      | name   | Schema name of the table                                                     |
   +-----------------+--------+------------------------------------------------------------------------------+
   | relname         | name   | Table name                                                                   |
   +-----------------+--------+------------------------------------------------------------------------------+
   | heap_blks_read  | bigint | Number of disk blocks read from this table                                   |
   +-----------------+--------+------------------------------------------------------------------------------+
   | heap_blks_hit   | bigint | Number of buffer hits in this table                                          |
   +-----------------+--------+------------------------------------------------------------------------------+
   | idx_blks_read   | bigint | Number of disk blocks read from the index in this table                      |
   +-----------------+--------+------------------------------------------------------------------------------+
   | idx_blks_hit    | bigint | Number of buffer hits in all indexes on this table                           |
   +-----------------+--------+------------------------------------------------------------------------------+
   | toast_blks_read | bigint | Number of disk blocks read from the TOAST table (if any) in this table       |
   +-----------------+--------+------------------------------------------------------------------------------+
   | toast_blks_hit  | bigint | Number of buffer hits in the TOAST table (if any) in this table              |
   +-----------------+--------+------------------------------------------------------------------------------+
   | tidx_blks_read  | bigint | Number of disk blocks read from the TOAST table index (if any) in this table |
   +-----------------+--------+------------------------------------------------------------------------------+
   | tidx_blks_hit   | bigint | Number of buffer hits in the TOAST table index (if any) in this table        |
   +-----------------+--------+------------------------------------------------------------------------------+
