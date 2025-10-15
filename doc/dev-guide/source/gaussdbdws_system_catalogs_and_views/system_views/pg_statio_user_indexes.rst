:original_name: dws_04_0780.html

.. _dws_04_0780:

PG_STATIO_USER_INDEXES
======================

**PG_STATIO_USER_INDEXES** displays the I/O status information about all the user relationship table indexes in the namespace.

.. table:: **Table 1** PG_STATIO_USER_INDEXES columns

   ============= ====== =========================================
   Column        Type   Description
   ============= ====== =========================================
   relid         OID    OID of the table for this index
   indexrelid    OID    OID of this index
   schemaname    Name   Name of the schema this index is in
   relname       Name   Name of the table for this index
   indexrelname  Name   Name of this index
   idx_blks_read Bigint Number of disk blocks read from the index
   idx_blks_hit  Bigint Number of buffer hits in this index
   ============= ====== =========================================
