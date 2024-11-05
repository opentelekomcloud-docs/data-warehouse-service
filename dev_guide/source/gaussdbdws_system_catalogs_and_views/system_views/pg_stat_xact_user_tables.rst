:original_name: dws_04_0773.html

.. _dws_04_0773:

PG_STAT_XACT_USER_TABLES
========================

**PG_STAT_XACT_USER_TABLES** displays the transaction status information of the user table in the namespace.

.. table:: **Table 1** PG_STAT_XACT_USER_TABLES columns

   +---------------+--------+-------------------------------------------------------------------------+
   | Name          | Type   | Description                                                             |
   +===============+========+=========================================================================+
   | relid         | oid    | Table OID                                                               |
   +---------------+--------+-------------------------------------------------------------------------+
   | schemaname    | name   | Schema name of the table                                                |
   +---------------+--------+-------------------------------------------------------------------------+
   | relname       | name   | Table name                                                              |
   +---------------+--------+-------------------------------------------------------------------------+
   | seq_scan      | bigint | Number of sequential scans started on the table                         |
   +---------------+--------+-------------------------------------------------------------------------+
   | seq_tup_read  | bigint | Number of live rows fetched by sequential scans                         |
   +---------------+--------+-------------------------------------------------------------------------+
   | idx_scan      | bigint | Number of index scans started on the table                              |
   +---------------+--------+-------------------------------------------------------------------------+
   | idx_tup_fetch | bigint | Number of live rows fetched by index scans                              |
   +---------------+--------+-------------------------------------------------------------------------+
   | n_tup_ins     | bigint | Number of rows inserted                                                 |
   +---------------+--------+-------------------------------------------------------------------------+
   | n_tup_upd     | bigint | Number of rows updated                                                  |
   +---------------+--------+-------------------------------------------------------------------------+
   | n_tup_del     | bigint | Number of rows deleted                                                  |
   +---------------+--------+-------------------------------------------------------------------------+
   | n_tup_hot_upd | bigint | Number of rows with HOT updates (no separate index update is required). |
   +---------------+--------+-------------------------------------------------------------------------+
