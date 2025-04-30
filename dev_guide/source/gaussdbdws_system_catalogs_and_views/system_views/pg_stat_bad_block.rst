:original_name: dws_04_0759.html

.. _dws_04_0759:

PG_STAT_BAD_BLOCK
=================

**PG_STAT_BAD_BLOCK** displays statistics about page or CU verification failures after a node is started.

.. table:: **Table 1** PG_STAT_BAD_BLOCK columns

   ============ ======================== ================================
   Column       Type                     Description
   ============ ======================== ================================
   nodename     Text                     Node name.
   databaseid   Integer                  Database OID.
   tablespaceid Integer                  Tablespace OID.
   relfilenode  Integer                  File object ID.
   forknum      Integer                  File type.
   error_count  Integer                  Number of verification failures.
   first_time   Timestamp with time zone Time of the first occurrence.
   last_time    Timestamp with time zone Time of the latest occurrence.
   ============ ======================== ================================
