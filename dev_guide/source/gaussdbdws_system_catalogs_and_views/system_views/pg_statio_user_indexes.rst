:original_name: dws_04_0780.html

.. _dws_04_0780:

PG_STATIO_USER_INDEXES
======================

**PG_STATIO_USER_INDEXES** displays the I/O status information about all the user relationship table indexes in the namespace.

.. table:: **Table 1** PG_STATIO_USER_INDEXES columns

   ============= ====== =========================================
   Name          Type   Description
   ============= ====== =========================================
   relid         oid    OID of the table for this index
   indexrelid    oid    OID of this index
   schemaname    name   Name of the schema this index is in
   relname       name   Name of the table for this index
   indexrelname  name   Name of this index
   idx_blks_read bigint Number of disk blocks read from the index
   idx_blks_hit  bigint Number of buffer hits in this index
   ============= ====== =========================================
