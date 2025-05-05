:original_name: dws_04_0831.html

.. _dws_04_0831:

PGXC_VARIABLE_INFO
==================

**PGXC_VARIABLE_INFO** displays information about XIDs and OIDs of all nodes in a cluster.

.. table:: **Table 1** PGXC_VARIABLE_INFO columns

   +-----------------------+---------+------------------------------------------------------------------+
   | Column                | Type    | Description                                                      |
   +=======================+=========+==================================================================+
   | node_name             | Text    | Node name.                                                       |
   +-----------------------+---------+------------------------------------------------------------------+
   | nextOid               | OID     | Next OID to be generated under this node.                        |
   +-----------------------+---------+------------------------------------------------------------------+
   | nextXid               | Xid     | Next transaction OID to be generated under this node.            |
   +-----------------------+---------+------------------------------------------------------------------+
   | oldestXid             | Xid     | Oldest transaction ID for a node                                 |
   +-----------------------+---------+------------------------------------------------------------------+
   | xidVacLimit           | Xid     | Critical point for forcing autovacuum.                           |
   +-----------------------+---------+------------------------------------------------------------------+
   | oldestXidDB           | OID     | Database OID with the minimum **datafrozenxid** under this node. |
   +-----------------------+---------+------------------------------------------------------------------+
   | lastExtendCSNLogpage  | Integer | Page number of the last extension of csnlog.                     |
   +-----------------------+---------+------------------------------------------------------------------+
   | startExtendCSNLogpage | Integer | Starting page number of the csnlog extension.                    |
   +-----------------------+---------+------------------------------------------------------------------+
   | nextCommitSeqNo       | Integer | Next CSN to be generated under this node.                        |
   +-----------------------+---------+------------------------------------------------------------------+
   | latestCompletedXid    | Xid     | Latest transaction ID on the node after commit or rollback.      |
   +-----------------------+---------+------------------------------------------------------------------+
   | startupMaxXid         | Xid     | Last transaction ID before the node shutdown.                    |
   +-----------------------+---------+------------------------------------------------------------------+
