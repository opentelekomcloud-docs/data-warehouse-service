:original_name: dws_04_0812.html

.. _dws_04_0812:

PGXC_OS_RUN_INFO
================

**PGXC_OS_RUN_INFO** displays the OS running status of each node in the cluster. All columns except **node_name** are the same as those in the :ref:`PV_OS_RUN_INFO <dws_04_0850>` view. Only the system administrator or the preset role **gs_role_read_all_stats** can access this view.

.. table:: **Table 1** PGXC_OS_RUN_INFO field columns

   ========== ======= ================================================
   Column     Type    Description
   ========== ======= ================================================
   node_name  Text    Node name
   id         Integer ID
   Name       Text    Name of the OS running status
   value      Numeric Value of the OS status
   comments   Text    Remarks of the OS status
   cumulative boolean Whether the value of the OS status is cumulative
   ========== ======= ================================================
