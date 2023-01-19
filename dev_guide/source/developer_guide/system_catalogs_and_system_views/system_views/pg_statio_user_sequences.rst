:original_name: dws_04_0781.html

.. _dws_04_0781:

PG_STATIO_USER_SEQUENCES
========================

**PG_STATIO_USER_SEQUENCES** displays the I/O status information about all the user relation table sequences in the namespace.

.. table:: **Table 1** PG_STATIO_USER_SEQUENCES columns

   ========== ====== =============================================
   Name       Type   Description
   ========== ====== =============================================
   relid      oid    OID of this sequence
   schemaname name   Name of the schema this sequence is in
   relname    name   Name of this sequence
   blks_read  bigint Number of disk blocks read from this sequence
   blks_hit   bigint Number of cache hits in this sequence
   ========== ====== =============================================
