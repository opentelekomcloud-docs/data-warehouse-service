:original_name: dws_04_0831.html

.. _dws_04_0831:

PGXC_VARIABLE_INFO
==================

**PGXC_VARIABLE_INFO** displays information about transaction IDs and OIDs of all nodes in a cluster.

.. table:: **Table 1** PGXC_VARIABLE_INFO columns

   +-----------------------+---------+------------------------------------------------------------------------------+
   | Name                  | Type    | Description                                                                  |
   +=======================+=========+==============================================================================+
   | node_name             | text    | Node name                                                                    |
   +-----------------------+---------+------------------------------------------------------------------------------+
   | nextOid               | oid     | OID generated next time for a node                                           |
   +-----------------------+---------+------------------------------------------------------------------------------+
   | nextXid               | xid     | Transaction ID generated next time for a node                                |
   +-----------------------+---------+------------------------------------------------------------------------------+
   | oldestXid             | xid     | Oldest transaction ID for a node                                             |
   +-----------------------+---------+------------------------------------------------------------------------------+
   | xidVacLimit           | xid     | Critical point that triggers forcible autovacuum                             |
   +-----------------------+---------+------------------------------------------------------------------------------+
   | oldestXidDB           | oid     | OID of the database that has the minimum **datafrozenxid** on a node         |
   +-----------------------+---------+------------------------------------------------------------------------------+
   | lastExtendCSNLogpage  | integer | Number of the last extended csnlog page                                      |
   +-----------------------+---------+------------------------------------------------------------------------------+
   | startExtendCSNLogpage | integer | Number of the page from which the csnlog extending starts                    |
   +-----------------------+---------+------------------------------------------------------------------------------+
   | nextCommitSeqNo       | integer | CSN generated next time for a node                                           |
   +-----------------------+---------+------------------------------------------------------------------------------+
   | latestCompletedXid    | xid     | Latest transaction ID on a node after the transaction commission or rollback |
   +-----------------------+---------+------------------------------------------------------------------------------+
   | startupMaxXid         | xid     | Last transaction ID before a node is powered off                             |
   +-----------------------+---------+------------------------------------------------------------------------------+
