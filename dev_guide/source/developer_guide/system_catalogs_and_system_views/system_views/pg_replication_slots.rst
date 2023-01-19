:original_name: dws_04_0744.html

.. _dws_04_0744:

PG_REPLICATION_SLOTS
====================

**PG_REPLICATION_SLOTS** displays the replication node information.

.. table:: **Table 1** PG_REPLICATION_SLOTS columns

   +---------------+---------+--------------------------------------------------------------------------------------+
   | Name          | Type    | Description                                                                          |
   +===============+=========+======================================================================================+
   | slot_name     | text    | Name of a replication node                                                           |
   +---------------+---------+--------------------------------------------------------------------------------------+
   | plugin        | name    | Name of the output plug-in of the logical replication slot                           |
   +---------------+---------+--------------------------------------------------------------------------------------+
   | slot_type     | text    | Type of a replication node                                                           |
   +---------------+---------+--------------------------------------------------------------------------------------+
   | datoid        | oid     | OID of the database on the replication node                                          |
   +---------------+---------+--------------------------------------------------------------------------------------+
   | database      | name    | Name of the database on the replication node                                         |
   +---------------+---------+--------------------------------------------------------------------------------------+
   | active        | boolean | Whether the replication node is active                                               |
   +---------------+---------+--------------------------------------------------------------------------------------+
   | xmin          | xid     | Transaction ID of the replication node                                               |
   +---------------+---------+--------------------------------------------------------------------------------------+
   | catalog_xmin  | text    | ID of the earliest-decoded transaction corresponding to the logical replication slot |
   +---------------+---------+--------------------------------------------------------------------------------------+
   | restart_lsn   | text    | Xlog file information on the replication node                                        |
   +---------------+---------+--------------------------------------------------------------------------------------+
   | dummy_standby | boolean | Whether the replication node is the dummy standby node                               |
   +---------------+---------+--------------------------------------------------------------------------------------+
