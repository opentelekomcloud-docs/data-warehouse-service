:original_name: dws_04_0828.html

.. _dws_04_0828:

PGXC_TOTAL_SCHEMA_INFO
======================

**PGXC_TOTAL_SCHEMA_INFO** displays the schema space information of all instances in the cluster, providing visibility into the schema space usage of each instance. This view can be queried only on CNs.

.. table:: **Table 1** PGXC_TOTAL_SCHEMA_INFO columns

   ============ ====== ==================
   Column       Type   Description
   ============ ====== ==================
   schemaname   Text   Schema name.
   schemaid     OID    Schema OID.
   databasename Text   Database name.
   databaseid   OID    Database OID.
   nodename     Text   Instance name.
   nodegroup    Text   Node group name.
   usedspace    Bigint Used space size.
   permspace    Bigint Space upper limit.
   ============ ====== ==================
