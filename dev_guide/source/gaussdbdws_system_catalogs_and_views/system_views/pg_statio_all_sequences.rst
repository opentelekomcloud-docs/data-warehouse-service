:original_name: dws_04_0775.html

.. _dws_04_0775:

PG_STATIO_ALL_SEQUENCES
=======================

**PG_STATIO_ALL_SEQUENCES** displays the sequence information in the current database and the I/O statistics of a specified sequence.

.. table:: **Table 1** PG_STATIO_ALL_SEQUENCES columns

   ========== ====== ============================================
   Name       Type   Description
   ========== ====== ============================================
   relid      oid    OID of this sequence
   schemaname name   Name of the schema this sequence is in
   relname    name   Name of this sequence
   blks_read  bigint Number of disk blocks read from the sequence
   blks_hit   bigint Number of buffer hits in this sequence
   ========== ====== ============================================
