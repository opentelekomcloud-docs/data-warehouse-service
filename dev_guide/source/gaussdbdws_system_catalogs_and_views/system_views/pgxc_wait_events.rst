:original_name: dws_04_0832.html

.. _dws_04_0832:

PGXC_WAIT_EVENTS
================

**PGXC_WAIT_EVENTS** displays statistics on the waiting status and events of each node in the cluster. The content is the same as that displayed in :ref:`GS_WAIT_EVENTS <dws_04_0696>`. This view is accessible only to users with system administrators rights.

.. table:: **Table 1** PGXC_WAIT_EVENTS columns

   +-----------------+--------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Column          | Type   | Description                                                                                                                                                             |
   +=================+========+=========================================================================================================================================================================+
   | nodename        | Name   | Node name.                                                                                                                                                              |
   +-----------------+--------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | type            | Text   | Event type, which can be **STATUS**, **LOCK_EVENT**, **LWLOCK_EVENT**, or **IO_EVENT**                                                                                  |
   +-----------------+--------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | event           | Text   | Event name. For details, see :ref:`PG_THREAD_WAIT_STATUS <dws_04_0783>`.                                                                                                |
   +-----------------+--------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | wait            | Bigint | Number of times an event occurs. This column and all the columns below are values accumulated during process running.                                                   |
   +-----------------+--------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | failed_wait     | Bigint | Number of waiting failures. In the current version, this column is used only for counting timeout errors and waiting failures of locks such as **LOCK** and **LWLOCK**. |
   +-----------------+--------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | total_wait_time | Bigint | Total duration of the event                                                                                                                                             |
   +-----------------+--------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | avg_wait_time   | Bigint | Average duration of the event                                                                                                                                           |
   +-----------------+--------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | max_wait_time   | Bigint | Maximum wait time of the event                                                                                                                                          |
   +-----------------+--------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | min_wait_time   | Bigint | Minimum wait time of the event                                                                                                                                          |
   +-----------------+--------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
