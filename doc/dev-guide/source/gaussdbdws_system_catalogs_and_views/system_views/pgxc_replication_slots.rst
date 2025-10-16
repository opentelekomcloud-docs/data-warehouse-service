:original_name: dws_04_0817.html

.. _dws_04_0817:

PGXC_REPLICATION_SLOTS
======================

**PGXC_REPLICATION_SLOTS** displays the replication information of DNs in the cluster. All columns except **node_name** are the same as those in the :ref:`PG_REPLICATION_SLOTS <dws_04_0744>` view. This view is accessible only to users with system administrators rights.

.. table:: **Table 1** PGXC_REPLICATION_SLOTS columns

   +---------------+---------+--------------------------------------------------------------------------------------+
   | Column        | Type    | Description                                                                          |
   +===============+=========+======================================================================================+
   | node_name     | Text    | Node name                                                                            |
   +---------------+---------+--------------------------------------------------------------------------------------+
   | slot_name     | Text    | Name of a replication node                                                           |
   +---------------+---------+--------------------------------------------------------------------------------------+
   | plugin        | Name    | Name of the output plug-in of the logical replication slot                           |
   +---------------+---------+--------------------------------------------------------------------------------------+
   | slot_type     | Text    | Type of a replication node                                                           |
   +---------------+---------+--------------------------------------------------------------------------------------+
   | datoid        | OID     | OID of the database on the replication node                                          |
   +---------------+---------+--------------------------------------------------------------------------------------+
   | database      | Name    | Name of the database on the replication node                                         |
   +---------------+---------+--------------------------------------------------------------------------------------+
   | active        | boolean | Whether the replication node is active                                               |
   +---------------+---------+--------------------------------------------------------------------------------------+
   | xmin          | Xid     | Transaction ID of the replication node                                               |
   +---------------+---------+--------------------------------------------------------------------------------------+
   | catalog_xmin  | Text    | ID of the earliest-decoded transaction corresponding to the logical replication slot |
   +---------------+---------+--------------------------------------------------------------------------------------+
   | restart_lsn   | Text    | Xlog file information on the replication node                                        |
   +---------------+---------+--------------------------------------------------------------------------------------+
   | dummy_standby | boolean | Whether the replication node is the dummy standby node                               |
   +---------------+---------+--------------------------------------------------------------------------------------+
