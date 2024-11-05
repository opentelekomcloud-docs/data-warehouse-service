:original_name: dws_04_0636.html

.. _dws_04_0636:

SNAPSHOT
========

**SNAPSHOT** records the start and end time of each performance view snapshot creation. After **enable_wdr_snapshot** is set to **on**, this catalog is created and maintained by the background snapshot thread. It is accessible only to users with system administrator rights.

.. table:: **Table 1** dbms_om.snapshot columns

   +-------------+--------------------------+-------------------------------------------------------------------+
   | Name        | Type                     | Description                                                       |
   +=============+==========================+===================================================================+
   | snapshot_id | name                     | Snapshot ID. This column is the primary key and distribution key. |
   +-------------+--------------------------+-------------------------------------------------------------------+
   | start_ts    | timestamp with time zone | Snapshot start time                                               |
   +-------------+--------------------------+-------------------------------------------------------------------+
   | end_ts      | timestamp with time zone | Snapshot end time                                                 |
   +-------------+--------------------------+-------------------------------------------------------------------+

.. important::

   -  This system catalog's schema is **dbms_om**.
   -  Do not modify or delete this catalog externally. Otherwise, functions related to view snapshots may not work properly.
