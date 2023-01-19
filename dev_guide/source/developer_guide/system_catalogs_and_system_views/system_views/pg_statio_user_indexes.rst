:original_name: dws_04_0780.html

.. _dws_04_0780:

PG_STATIO_USER_INDEXES
======================

**PG_STATIO_USER_INDEXES** displays the I/O status information about all the user relationship table indexes in the namespace.

.. table:: **Table 1** PG_STATIO_USER_INDEXES columns

   ============= ====== ==========================================
   Name          Type   Description
   ============= ====== ==========================================
   relid         oid    Table OID for the index
   indexrelid    oid    Index OID
   schemaname    name   Schema name for the index
   relname       name   Table name for the index
   indexrelname  name   Index name
   idx_blks_read bigint Number of disk blocks read from this index
   idx_blks_hit  bigint Number of buffer hits in this index
   ============= ====== ==========================================
