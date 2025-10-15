:original_name: dws_04_1055.html

.. _dws_04_1055:

PG_GLOBAL_TEMP_ATTACHED_PIDS
============================

This view displays information about sessions of resources occupied by global temporary tables on the current node. This view is supported only by clusters of 8.2.1.220 and later versions.

.. table:: **Table 1** PG_GLOBAL_TEMP_ATTACHED_PIDS columns

   ========== ====== ================
   Column     Type   Description
   ========== ====== ================
   schemaname Name   Schema name
   tablename  Name   Table name
   pid        Bigint PID of a session
   ========== ====== ================
