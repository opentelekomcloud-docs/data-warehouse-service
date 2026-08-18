:original_name: dws_04_0778.html

.. _dws_04_0778:

PG_STATIO_SYS_SEQUENCES
=======================

**PG_STATIO_SYS_SEQUENCES** displays the I/O status information about all the system sequences in the namespace.

.. table:: **Table 1** PG_STATIO_SYS_SEQUENCES columns

   ========== ====== ============================================
   Column     Type   Description
   ========== ====== ============================================
   relid      OID    OID of this sequence
   schemaname Name   Name of the schema this sequence is in
   relname    Name   Name of this sequence
   blks_read  Bigint Number of disk blocks read from the sequence
   blks_hit   Bigint Number of buffer hits in this sequence
   ========== ====== ============================================
