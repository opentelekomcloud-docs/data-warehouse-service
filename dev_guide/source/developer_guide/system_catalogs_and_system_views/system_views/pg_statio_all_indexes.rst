:original_name: dws_04_0774.html

.. _dws_04_0774:

PG_STATIO_ALL_INDEXES
=====================

**PG_STATIO_ALL_INDEXES** contains each row of each index in the current database, showing I/O statistics about accesses to that specific index.

.. table:: **Table 1** PG_STATIO_ALL_INDEXES columns

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
