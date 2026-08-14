:original_name: dws_04_0777.html

.. _dws_04_0777:

PG_STATIO_SYS_INDEXES
=====================

**PG_STATIO_SYS_INDEXES** displays the I/O status information about all system catalog indexes in the namespace.

.. table:: **Table 1** PG_STATIO_SYS_INDEXES columns

   ============= ====== ==========================================
   Column        Type   Description
   ============= ====== ==========================================
   relid         OID    Table OID for the index.
   indexrelid    OID    OID of this index.
   schemaname    Name   Name of the schema of the index.
   relname       Name   Name of the table for this index.
   indexrelname  Name   Name of this index.
   idx_blks_read Bigint Number of disk blocks read from the index.
   idx_blks_hit  Bigint Number of buffer hits in this index.
   ============= ====== ==========================================
