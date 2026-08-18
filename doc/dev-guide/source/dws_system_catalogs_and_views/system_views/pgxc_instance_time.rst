:original_name: dws_04_0807.html

.. _dws_04_0807:

PGXC_INSTANCE_TIME
==================

**PGXC_INSTANCE_TIME** displays the running time of processes on each node in the cluster and the time consumed in each execution phase. Except the **node_name** column, the other columns are the same as those in the :ref:`PV_INSTANCE_TIME <dws_04_0849>` view. Only the system administrator or the preset role **gs_role_read_all_stats** can access this view.

.. table:: **Table 1** PGXC_INSTANCE_TIME columns

   ========= ======= ========================
   Column    Type    Description
   ========= ======= ========================
   node_name Text    Node name
   stat_id   Integer Type ID
   stat_name Text    Name of the runtime type
   value     Bigint  Runtime value
   ========= ======= ========================
