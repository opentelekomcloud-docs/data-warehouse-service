:original_name: dws_04_0757.html

.. _dws_04_0757:

PG_STAT_ALL_INDEXES
===================

**PG_STAT_ALL_INDEXES** displays statistics about all accesses to a specific index in the current database.

Indexes can be used via either simple index scans or "bitmap" index scans. Bitmap scans can combine the output of multiple indexes using AND or OR rules, but combining independent row fetching with specific indexes is challenging. Consequently, a bitmap scan increases the index count in **pg_stat_all_indexes.idx_tup_read** and the table count in **pg_stat_all_tables.idx_tup_fetch**, while having no effect on **pg_stat_all_indexes.idx_tup_fetch**.

.. table:: **Table 1** PG_STAT_ALL_INDEXES columns

   +---------------+--------+---------------------------------------------------------------------------+
   | Column        | Type   | Description                                                               |
   +===============+========+===========================================================================+
   | relid         | OID    | OID of the table for this index.                                          |
   +---------------+--------+---------------------------------------------------------------------------+
   | indexrelid    | OID    | OID of this index.                                                        |
   +---------------+--------+---------------------------------------------------------------------------+
   | schemaname    | Name   | Name of the schema this index is in.                                      |
   +---------------+--------+---------------------------------------------------------------------------+
   | relname       | Name   | Name of the table for this index.                                         |
   +---------------+--------+---------------------------------------------------------------------------+
   | indexrelname  | Name   | Name of this index.                                                       |
   +---------------+--------+---------------------------------------------------------------------------+
   | idx_scan      | Bigint | Number of index scans initiated on this index.                            |
   +---------------+--------+---------------------------------------------------------------------------+
   | idx_tup_read  | Bigint | Number of index entries returned by scans on this index.                  |
   +---------------+--------+---------------------------------------------------------------------------+
   | idx_tup_fetch | Bigint | Number of live table rows fetched by simple index scans using this index. |
   +---------------+--------+---------------------------------------------------------------------------+
