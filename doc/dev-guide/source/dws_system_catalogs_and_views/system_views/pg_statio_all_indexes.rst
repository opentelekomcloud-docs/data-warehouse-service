:original_name: dws_04_0774.html

.. _dws_04_0774:

PG_STATIO_ALL_INDEXES
=====================

**PG_STATIO_ALL_INDEXES** displays I/O statistics of all indexes in the current database.

.. table:: **Table 1** PG_STATIO_ALL_INDEXES columns

   ============= ====== =========================================
   Column        Type   Description
   ============= ====== =========================================
   relid         OID    OID of the index table
   indexrelid    OID    OID of this index
   schemaname    Name   Name of the schema this index is in
   relname       Name   Name of the table for this index
   indexrelname  Name   Name of this index
   idx_blks_read Bigint Number of disk blocks read from the index
   idx_blks_hit  Bigint Number of buffer hits in this index
   ============= ====== =========================================
