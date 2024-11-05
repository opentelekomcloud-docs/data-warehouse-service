:original_name: dws_04_0828.html

.. _dws_04_0828:

PGXC_TOTAL_SCHEMA_INFO
======================

**PGXC_TOTAL_SCHEMA_INFO** displays the schema space information of all instances in the cluster, providing visibility into the schema space usage of each instance. This view can be queried only on CNs.

.. table:: **Table 1** PGXC_TOTAL_SCHEMA_INFO columns

   ============ ====== ========================
   Name         Type   Description
   ============ ====== ========================
   schemaname   text   Schema name
   schemaid     oid    Schema OID
   databasename text   Database name
   databaseid   oid    Database OID
   nodename     text   Instance name
   nodegroup    text   Name of the node group
   usedspace    bigint Size of used space
   permspace    bigint Upper limit of the space
   ============ ====== ========================
