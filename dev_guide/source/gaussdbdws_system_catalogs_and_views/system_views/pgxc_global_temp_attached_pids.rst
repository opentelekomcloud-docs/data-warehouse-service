:original_name: dws_04_1056.html

.. _dws_04_1056:

PGXC_GLOBAL_TEMP_ATTACHED_PIDS
==============================

This view displays information about sessions of resources occupied by global temporary tables on CNs. This view is supported only by clusters of 8.2.1.220 and later versions.

.. table:: **Table 1** PG_GLOBAL_TEMP_ATTACHED_PIDS columns

   ========== ====== ================
   Column     Type   Description
   ========== ====== ================
   nodename   Name   Node name
   schemaname Name   Schema name
   tablename  Name   Table name
   pid        Bigint PID of a session
   ========== ====== ================
