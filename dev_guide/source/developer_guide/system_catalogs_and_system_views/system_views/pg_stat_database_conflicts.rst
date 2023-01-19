:original_name: dws_04_0762.html

.. _dws_04_0762:

PG_STAT_DATABASE_CONFLICTS
==========================

**PG_STAT_DATABASE_CONFLICTS** displays statistics about database conflicts.

.. table:: **Table 1** PG_STAT_DATABASE_CONFLICTS columns

   ================ ====== =================================
   Name             Type   Description
   ================ ====== =================================
   datid            oid    Database OID
   datname          name   Database name
   confl_tablespace bigint Number of conflicting tablespaces
   confl_lock       bigint Number of conflicting locks
   confl_snapshot   bigint Number conflicting snapshots
   confl_bufferpin  bigint Number of conflicting buffers
   confl_deadlock   bigint Number of conflicting deadlocks
   ================ ====== =================================
