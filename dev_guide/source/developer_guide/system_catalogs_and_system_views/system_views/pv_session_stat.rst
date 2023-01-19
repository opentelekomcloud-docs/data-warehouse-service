:original_name: dws_04_0853.html

.. _dws_04_0853:

PV_SESSION_STAT
===============

**PV_SESSION_STAT** displays session state statistics based on session threads or the **AutoVacuum** thread.

.. table:: **Table 1** PV_SESSION_STAT columns

   ======== ======= ===============================
   Name     Type    Description
   ======== ======= ===============================
   sessid   text    Thread ID and start time
   statid   integer Statistics ID
   statname text    Name of the statistics session
   statunit text    Unit of the statistics session
   value    bigint  Value of the statistics session
   ======== ======= ===============================
