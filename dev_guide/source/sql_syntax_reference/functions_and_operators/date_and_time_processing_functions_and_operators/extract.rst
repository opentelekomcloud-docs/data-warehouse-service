:original_name: dws_06_0310.html

.. _dws_06_0310:

EXTRACT
=======

**EXTRACT(**\ *field* **FROM** *source*\ **)**

The **extract** function retrieves subcolumns such as year or hour from date/time values. **source** must be a value expression of type **timestamp**, **time**, or **interval**. (Expressions of type **date** are cast to **timestamp** and can therefore be used as well.) **field** is an identifier or string that selects what column to extract from the source value. The **extract** function returns values of type **double precision**. The following are valid field names:

century
-------

Century

The first century starts at 0001-01-01 00:00:00 AD. This definition applies to all Gregorian calendar countries. There is no century number 0. You go from **-1** century to **1** century.

Example:

::

   SELECT EXTRACT(century FROM TIMESTAMP '2000-12-16 12:21:13');
    date_part
   -----------
           20
   (1 row)

day
---

-  For **timestamp** values, the day (of the month) column (1-31)

   ::

      SELECT EXTRACT(day FROM TIMESTAMP '2001-02-16 20:38:40');
       date_part
      -----------
              16
      (1 row)

-  For **interval** values, the number of days

   ::

      SELECT EXTRACT(day FROM INTERVAL '40 days 1 minute');
       date_part
      -----------
              40
      (1 row)

decade
------

Year column divided by 10

::

   SELECT EXTRACT(decade FROM TIMESTAMP '2001-02-16 20:38:40');
    date_part
   -----------
          200
   (1 row)

dow
---

Day of the week as Sunday(**0**) to Saturday (**6**)

::

   SELECT EXTRACT(dow FROM TIMESTAMP '2001-02-16 20:38:40');
    date_part
   -----------
            5
   (1 row)

doy
---

Day of the year (1-365 or 366)

::

   SELECT EXTRACT(doy FROM TIMESTAMP '2001-02-16 20:38:40');
    date_part
   -----------
           47
   (1 row)

epoch
-----

-  For **timestamp with time zone** values, the number of seconds since 1970-01-01 00:00:00 UTC (can be negative);

   for **date** and **timestamp** values, the number of seconds since 1970-01-01 00:00:00 local time;

   for **interval** values, the total number of seconds in the interval.

   ::

      SELECT EXTRACT(epoch FROM TIMESTAMP WITH TIME ZONE '2001-02-16 20:38:40.12-08');
        date_part
      --------------
       982384720.12
      (1 row)

   ::

      SELECT EXTRACT(epoch FROM interval '5 days 3 hours');
       date_part
      -----------
          442800
      (1 row)

-  Way to convert an epoch value back to a timestamp

   ::

      SELECT TIMESTAMP WITH TIME ZONE 'epoch' + 982384720.12 * interval '1 second' AS RESULT;
                result
      ---------------------------
       2001-02-17 12:38:40.12+08
      (1 row)

hour
----

Hour column (0-23)

::

   SELECT EXTRACT(hour FROM TIMESTAMP '2001-02-16 20:38:40');
    date_part
   -----------
           20
   (1 row)

isodow
------

Day of the week (1-7)

Monday is 1 and Sunday is 7.

.. note::

   This is identical to **dow** except for Sunday.

::

   SELECT EXTRACT(isodow FROM TIMESTAMP '2001-02-18 20:38:40');
    date_part
   -----------
            7
   (1 row)

isoyear
-------

The ISO 8601 year that the date falls in (not applicable to intervals).

Each ISO year begins with the Monday of the week containing the 4th of January, so in early January or late December the ISO year may be different from the Gregorian year. See the **week** column for more information.

::

   SELECT EXTRACT(isoyear FROM DATE '2006-01-01');
    date_part
   -----------
         2005
   (1 row)

::

   SELECT EXTRACT(isoyear FROM DATE '2006-01-02');
    date_part
   -----------
         2006
   (1 row)

microseconds
------------

The seconds column, including fractional parts, multiplied by 1,000,000

::

   SELECT EXTRACT(microseconds FROM TIME '17:12:28.5');
    date_part
   -----------
     28500000
   (1 row)

millennium
----------

Millennium

Years in the 1900s are in the second millennium. The third millennium started from January 1, 2001.

::

   SELECT EXTRACT(millennium FROM TIMESTAMP '2001-02-16 20:38:40');
    date_part
   -----------
            3
   (1 row)

milliseconds
------------

The seconds column, including fractional parts, multiplied by 1000. Note that this includes full seconds.

::

   SELECT EXTRACT(milliseconds FROM TIME '17:12:28.5');
    date_part
   -----------
        28500
   (1 row)

minute
------

Minutes column (0-59)

::

   SELECT EXTRACT(minute FROM TIMESTAMP '2001-02-16 20:38:40');
    date_part
   -----------
           38
   (1 row)

month
-----

For **timestamp** values, the number of the month within the year (1-12);

::

   SELECT EXTRACT(month FROM TIMESTAMP '2001-02-16 20:38:40');
    date_part
   -----------
            2
   (1 row)

For **interval** values, the number of months, modulo 12 (0-11)

::

   SELECT EXTRACT(month FROM interval '2 years 13 months');
    date_part
   -----------
            1
   (1 row)

quarter
-------

Quarter of the year (1-4) that the date is in

::

   SELECT EXTRACT(quarter FROM TIMESTAMP '2001-02-16 20:38:40');
    date_part
   -----------
            1
   (1 row)

second
------

Seconds column, including fractional parts (0-59)

::

   SELECT EXTRACT(second FROM TIME '17:12:28.5');
    date_part
   -----------
         28.5
   (1 row)

timezone
--------

The time zone offset from UTC, measured in seconds. Positive values correspond to time zones east of UTC, negative values to zones west of UTC.

::

   SELECT EXTRACT(timezone FROM TIMETZ '17:12:28');
    date_part
   -----------
       0
   (1 row)

timezone_hour
-------------

The hour component of the time zone offset

::

   SELECT EXTRACT(timezone_hour FROM TIMETZ '17:12:28');
    date_part
   -----------
          0
   (1 row)

timezone_minute
---------------

The minute component of the time zone offset

::

   SELECT EXTRACT(timezone_minute FROM TIMETZ '17:12:28');
    date_part
   -----------
            0
   (1 row)

week
----

The number of the week of the year that the day is in. By definition (ISO 8601), the first week of a year contains January 4 of that year. (The ISO-8601 week starts on Monday.) In other words, the first Thursday of a year is in week 1 of that year.

Because of this, it is possible for early January dates to be part of the 52nd or 53rd week of the previous year, and late December dates to be part of the 1st week of the next year. For example, **2005-01-01** is part of the 53rd week of year 2004, **2006-01-01** is part of the 52nd week of year 2005, and **2012-12-31** is part of the 1st week of year 2013. You are advised to use the columns **isoyear** and **week** together to ensure consistency.

::

   SELECT EXTRACT(week FROM TIMESTAMP '2001-02-16 20:38:40');
    date_part
   -----------
            7
   (1 row)

year
----

Year column

::

   SELECT EXTRACT(year FROM TIMESTAMP '2001-02-16 20:38:40');
    date_part
   -----------
         2001
   (1 row)
