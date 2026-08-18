:original_name: dws_04_0768.html

.. _dws_04_0768:

PG_STAT_SYS_INDEXES
===================

**PG_STAT_SYS_INDEXES** displays the index status information about all the system catalogs in the **pg_catalog** and **information_schema** schemas.

.. table:: **Table 1** PG_STAT_SYS_INDEXES columns

   +---------------+--------+------------------------------------------------------------+
   | Column        | Type   | Description                                                |
   +===============+========+============================================================+
   | relid         | OID    | Table OID for the index.                                   |
   +---------------+--------+------------------------------------------------------------+
   | indexrelid    | OID    | OID of this index.                                         |
   +---------------+--------+------------------------------------------------------------+
   | schemaname    | Name   | Name of the schema this index is in.                       |
   +---------------+--------+------------------------------------------------------------+
   | relname       | Name   | Name of the table for this index.                          |
   +---------------+--------+------------------------------------------------------------+
   | indexrelname  | Name   | Name of this index.                                        |
   +---------------+--------+------------------------------------------------------------+
   | idx_scan      | Bigint | Number of index scans.                                     |
   +---------------+--------+------------------------------------------------------------+
   | idx_tup_read  | Bigint | Number of index entries returned by scans on this index.   |
   +---------------+--------+------------------------------------------------------------+
   | idx_tup_fetch | Bigint | Number of rows that have live data fetched by index scans. |
   +---------------+--------+------------------------------------------------------------+
