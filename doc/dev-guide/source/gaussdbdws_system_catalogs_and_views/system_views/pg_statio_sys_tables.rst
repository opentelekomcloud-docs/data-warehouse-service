:original_name: dws_04_0779.html

.. _dws_04_0779:

PG_STATIO_SYS_TABLES
====================

**PG_STATIO_SYS_TABLES** displays the I/O status information about all the system catalogs in the namespace.

.. table:: **Table 1** PG_STATIO_SYS_TABLES columns

   +-----------------+--------+------------------------------------------------------------------------------+
   | Column          | Type   | Description                                                                  |
   +=================+========+==============================================================================+
   | relid           | OID    | Table OID                                                                    |
   +-----------------+--------+------------------------------------------------------------------------------+
   | schemaname      | Name   | Schema name of the table                                                     |
   +-----------------+--------+------------------------------------------------------------------------------+
   | relname         | Name   | Name of a table                                                              |
   +-----------------+--------+------------------------------------------------------------------------------+
   | heap_blks_read  | Bigint | Number of disk blocks read from this table                                   |
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
