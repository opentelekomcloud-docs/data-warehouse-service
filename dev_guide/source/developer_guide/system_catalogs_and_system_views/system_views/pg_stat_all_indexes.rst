:original_name: dws_04_0757.html

.. _dws_04_0757:

PG_STAT_ALL_INDEXES
===================

**PG_STAT_ALL_INDEXES** displays access informaton about all indexes in the database, with information about each index displayed in a row.

Indexes can be used via either simple index scans or "bitmap" index scans. In a bitmap scan the output of several indexes can be combined via AND or OR rules, so it is difficult to associate individual heap row fetches with specific indexes when a bitmap scan is used. Therefore, a bitmap scan increments the **pg_stat_all_indexes.idx_tup_read** count(s) for the index(es) it uses, and it increments the **pg_stat_all_tables.idx_tup_fetch** count for the table, but it does not affect **pg_stat_all_indexes.idx_tup_fetch**.

.. table:: **Table 1** PG_STAT_ALL_INDEXES columns

   +---------------+--------+--------------------------------------------------------------------------+
   | Name          | Type   | Description                                                              |
   +===============+========+==========================================================================+
   | relid         | oid    | OID of the table for this index                                          |
   +---------------+--------+--------------------------------------------------------------------------+
   | indexrelid    | oid    | OID of this index                                                        |
   +---------------+--------+--------------------------------------------------------------------------+
   | schemaname    | name   | Name of the schema this index is in                                      |
   +---------------+--------+--------------------------------------------------------------------------+
   | relname       | name   | Name of the table for this index                                         |
   +---------------+--------+--------------------------------------------------------------------------+
   | indexrelname  | name   | Name of this index                                                       |
   +---------------+--------+--------------------------------------------------------------------------+
   | idx_scan      | bigint | Number of index scans initiated on this index                            |
   +---------------+--------+--------------------------------------------------------------------------+
   | idx_tup_read  | bigint | Number of index entries returned by scans on this index                  |
   +---------------+--------+--------------------------------------------------------------------------+
   | idx_tup_fetch | bigint | Number of live table rows fetched by simple index scans using this index |
   +---------------+--------+--------------------------------------------------------------------------+
