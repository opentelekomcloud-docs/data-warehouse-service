:original_name: dws_04_0770.html

.. _dws_04_0770:

PG_STAT_XACT_ALL_TABLES
=======================

**PG_STAT_XACT_ALL_TABLES** displays the transaction status information about all ordinary tables and TOAST tables in the namespaces.

.. table:: **Table 1** PG_STAT_XACT_ALL_TABLES columns

   +---------------+--------+-------------------------------------------------------------------------+
   | Column        | Type   | Description                                                             |
   +===============+========+=========================================================================+
   | relid         | OID    | Table OID                                                               |
   +---------------+--------+-------------------------------------------------------------------------+
   | schemaname    | Name   | Schema name of the table                                                |
   +---------------+--------+-------------------------------------------------------------------------+
   | relname       | Name   | Name of a table                                                         |
   +---------------+--------+-------------------------------------------------------------------------+
   | seq_scan      | Bigint | Number of sequential scans started on the table                         |
   +---------------+--------+-------------------------------------------------------------------------+
   | seq_tup_read  | Bigint | Number of live rows fetched by sequential scans                         |
   +---------------+--------+-------------------------------------------------------------------------+
   | idx_scan      | Bigint | Number of index scans started on the table                              |
   +---------------+--------+-------------------------------------------------------------------------+
   | idx_tup_fetch | Bigint | Number of live rows fetched by index scans                              |
   +---------------+--------+-------------------------------------------------------------------------+
   | n_tup_ins     | Bigint | Number of rows inserted                                                 |
   +---------------+--------+-------------------------------------------------------------------------+
   | n_tup_upd     | Bigint | Number of rows updated                                                  |
   +---------------+--------+-------------------------------------------------------------------------+
   | n_tup_del     | Bigint | Number of rows deleted                                                  |
   +---------------+--------+-------------------------------------------------------------------------+
   | n_tup_hot_upd | Bigint | Number of rows with HOT updates (no separate index update is required). |
   +---------------+--------+-------------------------------------------------------------------------+
