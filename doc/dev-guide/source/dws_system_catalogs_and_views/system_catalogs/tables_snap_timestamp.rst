:original_name: dws_04_0637.html

.. _dws_04_0637:

TABLES_SNAP_TIMESTAMP
=====================

**TABLES_SNAP_TIMESTAMP** records the start and end time of the snapshots created for each performance view. After **enable_wdr_snapshot** is set to **on**, this catalog is created and maintained by the background snapshot thread. It is accessible only to users with system administrator rights.

.. table:: **Table 1** dbms_om.tables_snap_timestamp columns

   +-------------+--------------------------+-------------------------------------------------------------------+
   | Column      | Type                     | Description                                                       |
   +=============+==========================+===================================================================+
   | snapshot_id | Name                     | Snapshot ID. This column is the primary key and distribution key. |
   +-------------+--------------------------+-------------------------------------------------------------------+
   | db_name     | Text                     | Name of the database to which the view belongs.                   |
   +-------------+--------------------------+-------------------------------------------------------------------+
   | tablename   | Text                     | View name.                                                        |
   +-------------+--------------------------+-------------------------------------------------------------------+
   | start_ts    | Timestamp with time zone | Snapshot start time.                                              |
   +-------------+--------------------------+-------------------------------------------------------------------+
   | end_ts      | Timestamp with time zone | Snapshot end time.                                                |
   +-------------+--------------------------+-------------------------------------------------------------------+

.. important::

   -  This system catalog's schema is **dbms_om**.
   -  Do not modify or delete this catalog externally. Otherwise, functions related to view snapshots may not work properly.
