:original_name: dws_04_0696.html

.. _dws_04_0696:

GS_WAIT_EVENTS
==============

**GS_WAIT_EVENTS** displays statistics about waiting status and events on the current node.

The values of statistical columns in this view are accumulated only when the **enable_track_wait_event** GUC parameter is set to **on**. If **enable_track_wait_event** is set to **off** during statistics measurement, the statistics will no longer be accumulated, but the existing values are not affected. If **enable_track_wait_event** is **off**, 0 row is returned when this view is queried.

.. table:: **Table 1** GS_WAIT_EVENTS columns

   +-----------------+--------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Name            | Type   | Description                                                                                                                                                             |
   +=================+========+=========================================================================================================================================================================+
   | nodename        | name   | Node name                                                                                                                                                               |
   +-----------------+--------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | type            | text   | Event type, which can be **STATUS**, **LOCK_EVENT**, **LWLOCK_EVENT**, or **IO_EVENT**                                                                                  |
   +-----------------+--------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | event           | text   | Event name. For details, see :ref:`PG_THREAD_WAIT_STATUS <dws_04_0783>`.                                                                                                |
   +-----------------+--------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | wait            | bigint | Number of times an event occurs. This column and all the columns below are values accumulated during process running.                                                   |
   +-----------------+--------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | failed_wait     | bigint | Number of waiting failures. In the current version, this column is used only for counting timeout errors and waiting failures of locks such as **LOCK** and **LWLOCK**. |
   +-----------------+--------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | total_wait_time | bigint | Total duration of the event                                                                                                                                             |
   +-----------------+--------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | avg_wait_time   | bigint | Average duration of the event                                                                                                                                           |
   +-----------------+--------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | max_wait_time   | bigint | Maximum wait time of the event                                                                                                                                          |
   +-----------------+--------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | min_wait_time   | bigint | Minimum wait time of the event                                                                                                                                          |
   +-----------------+--------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

In the current version, for events whose **type** is **LOCK_EVENT**, **LWLOCK_EVENT**, or **IO_EVENT**, the display scope of **GS_WAIT_EVENTS** is the same as that of the corresponding events in the :ref:`PG_THREAD_WAIT_STATUS <dws_04_0783>` view.

For events whose **type** is **STATUS**, **GS_WAIT_EVENTS** displays the following waiting status columns. For details, see the :ref:`PG_THREAD_WAIT_STATUS <dws_04_0783>` view.

-  acquire lwlock
-  acquire lock
-  wait io
-  wait pooler get conn
-  wait pooler abort conn
-  wait pooler clean conn
-  wait transaction sync
-  wait wal sync
-  wait data sync
-  wait producer ready
-  create index
-  analyze
-  vacuum
-  vacuum full
-  gtm connect
-  gtm begin trans
-  gtm commit trans
-  gtm rollback trans
-  gtm create sequence
-  gtm alter sequence
-  gtm get sequence val
-  gtm set sequence val
-  gtm drop sequence
-  gtm rename sequence
