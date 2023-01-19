:original_name: dws_06_0035.html

.. _dws_06_0035:

Date and Time Processing Functions and Operators
================================================

Date and Time Operators
-----------------------

.. warning::

   When the user uses date/time operators, explicit type prefixes are modified for corresponding operands to ensure that the operands parsed by the database are consistent with what the user expects, and no unexpected results occur.

   For example, abnormal mistakes will occur in the following example without an explicit data type.

   ::

      SELECT date '2001-10-01' - '7' AS RESULT;

.. table:: **Table 1** Time and date operators

   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------+
   | Operators                         | Examples                                                                                                                          |
   +===================================+===================================================================================================================================+
   | +                                 | Add a date with an integer to obtain the date after 7 days.                                                                       |
   |                                   |                                                                                                                                   |
   |                                   | ::                                                                                                                                |
   |                                   |                                                                                                                                   |
   |                                   |    SELECT date '2001-09-28' + integer '7' AS RESULT;                                                                              |
   |                                   |           result                                                                                                                  |
   |                                   |    ---------------------                                                                                                          |
   |                                   |     2001-10-05 00:00:00                                                                                                           |
   |                                   |    (1 row)                                                                                                                        |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------+
   |                                   | Add a date with an interval to obtain the time after 1 hour.                                                                      |
   |                                   |                                                                                                                                   |
   |                                   | ::                                                                                                                                |
   |                                   |                                                                                                                                   |
   |                                   |    SELECT date '2001-09-28' + interval '1 hour' AS RESULT;                                                                        |
   |                                   |           result                                                                                                                  |
   |                                   |    ---------------------                                                                                                          |
   |                                   |     2001-09-28 01:00:00                                                                                                           |
   |                                   |    (1 row)                                                                                                                        |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------+
   |                                   | Add a date with a time to obtain a specific time.                                                                                 |
   |                                   |                                                                                                                                   |
   |                                   | ::                                                                                                                                |
   |                                   |                                                                                                                                   |
   |                                   |    SELECT date '2001-09-28' + time '03:00' AS RESULT;                                                                             |
   |                                   |           result                                                                                                                  |
   |                                   |    ---------------------                                                                                                          |
   |                                   |     2001-09-28 03:00:00                                                                                                           |
   |                                   |    (1 row)                                                                                                                        |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------+
   |                                   | Add a date with an interval to obtain the time after one month.                                                                   |
   |                                   |                                                                                                                                   |
   |                                   | If the sum or subtraction results fall beyond the date range of a month, the result will be rounded to the last day of the month. |
   |                                   |                                                                                                                                   |
   |                                   | ::                                                                                                                                |
   |                                   |                                                                                                                                   |
   |                                   |    SELECT date '2021-01-31' + interval '1 month' AS RESULT;                                                                       |
   |                                   |           result                                                                                                                  |
   |                                   |    ---------------------                                                                                                          |
   |                                   |     2021-02-28 00:00:00                                                                                                           |
   |                                   |    (1 row)                                                                                                                        |
   |                                   |                                                                                                                                   |
   |                                   | ::                                                                                                                                |
   |                                   |                                                                                                                                   |
   |                                   |    SELECT date '2021-02-28' + interval '1 month' AS RESULT;                                                                       |
   |                                   |           result                                                                                                                  |
   |                                   |    ---------------------                                                                                                          |
   |                                   |     2021-03-28 00:00:00                                                                                                           |
   |                                   |    (1 row)                                                                                                                        |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------+
   |                                   | Add two intervals to obtain the sum.                                                                                              |
   |                                   |                                                                                                                                   |
   |                                   | ::                                                                                                                                |
   |                                   |                                                                                                                                   |
   |                                   |    SELECT interval '1 day' + interval '1 hour' AS RESULT;                                                                         |
   |                                   |         result                                                                                                                    |
   |                                   |    ----------------                                                                                                               |
   |                                   |     1 day 01:00:00                                                                                                                |
   |                                   |    (1 row)                                                                                                                        |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------+
   |                                   | Add a timestamp with an interval to obtain the time after 23 hours.                                                               |
   |                                   |                                                                                                                                   |
   |                                   | ::                                                                                                                                |
   |                                   |                                                                                                                                   |
   |                                   |    SELECT timestamp '2001-09-28 01:00' + interval '23 hours' AS RESULT;                                                           |
   |                                   |           result                                                                                                                  |
   |                                   |    ---------------------                                                                                                          |
   |                                   |     2001-09-29 00:00:00                                                                                                           |
   |                                   |    (1 row)                                                                                                                        |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------+
   |                                   | Add a time with an interval to obtain the time after three hours.                                                                 |
   |                                   |                                                                                                                                   |
   |                                   | ::                                                                                                                                |
   |                                   |                                                                                                                                   |
   |                                   |    SELECT time '01:00' + interval '3 hours' AS RESULT;                                                                            |
   |                                   |      result                                                                                                                       |
   |                                   |    ----------                                                                                                                     |
   |                                   |     04:00:00                                                                                                                      |
   |                                   |    (1 row)                                                                                                                        |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------+
   | ``-``                             | Subtract a date from another to obtain the difference.                                                                            |
   |                                   |                                                                                                                                   |
   |                                   | ::                                                                                                                                |
   |                                   |                                                                                                                                   |
   |                                   |    SELECT date '2001-10-01' - date '2001-09-28' AS RESULT;                                                                        |
   |                                   |     result                                                                                                                        |
   |                                   |    --------                                                                                                                       |
   |                                   |     3 days                                                                                                                        |
   |                                   |    (1 row)                                                                                                                        |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------+
   |                                   | Subtract an integer from a date, the return is a timestamp type.                                                                  |
   |                                   |                                                                                                                                   |
   |                                   | ::                                                                                                                                |
   |                                   |                                                                                                                                   |
   |                                   |    SELECT date '2001-10-01' - integer '7' AS RESULT;                                                                              |
   |                                   |           result                                                                                                                  |
   |                                   |    ---------------------                                                                                                          |
   |                                   |     2001-09-24 00:00:00                                                                                                           |
   |                                   |    (1 row)                                                                                                                        |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------+
   |                                   | Subtract an interval from a date to obtain the time difference.                                                                   |
   |                                   |                                                                                                                                   |
   |                                   | ::                                                                                                                                |
   |                                   |                                                                                                                                   |
   |                                   |    SELECT date '2001-09-28' - interval '1 hour' AS RESULT;                                                                        |
   |                                   |           result                                                                                                                  |
   |                                   |    ---------------------                                                                                                          |
   |                                   |     2001-09-27 23:00:00                                                                                                           |
   |                                   |    (1 row)                                                                                                                        |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------+
   |                                   | Subtract a time from another time to obtain the time difference.                                                                  |
   |                                   |                                                                                                                                   |
   |                                   | ::                                                                                                                                |
   |                                   |                                                                                                                                   |
   |                                   |    SELECT time '05:00' - time '03:00' AS RESULT;                                                                                  |
   |                                   |      result                                                                                                                       |
   |                                   |    ----------                                                                                                                     |
   |                                   |     02:00:00                                                                                                                      |
   |                                   |    (1 row)                                                                                                                        |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------+
   |                                   | Subtract an interval from a time to obtain the time difference.                                                                   |
   |                                   |                                                                                                                                   |
   |                                   | ::                                                                                                                                |
   |                                   |                                                                                                                                   |
   |                                   |    SELECT time '05:00' - interval '2 hours' AS RESULT;                                                                            |
   |                                   |      result                                                                                                                       |
   |                                   |    ----------                                                                                                                     |
   |                                   |     03:00:00                                                                                                                      |
   |                                   |    (1 row)                                                                                                                        |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------+
   |                                   | Subtract an interval from a timestamp to obtain the date difference.                                                              |
   |                                   |                                                                                                                                   |
   |                                   | ::                                                                                                                                |
   |                                   |                                                                                                                                   |
   |                                   |    SELECT timestamp '2001-09-28 23:00' - interval '23 hours' AS RESULT;                                                           |
   |                                   |           result                                                                                                                  |
   |                                   |    ---------------------                                                                                                          |
   |                                   |     2001-09-28 00:00:00                                                                                                           |
   |                                   |    (1 row)                                                                                                                        |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------+
   |                                   | Subtract an interval from another interval to obtain the time difference.                                                         |
   |                                   |                                                                                                                                   |
   |                                   | ::                                                                                                                                |
   |                                   |                                                                                                                                   |
   |                                   |    SELECT interval '1 day' - interval '1 hour' AS RESULT;                                                                         |
   |                                   |      result                                                                                                                       |
   |                                   |    ----------                                                                                                                     |
   |                                   |     23:00:00                                                                                                                      |
   |                                   |    (1 row)                                                                                                                        |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------+
   |                                   | Subtract a timestamp from another timestamp to obtain the time difference.                                                        |
   |                                   |                                                                                                                                   |
   |                                   | ::                                                                                                                                |
   |                                   |                                                                                                                                   |
   |                                   |    SELECT timestamp '2001-09-29 03:00' - timestamp '2001-09-27 12:00' AS RESULT;                                                  |
   |                                   |         result                                                                                                                    |
   |                                   |    ----------------                                                                                                               |
   |                                   |     1 day 15:00:00                                                                                                                |
   |                                   |    (1 row)                                                                                                                        |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------+
   |                                   | Obtain the time at the previous day.                                                                                              |
   |                                   |                                                                                                                                   |
   |                                   | ::                                                                                                                                |
   |                                   |                                                                                                                                   |
   |                                   |    select now() - interval '1 day'AS RESULT;                                                                                      |
   |                                   |               result                                                                                                              |
   |                                   |    -------------------------------                                                                                                |
   |                                   |     2022-08-08 01:46:15.555406+00                                                                                                 |
   |                                   |    (1 row)                                                                                                                        |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------+
   | \*                                | Multiply an interval by a quantity:                                                                                               |
   |                                   |                                                                                                                                   |
   |                                   | ::                                                                                                                                |
   |                                   |                                                                                                                                   |
   |                                   |    SELECT 900 * interval '1 second' AS RESULT;                                                                                    |
   |                                   |      result                                                                                                                       |
   |                                   |    ----------                                                                                                                     |
   |                                   |     00:15:00                                                                                                                      |
   |                                   |    (1 row)                                                                                                                        |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------+
   |                                   | ::                                                                                                                                |
   |                                   |                                                                                                                                   |
   |                                   |    SELECT 21 * interval '1 day' AS RESULT;                                                                                        |
   |                                   |     result                                                                                                                        |
   |                                   |    ---------                                                                                                                      |
   |                                   |     21 days                                                                                                                       |
   |                                   |    (1 row)                                                                                                                        |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------+
   |                                   | ::                                                                                                                                |
   |                                   |                                                                                                                                   |
   |                                   |    SELECT double precision '3.5' * interval '1 hour' AS RESULT;                                                                   |
   |                                   |      result                                                                                                                       |
   |                                   |    ----------                                                                                                                     |
   |                                   |     03:30:00                                                                                                                      |
   |                                   |    (1 row)                                                                                                                        |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------+
   | /                                 | Divide an interval by a quantity to obtain a time segment.                                                                        |
   |                                   |                                                                                                                                   |
   |                                   | ::                                                                                                                                |
   |                                   |                                                                                                                                   |
   |                                   |    SELECT interval '1 hour' / double precision '1.5' AS RESULT;                                                                   |
   |                                   |      result                                                                                                                       |
   |                                   |    ----------                                                                                                                     |
   |                                   |     00:40:00                                                                                                                      |
   |                                   |    (1 row)                                                                                                                        |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------+

Time/Date functions
-------------------

-  age(timestamp, timestamp)

   Description: Subtracts arguments, producing a result in YYYY-MM-DD format. If the result is negative, the returned result is also negative.

   Return type: interval

   For example:

   ::

      SELECT age(timestamp '2001-04-10', timestamp '1957-06-13');
                 age
      -------------------------
       43 years 9 mons 27 days
      (1 row)

-  age(timestamp)

   Description: Subtracts from **current_date**

   Return type: interval

   For example:

   ::

      SELECT age(timestamp '1957-06-13');
                 age
      -------------------------
       60 years 2 mons 18 days
      (1 row)

-  timestampdiff(field, timestamp1, timestamp2)

   Description: Subtracts **timestamp1** from **timestamp2** and returns the difference in the unit of **field**. If the difference is negative, this function returns it normally. The **field** can be **day**, **month**, **quarter**, **day**, **week**, **hour**, **minute**, **second**, or **microsecond**.

   Return type: bigint

   For example:

   .. code-block::

      SELECT timestampdiff(day, timestamp '2001-02-01', timestamp '2003-05-01 12:05:55');
       timestampdiff
      ---------------
            819
      (1 row)

-  clock_timestamp()

   Description: Specifies the current timestamp of the real-time clock.

   Return type: timestamp with time zone

   For example:

   ::

      SELECT clock_timestamp();
              clock_timestamp
      -------------------------------
       2017-09-01 16:57:36.636205+08
      (1 row)

-  current_date

   Description: Current date

   Return type: date

   For example:

   ::

      SELECT current_date;
          date
      ------------
       2017-09-01
      (1 row)

-  current_time

   Description: Current time

   Return type: time with time zone

   For example:

   ::

      SELECT current_time;
             timetz
      --------------------
       16:58:07.086215+08
      (1 row)

-  current_timestamp

   Description: Specifies the current date and time.

   Return type: timestamp with time zone

   For example:

   ::

      SELECT current_timestamp;
             pg_systimestamp
      ------------------------------
       2017-09-01 16:58:19.22173+08
      (1 row)

-  date_part(text, timestamp)

   Description:

   Description: Obtains the hour.

   Equivalent to **extract(field from timestamp)**.

   Return type: double precision

   For example:

   ::

      SELECT date_part('hour', timestamp '2001-02-16 20:38:40');
       date_part
      -----------
              20
      (1 row)

-  date_part(text, interval)

   Description:

   Obtains the month. If the value is greater than 12, obtain the remainder after it is divided by 12.

   Equivalent to **extract(field from timestamp)**.

   Return type: double precision

   For example:

   ::

      SELECT date_part('month', interval '2 years 3 months');
       date_part
      -----------
               3
      (1 row)

-  date_trunc(text, timestamp)

   Description: Truncates to the precision specified by **text**.

   Return type: timestamp

   For example:

   ::

      SELECT date_trunc('hour', timestamp  '2001-02-16 20:38:40');
           date_trunc
      ---------------------
       2001-02-16 20:00:00
      (1 row)

-  trunc(timestamp)

   Description: By default, the data is intercepted by day.

   For example:

   ::

      SELECT trunc(timestamp  '2001-02-16 20:38:40');                                                                                                                                                                   trunc
      ---------------------
      2001-02-16 00:00:00
      (1 row)

-  extract(field from timestamp)

   Description: Obtains the hour.

   Return type: double precision

   For example:

   ::

      SELECT extract(hour from timestamp '2001-02-16 20:38:40');
       date_part
      -----------
              20
      (1 row)

-  extract(field from interval)

   Description: Obtains the month. If the value is greater than 12, obtain the remainder after it is divided by 12.

   Return type: double precision

   For example:

   ::

      SELECT extract(month from interval '2 years 3 months');
       date_part
      -----------
               3
      (1 row)

-  isfinite(date)

   Description: Tests for valid date.

   Return type: boolean

   For example:

   ::

      SELECT isfinite(date '2001-02-16');
       isfinite
      ----------
       t
      (1 row)

-  isfinite(timestamp)

   Description: Tests for valid timestamp.

   Return type: boolean

   For example:

   ::

      SELECT isfinite(timestamp '2001-02-16 21:28:30');
       isfinite
      ----------
       t
      (1 row)

-  isfinite(interval)

   Description: Tests for valid interval.

   Return type: boolean

   For example:

   ::

      SELECT isfinite(interval '4 hours');
       isfinite
      ----------
       t
      (1 row)

-  justify_days(interval)

   Description: Adjusts interval to 30-day time periods are represented as months

   Return type: interval

   For example:

   ::

      SELECT justify_days(interval '35 days');
       justify_days
      --------------
       1 mon 5 days
      (1 row)

-  justify_hours(interval)

   Description: Adjusts interval to 24-hour time periods are represented as days

   Return type: interval

   For example:

   ::

      SELECT JUSTIFY_HOURS(INTERVAL '27 HOURS');
       justify_hours
      ----------------
       1 day 03:00:00
      (1 row)

-  justify_interval(interval)

   Description: Adjusts **interval** using **justify_days** and **justify_hours**.

   Return type: interval

   For example:

   ::

      SELECT JUSTIFY_INTERVAL(INTERVAL '1 MON -1 HOUR');
       justify_interval
      ------------------
       29 days 23:00:00
      (1 row)

-  localtime

   Description: Current time

   Return type: time

   For example:

   ::

      SELECT localtime AS RESULT;
           result
      ----------------
       16:05:55.664681
      (1 row)

-  localtimestamp

   Description: Specifies the current date and time.

   Return type: timestamp

   For example:

   ::

      SELECT localtimestamp;
               timestamp
      ----------------------------
       2017-09-01 17:03:30.781902
      (1 row)

-  now()

   Description: Timestamp indicating the start of the current transaction.

   Return type: timestamp with time zone

   For example:

   ::

      SELECT now();
                    now
      -------------------------------
       2017-09-01 17:03:42.549426+08
      (1 row)

-  numtodsinterval(num, interval_unit)

   Description: Converts a number to the interval type. **num** is a numeric-typed number. **interval_unit** is a string in the following format: 'DAY' \| 'HOUR' \| 'MINUTE' \| 'SECOND'

   You can set the **IntervalStyle** parameter to **oracle** to be compatible with the interval output format of the function in the Oracle database.

   For example:

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

-  pg_sleep(seconds)

   Description: Specifies the delay time of the server thread in unit of second.

   Return type: void

   For example:

   ::

      SELECT pg_sleep(10);
       pg_sleep
      ----------

      (1 row)

-  statement_timestamp()

   Description: Specifies the current date and time.

   Return type: timestamp with time zone

   For example:

   ::

      SELECT statement_timestamp();
            statement_timestamp
      -------------------------------
       2017-09-01 17:04:39.119267+08
      (1 row)

-  sysdate

   Description: Specifies the current date and time.

   Return type: timestamp

   For example:

   ::

      SELECT sysdate;
             sysdate
      ---------------------
       2017-09-01 17:04:49
      (1 row)

-  timeofday()

   Description: Current date and time (like **clock_timestamp**, but returned as a **text** string)

   Return type: text

   For example:

   ::

      SELECT timeofday();
                    timeofday
      -------------------------------------
       Fri Sep 01 17:05:01.167506 2017 CST
      (1 row)

-  transaction_timestamp()

   Description: Current date and time (equivalent to **current_timestamp**)

   Return type: timestamp with time zone

   For example:

   ::

      SELECT transaction_timestamp();
           transaction_timestamp
      -------------------------------
       2017-09-01 17:05:13.534454+08
      (1 row)

-  add_months(d,n)

   Description: Calculates the time point day and time of nth months.

   Return type: timestamp

   For example:

   ::

      SELECT add_months(to_date('2017-5-29', 'yyyy-mm-dd'), 11) FROM dual;
           add_months
      ---------------------
       2018-04-29 00:00:00
      (1 row)

-  last_day(d)

   Description: Calculates the time of the last day in the month.

   -  In the ORA- or TD-compatible mode, the return type is timestamp.
   -  In the MySQL-compatible mode, the return type is date.

   For example:

   ::

      select last_day(to_date('2017-01-01', 'YYYY-MM-DD')) AS cal_result;
           cal_result
      ---------------------
       2017-01-31 00:00:00
      (1 row)

-  next_day(x,y)

   Description: Calculates the time of the next week y started from x

   -  In the ORA- or TD-compatible mode, the return type is timestamp.
   -  In the MySQL-compatible mode, the return type is date.

   For example:

   ::

      select next_day(timestamp '2017-05-25 00:00:00','Sunday')AS cal_result;
           cal_result
      ---------------------
       2017-05-28 00:00:00
      (1 row)

-  to_days(timestamp)

   Description: Returns the number of days from the first day of year 0 to a specified date.

   Return type: int

   For example:

   ::

      SELECT to_days(timestamp '2008-10-07');
       to_days
      ---------
        733687
      (1 row)

.. _en-us_topic_0000001099150944__s46aae9f2fd354b0881ea30d645533c72:

EXTRACT
-------

**EXTRACT(**\ *field* **FROM** *source*\ **)**

The **extract** function retrieves subcolumns such as year or hour from date/time values. **source** must be a value expression of type **timestamp**, **time**, or **interval**. (Expressions of type **date** are cast to **timestamp** and can therefore be used as well.) **field** is an identifier or string that selects what column to extract from the source value. The **extract** function returns values of type **double precision**. The following are valid field names:

-  century

   Century

   The first century starts at 0001-01-01 00:00:00 AD. This definition applies to all Gregorian calendar countries. There is no century number 0. You go from **-1** century to **1** century.

   For example:

   ::

      SELECT EXTRACT(CENTURY FROM TIMESTAMP '2000-12-16 12:21:13');
       date_part
      -----------
              20
      (1 row)

-  day

   -  For **timestamp** values, the day (of the month) column (1-31)

      ::

         SELECT EXTRACT(DAY FROM TIMESTAMP '2001-02-16 20:38:40');
          date_part
         -----------
                 16
         (1 row)

   -  For **interval** values, the number of days

      ::

         SELECT EXTRACT(DAY FROM INTERVAL '40 days 1 minute');
          date_part
         -----------
                 40
         (1 row)

-  decade

   Year column divided by 10

   ::

      SELECT EXTRACT(DECADE FROM TIMESTAMP '2001-02-16 20:38:40');
       date_part
      -----------
             200
      (1 row)

-  dow

   Day of the week as Sunday(**0**) to Saturday (**6**)

   ::

      SELECT EXTRACT(DOW FROM TIMESTAMP '2001-02-16 20:38:40');
       date_part
      -----------
               5
      (1 row)

-  doy

   Day of the year (1-365 or 366)

   ::

      SELECT EXTRACT(DOY FROM TIMESTAMP '2001-02-16 20:38:40');
       date_part
      -----------
              47
      (1 row)

-  epoch

   -  For **timestamp with time zone** values, the number of seconds since 1970-01-01 00:00:00 UTC (can be negative);

      for **date** and **timestamp** values, the number of seconds since 1970-01-01 00:00:00 local time;

      for **interval** values, the total number of seconds in the interval.

      ::

         SELECT EXTRACT(EPOCH FROM TIMESTAMP WITH TIME ZONE '2001-02-16 20:38:40.12-08');
           date_part
         --------------
          982384720.12
         (1 row)

      ::

         SELECT EXTRACT(EPOCH FROM INTERVAL '5 days 3 hours');
          date_part
         -----------
             442800
         (1 row)

   -  Way to convert an epoch value back to a timestamp

      ::

         SELECT TIMESTAMP WITH TIME ZONE 'epoch' + 982384720.12 * INTERVAL '1 second' AS RESULT;
                   result
         ---------------------------
          2001-02-17 12:38:40.12+08
         (1 row)

-  hour

   Hour column (0-23)

   ::

      SELECT EXTRACT(HOUR FROM TIMESTAMP '2001-02-16 20:38:40');
       date_part
      -----------
              20
      (1 row)

-  isodow

   Day of the week (1-7)

   Monday is 1 and Sunday is 7.

   .. note::

      This is identical to **dow** except for Sunday.

   ::

      SELECT EXTRACT(ISODOW FROM TIMESTAMP '2001-02-18 20:38:40');
       date_part
      -----------
               7
      (1 row)

-  isoyear

   The ISO 8601 year that the date falls in (not applicable to intervals).

   Each ISO year begins with the Monday of the week containing the 4th of January, so in early January or late December the ISO year may be different from the Gregorian year. See the **week** column for more information.

   ::

      SELECT EXTRACT(ISOYEAR FROM DATE '2006-01-01');
       date_part
      -----------
            2005
      (1 row)

   ::

      SELECT EXTRACT(ISOYEAR FROM DATE '2006-01-02');
       date_part
      -----------
            2006
      (1 row)

-  microseconds

   The seconds column, including fractional parts, multiplied by 1,000,000

   ::

      SELECT EXTRACT(MICROSECONDS FROM TIME '17:12:28.5');
       date_part
      -----------
        28500000
      (1 row)

-  millennium

   Millennium

   Years in the 1900s are in the second millennium. The third millennium started from January 1, 2001.

   ::

      SELECT EXTRACT(MILLENNIUM FROM TIMESTAMP '2001-02-16 20:38:40');
       date_part
      -----------
               3
      (1 row)

-  milliseconds

   The seconds column, including fractional parts, multiplied by 1000. Note that this includes full seconds.

   ::

      SELECT EXTRACT(MILLISECONDS FROM TIME '17:12:28.5');
       date_part
      -----------
           28500
      (1 row)

-  minute

   Minutes column (0-59)

   ::

      SELECT EXTRACT(MINUTE FROM TIMESTAMP '2001-02-16 20:38:40');
       date_part
      -----------
              38
      (1 row)

-  month

   For **timestamp** values, the number of the month within the year (1-12);

   ::

      SELECT EXTRACT(MONTH FROM TIMESTAMP '2001-02-16 20:38:40');
       date_part
      -----------
               2
      (1 row)

   For **interval** values, the number of months, modulo 12 (0-11)

   ::

      SELECT EXTRACT(MONTH FROM INTERVAL '2 years 13 months');
       date_part
      -----------
               1
      (1 row)

-  quarter

   Quarter of the year (1-4) that the date is in

   ::

      SELECT EXTRACT(QUARTER FROM TIMESTAMP '2001-02-16 20:38:40');
       date_part
      -----------
               1
      (1 row)

-  second

   Seconds column, including fractional parts (0-59)

   ::

      SELECT EXTRACT(SECOND FROM TIME '17:12:28.5');
       date_part
      -----------
            28.5
      (1 row)

-  timezone

   The time zone offset from UTC, measured in seconds. Positive values correspond to time zones east of UTC, negative values to zones west of UTC.

-  timezone_hour

   The hour component of the time zone offset

-  timezone_minute

   The minute component of the time zone offset

-  week

   The number of the week of the year that the day is in. By definition (ISO 8601), the first week of a year contains January 4 of that year. (The ISO-8601 week starts on Monday.) In other words, the first Thursday of a year is in week 1 of that year.

   Because of this, it is possible for early January dates to be part of the 52nd or 53rd week of the previous year, and late December dates to be part of the 1st week of the next year. For example, **2005-01-01** is part of the 53rd week of year 2004, **2006-01-01** is part of the 52nd week of year 2005, and **2012-12-31** is part of the 1st week of year 2013. You are advised to use the columns **isoyear** and **week** together to ensure consistency.

   ::

      SELECT EXTRACT(WEEK FROM TIMESTAMP '2001-02-16 20:38:40');
       date_part
      -----------
               7
      (1 row)

-  year

   Year column

   ::

      SELECT EXTRACT(YEAR FROM TIMESTAMP '2001-02-16 20:38:40');
       date_part
      -----------
            2001
      (1 row)

date_part
---------

The **date_part** function is modeled on the traditional Ingres equivalent to the SQL-standard function **extract**:

**date_part('**\ *field*\ **',** *source*\ **)**

Note that the **field** must be a string, rather than a name. The valid field names are the same as those for **extract**. For details, see :ref:`EXTRACT <en-us_topic_0000001099150944__s46aae9f2fd354b0881ea30d645533c72>`.

For example:

::

   SELECT date_part('day', TIMESTAMP '2001-02-16 20:38:40');
    date_part
   -----------
           16
   (1 row)

::

   SELECT date_part('hour', INTERVAL '4 hours 3 minutes');
    date_part
   -----------
            4
   (1 row)

date_format
-----------

**date_format(**\ timestamp, fmt\ **)**

Converts a date into a string in the format specified by **fmt**.

For example:

::

   SELECT date_format('2009-10-04 22:23:00', '%M %D %W');
       date_format
   --------------------
    October 4th Sunday
   (1 row)
   SELECT date_format('2021-02-20 08:30:45', '%Y-%m-%d %H:%i:%S');
        date_format
   ---------------------
    2021-02-20 08:30:45
   (1 row)
   SELECT date_format('2021-02-20 18:10:15', '%r-%T');
        date_format
   ----------------------
    06:10:15 PM-18:10:15
   (1 row)

The following table describes the patterns of date parameter values. They can be used for the **date_format**, **time_format**, **str_to_date**, **str_to_time**, and **from_unixtime** functions.

.. table:: **Table 2** Output formats of date_format

   +--------+-----------------------------------------------------------------------------------------+-------------------------+
   | Format | Description                                                                             | Value                   |
   +========+=========================================================================================+=========================+
   | %a     | Abbreviated week name                                                                   | Sun...Sat               |
   +--------+-----------------------------------------------------------------------------------------+-------------------------+
   | %b     | Abbreviated month name                                                                  | Jan...Dec               |
   +--------+-----------------------------------------------------------------------------------------+-------------------------+
   | %c     | Month                                                                                   | 0...12                  |
   +--------+-----------------------------------------------------------------------------------------+-------------------------+
   | %D     | Date with a suffix                                                                      | 0th, 1st, 2nd, 3rd, ... |
   +--------+-----------------------------------------------------------------------------------------+-------------------------+
   | %d     | Day in a month (two digits)                                                             | 00...31                 |
   +--------+-----------------------------------------------------------------------------------------+-------------------------+
   | %e     | Day in a month                                                                          | 0...31                  |
   +--------+-----------------------------------------------------------------------------------------+-------------------------+
   | %f     | Microsecond                                                                             | 000000...999999         |
   +--------+-----------------------------------------------------------------------------------------+-------------------------+
   | %H     | Hour, in 24-hour format                                                                 | 00...23                 |
   +--------+-----------------------------------------------------------------------------------------+-------------------------+
   | %h     | Hour, in 12-hour format                                                                 | 01...12                 |
   +--------+-----------------------------------------------------------------------------------------+-------------------------+
   | %I     | Hour, in 12-hour format, same as **%h**                                                 | 01...12                 |
   +--------+-----------------------------------------------------------------------------------------+-------------------------+
   | %i     | Minute                                                                                  | 00...59                 |
   +--------+-----------------------------------------------------------------------------------------+-------------------------+
   | %j     | Day in a year                                                                           | 001...366               |
   +--------+-----------------------------------------------------------------------------------------+-------------------------+
   | %k     | Hour, in 24-hour format, same as **%H**                                                 | 0...23                  |
   +--------+-----------------------------------------------------------------------------------------+-------------------------+
   | %l     | Hour, in 12-hour format, same as **%h**                                                 | 1...12                  |
   +--------+-----------------------------------------------------------------------------------------+-------------------------+
   | %M     | Month name                                                                              | January...December      |
   +--------+-----------------------------------------------------------------------------------------+-------------------------+
   | %m     | Month (two digits)                                                                      | 00...12                 |
   +--------+-----------------------------------------------------------------------------------------+-------------------------+
   | %p     | Morning and afternoon                                                                   | AM PM                   |
   +--------+-----------------------------------------------------------------------------------------+-------------------------+
   | %r     | Time, in 12-hour format                                                                 | hh::mm::ss AM/PM        |
   +--------+-----------------------------------------------------------------------------------------+-------------------------+
   | %S     | Second                                                                                  | 00...59                 |
   +--------+-----------------------------------------------------------------------------------------+-------------------------+
   | %s     | Second, same as **%S**                                                                  | 00...59                 |
   +--------+-----------------------------------------------------------------------------------------+-------------------------+
   | %T     | Time, in 24-hour format                                                                 | hh::mm::ss              |
   +--------+-----------------------------------------------------------------------------------------+-------------------------+
   | %U     | Week (Sunday is the first day of a week)                                                | 00...53                 |
   +--------+-----------------------------------------------------------------------------------------+-------------------------+
   | %u     | Week (Monday is the first day of a week)                                                | 00...53                 |
   +--------+-----------------------------------------------------------------------------------------+-------------------------+
   | %V     | Week (Sunday is the first day of a week). It is used together with **%X**.              | 01...53                 |
   +--------+-----------------------------------------------------------------------------------------+-------------------------+
   | %v     | Week (Monday is the first day of a week). It is used together with **%x**.              | 01...53                 |
   +--------+-----------------------------------------------------------------------------------------+-------------------------+
   | %W     | Week name                                                                               | Sunday...Saturday       |
   +--------+-----------------------------------------------------------------------------------------+-------------------------+
   | %w     | Day of a week. The value is **0** for Sunday.                                           | 0...6                   |
   +--------+-----------------------------------------------------------------------------------------+-------------------------+
   | %X     | Year (four digits). It is used together with **%V**. Sunday is the first day of a week. | ``-``                   |
   +--------+-----------------------------------------------------------------------------------------+-------------------------+
   | %x     | Year (four digits). It is used together with **%v**. Monday is the first day of a week. | ``-``                   |
   +--------+-----------------------------------------------------------------------------------------+-------------------------+
   | %Y     | Year (four digits)                                                                      | ``-``                   |
   +--------+-----------------------------------------------------------------------------------------+-------------------------+
   | %y     | Year (two digits)                                                                       | ``-``                   |
   +--------+-----------------------------------------------------------------------------------------+-------------------------+
   | %%     | Character '%'                                                                           | Character '%'           |
   +--------+-----------------------------------------------------------------------------------------+-------------------------+
   | %\ *x* | '*x*': any character apart from the preceding ones                                      | Character '*x*'         |
   +--------+-----------------------------------------------------------------------------------------+-------------------------+

.. important::

   In the preceding table, **%U**, **%u**, **%V**, **%v**, **%X**, and **%x** are not supported currently.
