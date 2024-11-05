:original_name: dws_04_0759.html

.. _dws_04_0759:

PG_STAT_BAD_BLOCK
=================

**PG_STAT_BAD_BLOCK** displays statistics about page or CU verification failures after a node is started.

.. table:: **Table 1** PG_STAT_BAD_BLOCK columns

   ============ ======================== ===============================
   Name         Type                     Description
   ============ ======================== ===============================
   nodename     text                     Node name
   databaseid   integer                  Database OID
   tablespaceid integer                  Tablespace OID
   relfilenode  integer                  File object ID
   forknum      integer                  File type
   error_count  integer                  Number of verification failures
   first_time   timestamp with time zone Time of the first occurrence
   last_time    timestamp with time zone Time of the latest occurrence
   ============ ======================== ===============================
