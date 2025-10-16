:original_name: dws_04_0775.html

.. _dws_04_0775:

PG_STATIO_ALL_SEQUENCES
=======================

**PG_STATIO_ALL_SEQUENCES** displays the sequence information in the current database and the I/O statistics of a specified sequence.

.. table:: **Table 1** PG_STATIO_ALL_SEQUENCES columns

   ========== ====== ============================================
   Column     Type   Description
   ========== ====== ============================================
   relid      OID    OID of this sequence
   schemaname Name   Name of the schema this sequence is in
   relname    Name   Name of this sequence
   blks_read  Bigint Number of disk blocks read from the sequence
   blks_hit   Bigint Number of buffer hits in this sequence
   ========== ====== ============================================
