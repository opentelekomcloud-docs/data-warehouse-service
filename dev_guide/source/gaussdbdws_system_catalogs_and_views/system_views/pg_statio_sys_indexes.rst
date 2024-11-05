:original_name: dws_04_0777.html

.. _dws_04_0777:

PG_STATIO_SYS_INDEXES
=====================

**PG_STATIO_SYS_INDEXES** displays the I/O status information about all system catalog indexes in the namespace.

.. table:: **Table 1** PG_STATIO_SYS_INDEXES columns

   ============= ====== =========================================
   Name          Type   Description
   ============= ====== =========================================
   relid         oid    Table OID for the index
   indexrelid    oid    OID of this index
   schemaname    name   Schema name for the index
   relname       name   Name of the table for this index
   indexrelname  name   Name of this index
   idx_blks_read bigint Number of disk blocks read from the index
   idx_blks_hit  bigint Number of buffer hits in this index
   ============= ====== =========================================
