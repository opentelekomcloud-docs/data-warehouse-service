:original_name: dws_04_0853.html

.. _dws_04_0853:

PV_SESSION_STAT
===============

**PV_SESSION_STAT** displays session state statistics based on session threads or the **AutoVacuum** thread.

.. table:: **Table 1** PV_SESSION_STAT columns

   ======== ======= ================================
   Column   Type    Description
   ======== ======= ================================
   sessid   Text    Thread ID and thread start time.
   statid   Integer Statistics ID.
   statname Text    Name of the statistics session.
   statunit Text    Unit of the statistics session.
   value    Bigint  Value of the statistics session.
   ======== ======= ================================
