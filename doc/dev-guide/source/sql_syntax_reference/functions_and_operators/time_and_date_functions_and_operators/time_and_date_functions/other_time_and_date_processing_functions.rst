:original_name: dws_06_0423.html

.. _dws_06_0423:

Other Time and Date Processing Functions
========================================

.. _en-us_topic_0000002510221160__section9353185020554:

justify_days(interval)
----------------------

Description: Adjusts the interval, so that the 30-day time period is represented as a month.

Return type: interval

Example:

::

   SELECT justify_days(interval '35 days');
    justify_days
   --------------
    1 mon 5 days
   (1 row)

.. _en-us_topic_0000002510221160__section9897647125518:

justify_hours(interval)
-----------------------

Description: Adjusts the interval, so that the 24-hour time period is represented as a day.

Return type: interval

Example:

::

   SELECT JUSTIFY_HOURS(INTERVAL '27 HOURS');
    justify_hours
   ----------------
    1 day 03:00:00
   (1 row)

.. _en-us_topic_0000002510221160__section123652457550:

justify_interval(interval)
--------------------------

Description: Adjusts the interval using **justify_days** and **justify_hours**.

Return type: interval

Example:

::

   SELECT JUSTIFY_INTERVAL(INTERVAL '1 MON -1 HOUR');
    justify_interval
   ------------------
    29 days 23:00:00
   (1 row)

.. _en-us_topic_0000002510221160__section63941218165516:

numtodsinterval(num, text)
--------------------------

Description: Converts a numeric value into an INTERVAL DAY TO SECOND literal based on a specified unit. It can be used to add or subtract a date and time value. You can set the **IntervalStyle** parameter to **oracle** to be compatible with the interval output format of the function in the Oracle database.

Parameters:

-  **num**: numeric number to be converted.
-  **text**: interval unit. Its value can be **DAY**, **HOUR**, **MINUTE**, or **SECOND**.

Example:

Return the time 100 hours later than the current time.

::

   SELECT sysdate + numtodsinterval(100, 'HOUR') FROM dual;
         ?column?
   ---------------------
    2026-01-18 18:49:00
   (1 row)

Set the **IntervalStyle** parameter to **oracle** to be compatible with the interval output format of the function in the Oracle database. Convert 100 hours into a INTERVAL DAY TO SECOND literal.

::

   SET intervalstyle = oracle;
   SET
   SELECT numtodsinterval(100, 'HOUR');
           numtodsinterval
   -------------------------------
    +000000004 04:00:00.000000000
   (1 row)

.. _en-us_topic_0000002510221160__section199981454573:

convert_tz(timestamp, from_tz, to_tz)
-------------------------------------

Description: Converts the timestamp value from the time zone specified by **from_tz** (text) to the time zone specified by **to_tz** (text) and returns the converted time. This parameter is supported only by clusters of version 8.2.0 or later.

Return type: timestamp without time zone

Example:

::

   SELECT convert_tz('2018-12-25 13:25:00', '+02:00', '+08:00');
       convert_tz
   --------------------
    2018-12-25 19:25:00
   (1 row)
   SELECT convert_tz('2000-02-28 23:00:00', 'GMT', 'MET');
       convert_tz
   --------------------
    2000-02-29 00:00:00
   (1 row)

.. _en-us_topic_0000002510221160__section19578115919559:

isfinite(date)
--------------

Description: Checks whether the date is finite.

Return type: boolean

Example:

::

   SELECT isfinite(date '2001-02-16');
    isfinite
   ----------
    t
   (1 row)
   SELECT isfinite(date 'infinity');
    isfinite
   ----------
    f
   (1 row)

.. _en-us_topic_0000002510221160__section5857195655513:

isfinite(timestamp)
-------------------

Description: Checks whether the timestamp is finite.

Return type: boolean

Example:

::

   SELECT isfinite(timestamp '2001-02-16 21:28:30');
    isfinite
   ----------
    t
   (1 row)
   SELECT isfinite(timestamp 'infinity');
    isfinite
   ----------
    f
   (1 row)

.. _en-us_topic_0000002510221160__section13251105395512:

isfinite(interval)
------------------

Description: Checks whether the time interval is finite.

Return type: boolean

Example:

::

   SELECT isfinite(interval '4 hours');
    isfinite
   ----------
    t
   (1 row)

.. _en-us_topic_0000002510221160__section596101465516:

pg_sleep(seconds)
-----------------

Description: Returns the delay of the server thread in seconds.

Return type: void

Example:

::

   SELECT pg_sleep(10);
    pg_sleep
   ----------

   (1 row)
