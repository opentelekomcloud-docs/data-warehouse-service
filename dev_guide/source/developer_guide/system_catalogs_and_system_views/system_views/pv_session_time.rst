:original_name: dws_04_0854.html

.. _dws_04_0854:

PV_SESSION_TIME
===============

**PV_SESSION_TIME** displays statistics about the running time of session threads and time consumed in each execution phase, in microseconds.

.. table:: **Table 1** PV_SESSION_TIME columns

   ========= ======= ========================
   Name      Type    Description
   ========= ======= ========================
   sessid    text    Thread ID and start time
   stat_id   integer Statistics ID
   stat_name text    Running time type name
   value     bigint  Running time value
   ========= ======= ========================
