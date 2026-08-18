:original_name: dws_04_0795.html

.. _dws_04_0795:

PGXC_BULKLOAD_PROGRESS
======================

**PGXC_BULKLOAD_PROGRESS** displays the progress of the service import. Only GDS common files can be imported. This view is accessible only to users with system administrators rights.

.. table:: **Table 1** PGXC_BULKLOAD_PROGRESS columns

   ========== ====== =================================================
   Column     Type   Description
   ========== ====== =================================================
   session_id Bigint GDS session ID
   query_id   Bigint Query ID. It is equivalent to **debug_query_id**.
   query      Text   Query statement
   progress   Text   Progress percentage
   ========== ====== =================================================
