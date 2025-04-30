:original_name: dws_04_0854.html

.. _dws_04_0854:

PV_SESSION_TIME
===============

**PV_SESSION_TIME** displays statistics about the running time of session threads and time consumed in each execution phase, in microseconds.

.. table:: **Table 1** PV_SESSION_TIME columns

   ========= ======= ================================
   Column    Type    Description
   ========= ======= ================================
   sessid    Text    Thread ID and thread start time.
   stat_id   Integer Statistics ID.
   stat_name Text    Name of the runtime type.
   value     Bigint  Runtime value.
   ========= ======= ================================
