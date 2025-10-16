:original_name: dws_04_0811.html

.. _dws_04_0811:

PGXC_NODE_STAT_RESET_TIME
=========================

**PGXC_NODE_STAT_RESET_TIME** displays the time when statistics of each node in the cluster are reset. All columns except **node_name** are the same as those in the :ref:`GS_NODE_STAT_RESET_TIME <dws_04_0692>` view. This view is accessible only to users with system administrators rights.

.. table:: **Table 1** PGXC_NODE_STAT_RESET_TIME columns

   ========== ========= ===========================================
   Column     Type      Description
   ========== ========= ===========================================
   node_name  Text      Node name
   reset_time Timestamp Time when statistics on each node are reset
   ========== ========= ===========================================
