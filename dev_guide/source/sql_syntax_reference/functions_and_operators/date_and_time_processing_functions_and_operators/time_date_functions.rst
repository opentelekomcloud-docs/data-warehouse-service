:original_name: dws_06_0309.html

.. _dws_06_0309:

Time/Date functions
===================

age(timestamp, timestamp)
-------------------------

Description: Subtracts arguments, producing a result in YYYY-MM-DD format. If the result is negative, the returned result is also negative.

Return type: interval

Example:

::

   SELECT age(TIMESTAMP '2001-04-10', TIMESTAMP '1957-06-13');
              age
   -------------------------
    43 years 9 mons 27 days
   (1 row)

age(timestamp)
--------------

Description: Subtracts from **current_date**

Return type: interval

Example:

::

   SELECT age(TIMESTAMP '1957-06-13');
              age
   -------------------------
    60 years 2 mons 18 days
   (1 row)

.. _en-us_topic_0000001495991685__section12597367428:

adddate(date, interval \| int)
------------------------------

Description: Returns the result of a given datetime plus the time interval of a specified unit. The default unit is day (when the second parameter is an integer).

Return type: timestamp

Example:

When the input parameter is of the text type:

::

   SELECT adddate('2020-11-13', 10);
     adddate
   ------------
    2020-11-23
   (1 row)

   SELECT adddate('2020-11-13', interval '1' month);
     adddate
   ------------
    2020-12-13
   (1 row)

   SELECT adddate('2020-11-13 12:15:16', interval '1' month);
          adddate
   ---------------------
    2020-12-13 12:15:16
   (1 row)


   SELECT adddate('2020-11-13', interval '1' minute);
          adddate
   ---------------------
    2020-11-13 00:01:00
   (1 row)

When the input parameter is of the date type:

::

   SELECT adddate(current_date, 10);
     adddate
   ------------
    2021-09-24
   (1 row)

   SELECT adddate(date '2020-11-13', interval '1' month);
          adddate
   ---------------------
    2020-12-13 00:00:00
   (1 row)

.. _en-us_topic_0000001495991685__section14108183084217:

subdate(date, interval \| int)
------------------------------

Description: Returns the result of a given datetime minus the time interval of a specified unit. The default unit is day (when the second parameter is an integer).

Return type: timestamp

Example:

When the input parameter is of the text type:

::

   SELECT subdate('2020-11-13', 10);
     subdate
   ------------
    2020-11-03
   (1 row)

   SELECT subdate('2020-11-13', interval '2' month);
     subdate
   ------------
    2020-09-13
   (1 row)

   SELECT subdate('2020-11-13 12:15:16', interval '1' month);
          subdate
   ---------------------
    2020-10-13 12:15:16
   (1 row)

   SELECT subdate('2020-11-13', interval '2' minute);
          subdate
   ---------------------
    2020-11-12 23:58:00
   (1 row)

When the input parameter is of the date type:

::

   SELECT subdate(current_date, 10);
     subdate
   ------------
    2021-09-05
   (1 row)

   SELECT subdate(current_date, interval '1' month);
          subdate
   ---------------------
    2021-08-15 00:00:00
   (1 row)

date_add(date, interval)
------------------------

Description: Returns the result of a given datetime plus the time interval of a specified unit. It is equivalent to :ref:`adddate(date, interval | int) <en-us_topic_0000001495991685__section12597367428>`.

Return type: timestamp

date_sub(date, interval)
------------------------

Description: Returns the result of a given datetime minus the time interval of a specified unit. It is equivalent to :ref:`subdate(date, interval | int) <en-us_topic_0000001495991685__section14108183084217>`.

Return type: timestamp

timestampadd(field, numeric, timestamp)
---------------------------------------

Description: Adds an integer interval in the unit of **field** (the number of seconds can be a decimal) to a datetime expression. If the value is negative, the corresponding time interval is subtracted from the given datetime expression. The **field** can be **year**, **month**, **quarter**, **day**, **week**, **hour**, **minute**, **second**, and **microsecond**.

Return type: timestamp

Example:

::

   SELECT timestampadd(year, 1, TIMESTAMP '2020-2-29');
       timestampadd
   ---------------------
    2021-02-28 00:00:00
   (1 row)

   SELECT timestampadd(second, 2.354156, TIMESTAMP '2020-11-13');
           timestampadd
   ----------------------------
    2020-11-13 00:00:02.354156
   (1 row)

timestampdiff(field, timestamp1, timestamp2)
--------------------------------------------

Description: Subtracts **timestamp1** from **timestamp2** and returns the difference in the unit of **field**. If the difference is negative, this function returns it normally. The **field** can be **year**, **month**, **quarter**, **day**, **week**, **hour**, **minute**, **second**, or **microsecond**.

Return type: bigint

Example:

::

   SELECT timestampdiff(day, TIMESTAMP '2001-02-01', TIMESTAMP '2003-05-01 12:05:55');
    timestampdiff
   ---------------
         819
   (1 row)

clock_timestamp()
-----------------

Description: Specifies the current timestamp of the real-time clock.

Return type: timestamp with time zone

Example:

::

   SELECT clock_timestamp();
           clock_timestamp
   -------------------------------
    2017-09-01 16:57:36.636205+08
   (1 row)

current_date
------------

Description: Current date

Return type: date

Example:

::

   SELECT current_date;
       date
   ------------
    2017-09-01
   (1 row)

current_time
------------

Description: Current time

Return type: time with time zone

Example:

::

   SELECT current_time;
          timetz
   --------------------
    16:58:07.086215+08
   (1 row)

.. _en-us_topic_0000001495991685__section75999514425:

current_timestamp
-----------------

Description: Specifies the current date and time.

Return type: timestamp with time zone

Example:

::

   SELECT current_timestamp;
          pg_systimestamp
   ------------------------------
    2017-09-01 16:58:19.22173+08
   (1 row)

datediff(date1, date2)
----------------------

Description: Returns the number of days between two given dates.

Return type: integer

Example:

::

   SELECT datediff(date '2020-11-13', date '2012-10-16');
    datediff
   ----------
        2950
   (1 row)

date_part(text, timestamp)
--------------------------

Description: Obtains the precision specified by **text**.

**Equals** :ref:`extract(field from timestamp) <en-us_topic_0000001495991685__section7165144274119>`.

Return type: double precision

Example:

::

   SELECT date_part('hour', TIMESTAMP '2001-02-16 20:38:40');
    date_part
   -----------
           20
   (1 row)

date_part(text, interval)
-------------------------

Description: Obtains the precision specified by **text**. If the value is greater than 12, obtain the remainder after it is divided by 12.

**Equals** :ref:`extract(field from timestamp) <en-us_topic_0000001495991685__section7165144274119>`

Return type: double precision

Example:

::

   SELECT date_part('month', interval '2 years 3 months');
    date_part
   -----------
            3
   (1 row)

date_trunc(text, timestamp)
---------------------------

Description: Truncates to the precision specified by **text**.

Return type: timestamp

Example:

::

   SELECT date_trunc('hour', TIMESTAMP  '2001-02-16 20:38:40');
        date_trunc
   ---------------------
    2001-02-16 20:00:00
   (1 row)

   -- Obtain the last day of last year.
   SELECT date_trunc('day', date_trunc('year',CURRENT_DATE)+ '-1');
          date_trunc
   ------------------------
    2022-12-31 00:00:00+00
   (1 row)

   -- Obtain the first day of this year.
   SELECT date_trunc('year',CURRENT_DATE);
          date_trunc
   ------------------------
    2023-01-01 00:00:00+00
   (1 row)

   -- Obtain the first day of last year.
   SELECT date_trunc('year',now() + '-1 year');
          date_trunc
   ------------------------
    2022-01-01 00:00:00+00
   (1 row)

trunc(timestamp)
----------------

Description: By default, the data is intercepted by day.

Return type: timestamp

Example:

::

   SELECT trunc(TIMESTAMP  '2001-02-16 20:38:40');                                                                                                                                                                   trunc
   ---------------------
   2001-02-16 00:00:00
   (1 row)

.. _en-us_topic_0000001495991685__section7165144274119:

extract(field from timestamp)
-----------------------------

Description: Obtains the value of **field** with the specified precision. For details about the valid values of **field**, see :ref:`EXTRACT <dws_06_0310>`.

Return type: double precision

Example:

::

   SELECT extract(hour FROM TIMESTAMP '2001-02-16 20:38:40');
    date_part
   -----------
           20
   (1 row)

extract(field from interval)
----------------------------

Description: Obtains the value of **field** with the specified precision. If the value is greater than 12, obtain the remainder after it is divided by 12. For details about the valid values of **field**, see :ref:`EXTRACT <dws_06_0310>`.

Return type: double precision

Example:

::

   SELECT extract(month FROM interval '2 years 3 months');
    date_part
   -----------
            3
   (1 row)

day(date)
---------

Description: Obtains the number of days in the month in which **date** is located. This function is the same as the **dayofmonth** function.

Value range: 1 to 31

Return type: integer

Example:

::

   SELECT day('2020-06-28');
    day
   -----
     28
   (1 row)

dayofmonth(date)
----------------

Description: Obtains the number of days in the month in which **date** is located.

Value range: 1 to 31

Return type: integer

Example:

::

   SELECT dayofmonth('2020-06-28');
    dayofmonth
   ------------
            28
   (1 row)

dayofweek(date)
---------------

Description: Returns the week index corresponding to the given date, with Sunday as the start day of the week.

Value range: 1 to 7

Return type: integer

Example:

::

   SELECT dayofweek('2020-11-22');
    dayofweek
   -----------
            1
   (1 row)

dayofyear(date)
---------------

Description: Returns the number of days of a given date in the current year.

Value range: 1 to 366

Return type: integer

Example:

::

   SELECT dayofyear('2020-02-29');
    dayofyear
   -----------
           60
   (1 row)

hour(timestamp with time zone)
------------------------------

Description: Obtains the hour value in the time.

Return type: integer

Example:

::

   SELECT hour(timestamptz '2018-12-13 12:11:15+06');
    hour
   ------
      6
   (1 row)

isfinite(date)
--------------

Description: Checks whether a date is finite.

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

isfinite(timestamp)
-------------------

Description: Checks whether a timestamp is finite.

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

isfinite(interval)
------------------

Description: Checks whether an interval is finite.

Return type: boolean

Example:

::

   SELECT isfinite(interval '4 hours');
    isfinite
   ----------
    t
   (1 row)

justify_days(interval)
----------------------

Description: Adjusts interval to 30-day time periods are represented as months

Return type: interval

Example:

::

   SELECT justify_days(interval '35 days');
    justify_days
   --------------
    1 mon 5 days
   (1 row)

justify_hours(interval)
-----------------------

Description: Adjusts interval to 24-hour time periods are represented as days

Return type: interval

Example:

::

   SELECT justify_hours(interval '27 HOURS');
    justify_hours
   ----------------
    1 day 03:00:00
   (1 row)

justify_interval(interval)
--------------------------

Description: Adjusts **interval** using **justify_days** and **justify_hours**.

Return type: interval

Example:

::

   SELECT justify_interval(interval '1 MON -1 HOUR');
    justify_interval
   ------------------
    29 days 23:00:00
   (1 row)

localtime
---------

Description: Current time

Return type: time

Example:

::

   SELECT localtime ;
        time
   ----------------
    16:05:55.664681
   (1 row)

localtimestamp
--------------

Description: Specifies the current date and time.

Return type: timestamp

Example:

::

   SELECT localtimestamp;
            timestamp
   ----------------------------
    2017-09-01 17:03:30.781902
   (1 row)

makedate(year, dayofyear)
-------------------------

Description: Returns a date value based on the given year and the number of days in a year.

Return type: date

Example:

::

   SELECT makedate(2020, 60);
     makedate
   ------------
    2020-02-29
   (1 row)

maketime(hour, minute, second)
------------------------------

Description: Returns a value of the time type based on the given hour, minute, and second. The value of the time type in GaussDB(DWS) ranges from 00:00:00 to 24:00:00. Therefore, this function is not available when the value of hour is greater than 24 or less than 0.

Return type: time

Example:

::

   SELECT maketime(12, 15, 30.12);
     maketime
   -------------
    12:15:30.12
   (1 row)

microsecond(timestamp with time zone)
-------------------------------------

Description: Obtains the microsecond value in the time.

Return type: integer

Example:

::

   SELECT microsecond(timestamptz '2018-12-13 12:11:15.123634+06');
    microsecond
   -------------
         123634
   (1 row)

minute(timestamp with time zone)
--------------------------------

Description: Obtains the minute value in the time.

Return type: integer

Example:

::

   SELECT minute(timestamptz '2018-12-13 12:11:15+06');
    minute
   --------
        11
   (1 row)

month(date)
-----------

Description: Returns the month of a given datetime.

Return type: integer

Example:

::

   SELECT month('2020-11-30');
    month
   -------
       11
   (1 row)

now([fsp])
----------

Description: Specifies the start time of the transaction. The parameter determines the microsecond output precision. The default value is **6**.

Return type: timestamp with time zone

Example:

::

   SELECT now();
                 now
   -------------------------------
    2017-09-01 17:03:42.549426+08
   (1 row)

::

   SELECT now(3);
                now
   ----------------------------
    2021-09-08 10:59:00.427+08
   (1 row)

numtodsinterval(num, interval_unit)
-----------------------------------

Description: Converts a number to the interval type. **num** is a numeric-typed number. **interval_unit** is a string in the following format: 'DAY' \| 'HOUR' \| 'MINUTE' \| 'SECOND'

You can set the **IntervalStyle** parameter to **oracle** to be compatible with the interval output format of the function in the Oracle database.

Example:

::

   SELECT numtodsinterval(100, 'HOUR');
    numtodsinterval
   -----------------
    100:00:00
   (1 row)

   SET intervalstyle = oracle;
   SET
   SELECT numtodsinterval(100, 'HOUR');
           numtodsinterval
   -------------------------------
    +000000004 04:00:00.000000000
   (1 row)

pg_sleep(seconds)
-----------------

Description: Pauses the current session for a specified number of seconds.

Return type: void

Example:

::

   SELECT pg_sleep(10);
    pg_sleep
   ----------

   (1 row)

period_add(P, N)
----------------

Description: Returns the date of a given period plus *N* months.

Return type: integer

Example:

::

   SELECT period_add(200801, 2);
    period_add
   ------------
        200803
   (1 row)

period_diff(P1, P2)
-------------------

Description: Returns the number of months between two given dates.

Return type: integer

::

   SELECT period_diff(200802, 200703);
    period_diff
   -------------
             11
   (1 row)

quarter(date)
-------------

Description: Obtains the quarter to which the date belongs.

Return type: integer

Example:

::

   SELECT quarter(date '2018-12-13');
    quarter
   ---------
          4
   (1 row)

second(timestamp with time zone)
--------------------------------

Description: Obtains the second value in the time.

Return type: integer

Example:

::

   SELECT second(timestamptz '2018-12-13 12:11:15+06');
    second
   --------
        15
   (1 row)

statement_timestamp()
---------------------

Description: Specifies the current date and time.

Return type: timestamp with time zone

Example:

::

   SELECT statement_timestamp();
         statement_timestamp
   -------------------------------
    2017-09-01 17:04:39.119267+08
   (1 row)

sysdate
-------

Description: Specifies the current date and time.

Return type: timestamp

Example:

::

   SELECT sysdate;
          sysdate
   ---------------------
    2017-09-01 17:04:49
   (1 row)

timeofday()
-----------

Description: Current date and time (like **clock_timestamp**, but returned as a **text** string)

Return type: text

Example:

::

   SELECT timeofday();
                 timeofday
   -------------------------------------
    Fri Sep 01 17:05:01.167506 2017 CST
   (1 row)

transaction_timestamp()
-----------------------

Description: Current date and time (equivalent to :ref:`current_timestamp <en-us_topic_0000001495991685__section75999514425>`)

Return type: timestamp with time zone

Example:

::

   SELECT transaction_timestamp();
        transaction_timestamp
   -------------------------------
    2017-09-01 17:05:13.534454+08
   (1 row)

from_unixtime(unix_timestamp[,format])
--------------------------------------

Description: Converts a Unix timestamp to the datetime type when the format string is set to the default value. If the format string is specified, convert the Unix timestamp to a string of a specified format.

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
    1997 4th October 08:23:00
   (1 row)

unix_timestamp([timestamp with time zone])
------------------------------------------

Description: Obtains the number of seconds from **'1970-01-01 00:00:00'UTC** to the time when the parameter is input. If no parameter is input, set this parameter to the current time.

Return type: bigint (no parameter is input) or numeric (parameter is input)

Example:

::

   SELECT unix_timestamp();
    unix_timestamp
   ----------------
        1693906219
   (1 row)

::

   SELECT unix_timestamp('2018-09-08 12:11:13+06');
    unix_timestamp
   ----------------
        1536387073.000000
   (1 row)

add_months(d,n)
---------------

Description: Calculates the time point day and time of nth months.

Return type: timestamp

Example:

::

   SELECT add_months(to_date('2017-5-29', 'yyyy-mm-dd'), 11);
        add_months
   ---------------------
    2018-04-29 00:00:00
   (1 row)

last_day(d)
-----------

Description: Calculates the time of the last day in the month.

-  In the ORA- or TD-compatible mode, the return type is timestamp.
-  In the MySQL-compatible mode, the return type is date.

Example:

::

   SELECT last_day(to_date('2017-01-01', 'YYYY-MM-DD')) AS cal_result;
        cal_result
   ---------------------
    2017-01-31 00:00:00
   (1 row)

next_day(x,y)
-------------

Description: Calculates the time of the next week y started from x

-  In the ORA- or TD-compatible mode, the return type is timestamp.
-  In the MySQL-compatible mode, the return type is date.

Example:

::

   SELECT next_day(TIMESTAMP '2017-05-25 00:00:00','Sunday')AS cal_result;
        cal_result
   ---------------------
    2017-05-28 00:00:00
   (1 row)

from_days(days)
---------------

Description: Returns the corresponding date value based on the given number of days.

Return type: date

Example:

::

   SELECT from_days(730669);
    from_days
   ------------
    2000-07-03
   (1 row)

to_days(timestamp)
------------------

Description: Returns the number of days from the first day of year 0 to a specified date.

Return type: integer

Example:

::

   SELECT to_days(TIMESTAMP '2008-10-07');
    to_days
   ---------
     733687
   (1 row)
