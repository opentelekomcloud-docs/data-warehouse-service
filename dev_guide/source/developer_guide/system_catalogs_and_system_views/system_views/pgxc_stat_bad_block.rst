:original_name: dws_04_0821.html

.. _dws_04_0821:

PGXC_STAT_BAD_BLOCK
===================

**PGXC_STAT_BAD_BLOCK** displays statistics about page or CU verification failures after all nodes in a cluster are started.

.. table:: **Table 1** PGXC_STAT_BAD_BLOCK columns

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
