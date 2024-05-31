:original_name: dws_04_0638.html

.. _dws_04_0638:

System Catalogs for Performance View Snapshot
=============================================

After **enable_wdr_snapshot** is set to **on**, the background snapshot thread creates and maintains a system catalog named in the format of **SNAP\_**\ *View name* to record the snapshot result of each performance view. The following system catalogs are accessible only to users with system administrator rights:

-  SNAP\_\ :ref:`PGXC_OS_RUN_INFO <dws_04_0812>`
-  SNAP\_\ :ref:`PGXC_WAIT_EVENTS <dws_04_0832>`
-  SNAP\_\ :ref:`PGXC_INSTR_UNIQUE_SQL <dws_04_0808>`
-  SNAP\_\ :ref:`PGXC_STAT_BAD_BLOCK <dws_04_0821>`
-  SNAP\_\ :ref:`PGXC_STAT_BGWRITER <dws_04_0822>`
-  SNAP\_\ :ref:`PGXC_STAT_REPLICATION <dws_04_0824>`
-  SNAP\_\ :ref:`PGXC_REPLICATION_SLOTS <dws_04_0817>`
-  SNAP\_\ :ref:`PGXC_SETTINGS <dws_04_0819>`
-  SNAP\_\ :ref:`PGXC_INSTANCE_TIME <dws_04_0807>`
-  SNAP\_\ :ref:`GLOBAL_WORKLOAD_TRANSACTION <dws_04_0687>`
-  SNAP\_\ :ref:`PGXC_WORKLOAD_SQL_COUNT <dws_04_0843>`
-  SNAP\_\ :ref:`PGXC_STAT_DATABASE <dws_04_0823>`
-  SNAP\_\ :ref:`GLOBAL_STAT_DATABASE <dws_04_0684>`
-  SNAP\_\ :ref:`PGXC_REDO_STAT <dws_04_0815>`
-  SNAP\_\ :ref:`GLOBAL_REDO_STAT <dws_04_0682>`
-  SNAP\_\ :ref:`PGXC_REL_IOSTAT <dws_04_0816>`
-  SNAP\_\ :ref:`GLOBAL_REL_IOSTAT <dws_04_0683>`
-  SNAP\_\ :ref:`PGXC_TOTAL_MEMORY_DETAIL <dws_04_0827>`
-  SNAP\_\ :ref:`PGXC_NODE_STAT_RESET_TIME <dws_04_0811>`
-  SNAP\_\ :ref:`PGXC_SQL_COUNT <dws_04_0825>`
-  SNAP\_\ :ref:`GLOBAL_TABLE_STAT <dws_04_0962>`
-  SNAP\_\ :ref:`GLOBAL_TABLE_CHANGE_STAT <dws_04_0963>`
-  SNAP\_\ :ref:`GLOBAL_COLUMN_TABLE_IO_STAT <dws_04_0964>`
-  SNAP\_\ :ref:`GLOBAL_ROW_TABLE_IO_STAT <dws_04_0965>`

Except the new **snapshot_id** column (of the bigint type), the definitions of the other columns in these system catalogs are the same as those of the corresponding views, and the distribution key of each system catalog is **snapshot_id**.

For example, **SNAP_PGXC_OS_RUN_INFO** is used to record snapshots of the **PGXC_OS_RUN_INFO** view. The **snapshot_id** column is new, and other columns are the same as those of the :ref:`PGXC_OS_RUN_INFO <dws_04_0812>` view.

.. important::

   -  The schema of all above system catalogs is **dbms_om**.
   -  Do not modify or delete these catalogs externally. Otherwise, functions related to view snapshots may not work properly.
