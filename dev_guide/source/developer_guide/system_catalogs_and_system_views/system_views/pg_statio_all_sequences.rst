:original_name: dws_04_0775.html

.. _dws_04_0775:

PG_STATIO_ALL_SEQUENCES
=======================

**PG_STATIO_ALL_SEQUENCES** contains each row of each sequence in the current database, showing I/O statistics about accesses to that specific sequence.

.. table:: **Table 1** PG_STATIO_ALL_SEQUENCES columns

   ========== ====== =============================================
   Name       Type   Description
   ========== ====== =============================================
   relid      oid    OID of this sequence
   schemaname name   Name of the schema this sequence is in
   relname    name   Name of the sequence
   blks_read  bigint Number of disk blocks read from this sequence
   blks_hit   bigint Number of buffer hits in this sequence
   ========== ====== =============================================
