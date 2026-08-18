:original_name: dws_06_0311.html

.. _dws_06_0311:

Basic Time and Date Functions
=============================

.. _en-us_topic_0000002541821149__section5432141514561:

day(date \| timestamp with time zone)
-------------------------------------

Description: Returns the day (that is, the day in a month) of a given date or time. This function is the same as the :ref:`dayofmonth(date) <en-us_topic_0000002541821149__section12631211105616>` function.

Value range: 1 to 31

Return type: integer

Example:

::

   SELECT day('2025-06-28');
    day
   -----
     28
   (1 row)
   SELECT day('2025-06-28 09:00:00+08');
    day
   -----
     28
   (1 row)

.. _en-us_topic_0000002541821149__section14348956185810:

weekday(date \| timestamp with time zone)
-----------------------------------------

Description: Returns the week of a given date or time. Monday is the start day of a week.

Value range: 0 to 6

Return type: integer

Example:

::

   SELECT weekday('2020-11-06');
    weekday
   ---------
          4
   (1 row)
   SELECT weekday('2020-11-06 09:00:00+08');
    weekday
   ---------
          4
   (1 row)

.. _en-us_topic_0000002541821149__section32002275915:

week(date[, mode])
------------------

Description: Returns the week number in the year of the given date or time based on the mode. The default value of **mode** is **0**.

-  **date**: specifies the date for which the week number is to be obtained.
-  **mode** (optional): controls the start of a week and the first week of a year. By default, the first day of a week is Sunday. You can also specify whether a week starts from Monday or Sunday. For details about **mode**, see the following table.

Return type: integer

.. table:: **Table 1** Working principle of the mode in the week function

   +------+---------------------+------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Mode | First Day of a Week | Week Range | Rule for Determining the First Week                                                                                                                                    |
   +======+=====================+============+========================================================================================================================================================================+
   | 0    | Sunday              | 0-53       | The first week starts from the first Sunday of the year. The previous week is week 0.                                                                                  |
   +------+---------------------+------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | 1    | Monday              | 0-53       | If the number of days from January 1 to the first Monday is greater than 3, the week is the first week of the year. Otherwise, the week is week 0.                     |
   +------+---------------------+------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | 2    | Sunday              | 1-53       | The first week starts from the first Sunday of the year. The previous week is week 5\ *x* of the previous year.                                                        |
   +------+---------------------+------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | 3    | Monday              | 1-53       | If the number of days from January 1 to the first Monday is greater than 3, the week is the first week of the year. Otherwise, it is week 5\ *x* of the previous year. |
   +------+---------------------+------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | 4    | Sunday              | 0-53       | If the number of days from January 1 to the first Sunday is greater than 3, the week is the first week of the year. Otherwise, the week is week 0.                     |
   +------+---------------------+------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | 5    | Monday              | 0-53       | The first week starts from the first Monday of the year. The previous week is week 0.                                                                                  |
   +------+---------------------+------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | 6    | Sunday              | 1-53       | If the number of days from January 1 to the first Sunday is greater than 3, the week is the first week of the year. Otherwise, it is week 5\ *x* of the previous year. |
   +------+---------------------+------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | 7    | Monday              | 1-53       | The first week starts from the first Monday of the year. The previous week is week 5\ *x* of the previous year.                                                        |
   +------+---------------------+------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Examples:

Obtain the week number of a specified date (use the default value **0** of **mode**, that is, Sunday is the first day of a week).

::

   SELECT WEEK('2026-01-01');
    week
   ------
   0 -- January 1 is not in the first week of 2026. According to the rule of mode 0, January 1 is in the last week of the previous year. Therefore, 0 is returned.
   (1 row)

Obtain the week number of a specified date (set **mode** to 1, that is, Monday is the first day of a week according to the ISO standard).

::

   SELECT WEEK('2026-01-01', 1);
    week
   ------
       1 -- According to the rule of mode 1, January 1, 2026 is in the first week of the year.
   (1 row)

.. _en-us_topic_0000002541821149__section198971510124012:

month(date \| timestamp with time zone)
---------------------------------------

Description: Returns the month of a given date or time.

Return type: integer

Example:

::

   SELECT month('2025-11-30');
    month
   -------
       11
   (1 row)
   SELECT month('2025-11-30 09:00:00+08');
    month
   -------
       11
   (1 row)

.. _en-us_topic_0000002541821149__section93761962559:

quarter(date \| timestamp with time zone)
-----------------------------------------

Description: Returns the quarter to which the date or time belongs. The quarter ranges from 1 to 4.

Return type: integer

Example:

::

   SELECT quarter(date '2025-12-13');
    quarter
   ---------
          4
   (1 row)
   SELECT quarter('2025-03-20 09:00:00+08');
    quarter
   ---------
          1
   (1 row)

.. _en-us_topic_0000002541821149__section1071211507583:

year(date \| timestamp with time zone)
--------------------------------------

Description: Returns the year of a given date or time.

Return type: integer

Example:

::

   SELECT year('2025-11-13');
    year
   ------
    2025
   (1 row)
   SELECT year('2025-11-13 09:00:00+08');
    year
   ------
    2025
   (1 row)

.. _en-us_topic_0000002541821149__section1719311261177:

yearweek(date \| timestamp with time zone[, mode])
--------------------------------------------------

Description: Returns the year and the number of weeks in the year corresponding to a given date or time. The number of weeks ranges from 1 to 53.

Return type: integer

Example:

::

   SELECT yearweek('2025-11-13');
    yearweek
   ----------
      202545
   (1 row)
   SELECT yearweek('2025-1-1');
    yearweek
   ----------
      202452
   (1 row)

.. _en-us_topic_0000002541821149__section74827810567:

dayofweek(date \| timestamp with time zone)
-------------------------------------------

Description: Returns the week of a given date or time. Sunday is the start day of a week.

Value range: 1 to 7

Return type: integer

Example:

::

   SELECT dayofweek('2025-11-22');
    dayofweek
   -----------
            7
   (1 row)
   SELECT dayofweek('2025-11-22 09:00:00+08');
    dayofweek
   -----------
            7
   (1 row)

.. _en-us_topic_0000002541821149__section12631211105616:

dayofmonth(date \| timestamp with time zone)
--------------------------------------------

Description: Returns the number of days in the month of a given date or time.

Value range: 1 to 31

Return type: integer

Example:

::

   SELECT dayofmonth('2025-10-28');
    dayofmonth
   ------------
            28
   (1 row)
   SELECT dayofmonth('2025-10-28 09:00:00+08');
    dayofmonth
   ------------
            28
   (1 row)

.. _en-us_topic_0000002541821149__section3547358566:

dayofyear(date \| timestamp with time zone)
-------------------------------------------

Description: Returns the day of a given date or time in the year. January 1 is the first day, and December 31 is 365 or 366 days.

Value range: 1 to 366

Return type: integer

Example:

::

   SELECT dayofyear('2025-10-28');
    dayofyear
   -----------
          301
   (1 row)
   SELECT dayofyear('2025-10-28 09:00:00+08');
    dayofyear
   -----------
          301
   (1 row

.. _en-us_topic_0000002541821149__section17442053155815:

weekofyear(date \| timestamp with time zone)
--------------------------------------------

Description: Returns the week number of a given date or time in the year. It is equivalent to **week(date, 3**).

Value range: 1 to 53

Return type: integer

Example:

::

   SELECT weekofyear('2025-11-30');
    weekofyear
   ------------
            48
   (1 row)
   SELECT weekofyear('2025-11-30 09:00:00+08');
    weekofyear
   ------------
            48
   (1 row)

.. _en-us_topic_0000002541821149__section12161037205516:

makedate(year, dayofyear)
-------------------------

Description: Creates a date based on the given year and day of the year.

Return type: date

Example:

::

   SELECT makedate(2025, 60);
     makedate
   ------------
    2025-03-01
   (1 row)

.. _en-us_topic_0000002541821149__section11544113475511:

maketime(hour, minute, second)
------------------------------

Description: Generates a time value based on the given hour, minute, and second.

The value of the time type in DWS ranges from 00:00:00 to 24:00:00. Therefore, this function is not available when the value of hour is greater than 24 or less than 0.

Return type: time

Example:

::

   SELECT maketime(12, 15, 30.12);
     maketime
   -------------
    12:15:30.12
   (1 row)

.. _en-us_topic_0000002541821149__section199936371484:

hour(timestamp with time zone)
------------------------------

Description: Returns the hour value in a timestamp.

Return type: integer

Example:

::

   SELECT hour(timestamptz '2025-12-13 12:11:15+06');
    hour
   ------
      14
   (1 row)

.. _en-us_topic_0000002541821149__section1468172718553:

minute(timestamp with time zone)
--------------------------------

Description: Returns the minute value in the time.

Return type: integer

Example:

::

   SELECT minute(timestamptz '2025-12-13 12:11:15+06');
    minute
   --------
        11
   (1 row)

.. _en-us_topic_0000002541821149__section45322015483:

second(timestamp with time zone)
--------------------------------

Description: Returns the second value in a timestamp.

Return type: integer

Example:

::

   SELECT second(timestamptz '2025-12-13 12:11:15+06');
    second
   --------
        15
   (1 row)

.. _en-us_topic_0000002541821149__section2416103012559:

microsecond(timestamp with time zone)
-------------------------------------

Description: Returns the microsecond value in the time.

Return type: integer

Example:

::

   SELECT microsecond(timestamptz '2025-12-13 12:11:15.123634+06');
    microsecond
   -------------
         123634
   (1 row)

.. _en-us_topic_0000002541821149__section1155016482545:

from_unixtime(unix_timestamp[,format])
--------------------------------------

Description: Converts a Unix timestamp to the datetime type when the format string is set to the default value. If the format string is specified, this function converts the Unix timestamp to a string in a specified format.

Return type: timestamp (default format string) or text (specified format string)

Example:

::

   SELECT from_unixtime(875996580);
       from_unixtime
   ---------------------
    1997-10-04 20:23:00
   (1 row)
   SELECT from_unixtime(875996580, '%Y %D %M %h:%i:%s');
          from_unixtime
   ---------------------------
    1997 5th October 04:23:00
   (1 row)

.. _en-us_topic_0000002541821149__section373415443542:

unix_timestamp([timestamp with time zone])
------------------------------------------

Description: Converts a date or time to a Unix timestamp (that is, returns the total number of seconds that have elapsed since January 1, 1970 00:00:00 UTC). If there is no input parameter, the current time is used.

Return type: bigint (no parameter is input) or numeric (parameter is input)

Example:

::

   SELECT unix_timestamp();
    unix_timestamp
   ----------------
        1768793418
   (1 row)

::

   SELECT unix_timestamp('2025-09-08 12:11:13+06');
       unix_timestamp
   -------------------
    1757311873.000000
   (1 row)

.. _en-us_topic_0000002541821149__section196965211550:

now([fsp])
----------

Description: Returns the start date and time of the current transaction. The input parameter determines the microsecond precision. The default value is 6.

Return type: timestamp with time zone

Example:

::

   SELECT now();
                 now
   -------------------------------
    2026-01-19 11:35:09.082256+08
   (1 row)

::

   SELECT now(3);
                now
   ----------------------------
    2026-01-19 11:35:31.839+08
   (1 row)

.. _en-us_topic_0000002541821149__section158077372545:

last_day(d)
-----------

Description: Returns the date or time of the last day of the month that contains date.

Return type:

-  In the ORA- or TD-compatible mode, a timestamp is returned.
-  In the MySQL-compatible mode, a date is returned.

Example:

::

   SELECT last_day(to_date('2026-01-01', 'YYYY-MM-DD')) AS cal_result;
        cal_result
   ---------------------
    2026-01-31 00:00:00
   (1 row)

.. _en-us_topic_0000002541821149__section9551335205418:

next_day(x,y)
-------------

Description: Returns the earliest date that is later than its first argument (*x*), and that falls on the day of the week that its second argument (*y*) specifies.

Return type:

-  In the ORA- or TD-compatible mode, a timestamp is returned.
-  In the MySQL-compatible mode, a date is returned.

Example:

::

   SELECT next_day(timestamp '2025-05-21 00:00:00','Sunday')AS cal_result;
        cal_result
   ---------------------
    2025-05-25 00:00:00
   (1 row)

.. _en-us_topic_0000002541821149__section18837331135418:

from_days(days)
---------------

Description: Returns the date value based on the given number of days.

Return type: date

Example:

::

   SELECT from_days(730669);
    from_days
   ------------
    2000-07-03
   (1 row)

.. _en-us_topic_0000002541821149__section940512825411:

to_days(timestamp)
------------------

Description: Returns the number of days since the year 0 based on the specified date.

Return type: integer

Example:

::

   SELECT to_days(timestamp '2025-10-07');
    to_days
   ---------
     739896
   (1 row)
