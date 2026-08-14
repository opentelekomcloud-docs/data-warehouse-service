:original_name: dws_04_0762.html

.. _dws_04_0762:

PG_STAT_DATABASE_CONFLICTS
==========================

**PG_STAT_DATABASE_CONFLICTS** displays statistics about database conflicts.

.. table:: **Table 1** PG_STAT_DATABASE_CONFLICTS columns

   ================ ====== ==================================
   Column           Type   Description
   ================ ====== ==================================
   datid            OID    Database OID.
   datname          Name   Database name.
   confl_tablespace Bigint Number of conflicting tablespaces.
   confl_lock       Bigint Number of conflicting locks.
   confl_snapshot   Bigint Number of conflicting snapshots.
   confl_bufferpin  Bigint Number of conflicting buffers.
   confl_deadlock   Bigint Number of conflicting deadlocks.
   ================ ====== ==================================
