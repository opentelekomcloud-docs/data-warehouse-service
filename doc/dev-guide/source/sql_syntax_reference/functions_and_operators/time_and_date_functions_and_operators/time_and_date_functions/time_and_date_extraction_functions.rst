:original_name: dws_06_0313.html

.. _dws_06_0313:

Time and Date Extraction Functions
==================================

.. _en-us_topic_0000002541901151__section16931241799:

date_part(text, timestamp \| interval)
--------------------------------------

Description: Extracts a specified part (text) from a date or time expression (timestamp or interval). It is equivalent to the **extract(field from timestamp)** function.

Syntax:

.. code-block::

   date_part('field', source)

-  **source** must be a value expression of type **timestamp**, **time**, or **interval**. (Expressions of type **date** are cast to **timestamp** and can therefore be used as well.)
-  **field** is an identifier or a string that specifies the time part to be extracted from the source data. For the value of **field**, see :ref:`Table 1 <en-us_topic_0000002541901151__table469112361188>`.

Return type: double precision

Examples:

Obtain the hour of a specified date.

::

   SELECT date_part('hour', timestamp '2001-02-16 20:38:40');
    date_part
   -----------
           20
   (1 row)

Extract the month from the time interval 2 years 3 months.

::

   SELECT date_part('month', interval '2 years 3 months');
    date_part
   -----------
            3
   (1 row)

.. _en-us_topic_0000002541901151__section102671275565:

date_trunc(text, timestamp with time zone \| timestamp without time zone \| interval)
-------------------------------------------------------------------------------------

Description: Truncates a time or date to the specified precision (**text**).

**text** cam be **microseconds**, **milliseconds**, **second**, **minute**, **hour**, **day**, **week**, **month**, **quarter**, **year**, **decade**, **century**, and **millennium**.

Return type: timestamp with time zone, timestamp without time zone, and interval

Examples:

Truncate the input time to the hour.

::

   SELECT date_trunc('hour', timestamp '2025-02-16 20:38:40');
        date_trunc
   ---------------------
    2025-02-16 20:00:00
   (1 row)

Truncate the input time to the day.

::

   SELECT date_trunc('day', timestamptz'2025-02-16 20:38:40+08');
          date_trunc
   ------------------------
    2025-02-16 00:00:00-05
   (1 row)

Truncate the input time to the month.

::

   SELECT date_trunc('month', timestamp '2025-02-16 20:38:40');
        date_trunc
   ---------------------
    2025-02-01 00:00:00
   (1 row)

Truncate the input timestamp to the first day of a quarter.

::

   SELECT DATE_TRUNC('quarter', TIMESTAMP '20250430 04:05:06.789');
        date_trunc
   ---------------------
    2025-04-01 00:00:00
   (1 row)

Truncate the first day of the current month at 12:00.

::

   SELECT date_trunc('month',now()) +interval '12h';
           ?column?
   ------------------------
    2025-09-01 12:00:00-04
   (1 row)

Truncate 09:00 of the current day.

::

   SELECT date_trunc('day',now()) + interval '9h';
           ?column?
   ------------------------
    2025-09-22 09:00:00-04
   (1 row)

Truncate the first day of the current year.

::

   SELECT date_trunc('year',CURRENT_DATE);
          date_trunc
   ------------------------
    2025-01-01 00:00:00-05
   (1 row)

.. _en-us_topic_0000002541901151__section8731162365616:

trunc(timestamp)
----------------

Description: Truncates a timestamp to day.

Return type: timestamp

Example:

::

   SELECT trunc(timestamp '2001-02-16 20:38:40');                                                                                                                                                                   trunc
   ---------------------
   2001-02-16 00:00:00
   (1 row)

.. _en-us_topic_0000002541901151__section181911910131614:

extract(field from timestamp \| time \| interval)
-------------------------------------------------

Description: Extracts a specified time part from a date or time expression, for example, the year, month, and hour.

Syntax:

.. code-block::

   EXTRACT(field FROM source)

-  **source** must be a value expression of type **timestamp**, **time**, or **interval**. (Expressions of type **date** are cast to **timestamp** and can therefore be used as well.)
-  **field** is an identifier or a string that specifies the time part to be extracted from the source data. For the value of **field**, see :ref:`Table 1 <en-us_topic_0000002541901151__table469112361188>`.

Return type: double precision

.. _en-us_topic_0000002541901151__table469112361188:

.. table:: **Table 1** Values of field

   +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | Field                 | Example                                                                                                                                                                                                                    | Return Result         |
   +=======================+============================================================================================================================================================================================================================+=======================+
   | century               | Extract the century from the given timestamp.                                                                                                                                                                              | Return result:        |
   |                       |                                                                                                                                                                                                                            |                       |
   |                       | ::                                                                                                                                                                                                                         | ::                    |
   |                       |                                                                                                                                                                                                                            |                       |
   |                       |    SELECT EXTRACT(CENTURY FROM TIMESTAMP '2000-12-16 12:21:13');                                                                                                                                                           |    date_part          |
   |                       |                                                                                                                                                                                                                            |    -----------        |
   |                       | The first century starts at 0001-01-01 00:00:00 AD. This definition applies to all Gregorian calendar countries. There is no 0th century. The timeline transitions directly from the 1st century BC to the 1st century AD. |            20         |
   +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | millennium            | Extract the millennium from the specified timestamp.                                                                                                                                                                       | Return result:        |
   |                       |                                                                                                                                                                                                                            |                       |
   |                       | ::                                                                                                                                                                                                                         | ::                    |
   |                       |                                                                                                                                                                                                                            |                       |
   |                       |    SELECT EXTRACT(MILLENNIUM FROM TIMESTAMP '2001-02-16 20:38:40');                                                                                                                                                        |     date_part         |
   |                       |                                                                                                                                                                                                                            |    -----------        |
   |                       | Years in the 1900s are in the second millennium. The third millennium started from January 1, 2001.                                                                                                                        |             3         |
   |                       |                                                                                                                                                                                                                            |    (1 row)            |
   +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | decade                | Extract the decade from the specified timestamp.                                                                                                                                                                           | Return result:        |
   |                       |                                                                                                                                                                                                                            |                       |
   |                       | ::                                                                                                                                                                                                                         | ::                    |
   |                       |                                                                                                                                                                                                                            |                       |
   |                       |    SELECT EXTRACT(DECADE FROM TIMESTAMP '2001-02-16 20:38:40');                                                                                                                                                            |     date_part         |
   |                       |                                                                                                                                                                                                                            |    -----------        |
   |                       |                                                                                                                                                                                                                            |           200         |
   |                       |                                                                                                                                                                                                                            |    (1 row)            |
   +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | year                  | Extract the year from the specified timestamp.                                                                                                                                                                             | Return result:        |
   |                       |                                                                                                                                                                                                                            |                       |
   |                       | ::                                                                                                                                                                                                                         | ::                    |
   |                       |                                                                                                                                                                                                                            |                       |
   |                       |    SELECT EXTRACT(YEAR FROM TIMESTAMP '2001-02-16 20:38:40');                                                                                                                                                              |     date_part         |
   |                       |                                                                                                                                                                                                                            |    -----------        |
   |                       |                                                                                                                                                                                                                            |          2001         |
   |                       |                                                                                                                                                                                                                            |    (1 row)            |
   +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | quarter               | Extract the quarter from the specified timestamp. The value ranges from 1 to 4.                                                                                                                                            | Return result:        |
   |                       |                                                                                                                                                                                                                            |                       |
   |                       | ::                                                                                                                                                                                                                         | ::                    |
   |                       |                                                                                                                                                                                                                            |                       |
   |                       |    SELECT EXTRACT(QUARTER FROM TIMESTAMP '2001-02-16 20:38:40');                                                                                                                                                           |     date_part         |
   |                       |                                                                                                                                                                                                                            |    -----------        |
   |                       |                                                                                                                                                                                                                            |             1         |
   |                       |                                                                                                                                                                                                                            |    (1 row)            |
   +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | month                 | -  Extract the month number from the specified timestamp.                                                                                                                                                                  | -  Return result:     |
   |                       |                                                                                                                                                                                                                            |                       |
   |                       |    If **source** is **timestamp**, it indicates the month number within the year. The value ranges from 1 to 12.                                                                                                           |    ::                 |
   |                       |                                                                                                                                                                                                                            |                       |
   |                       |    ::                                                                                                                                                                                                                      |        date_part      |
   |                       |                                                                                                                                                                                                                            |       -----------     |
   |                       |       SELECT EXTRACT(MONTH FROM TIMESTAMP '2001-02-16 20:38:40');                                                                                                                                                          |                2      |
   |                       |                                                                                                                                                                                                                            |       (1 row)         |
   |                       | -  Extract the month from the time interval.                                                                                                                                                                               |                       |
   |                       |                                                                                                                                                                                                                            | -  Return result:     |
   |                       |    If **source** is **interval**, it indicates the number of months. The system automatically normalizes the months that exceed 12 into years.                                                                             |                       |
   |                       |                                                                                                                                                                                                                            |    ::                 |
   |                       |    ::                                                                                                                                                                                                                      |                       |
   |                       |                                                                                                                                                                                                                            |        date_part      |
   |                       |       SELECT EXTRACT(MONTH FROM INTERVAL '2 years 13 months');                                                                                                                                                             |       -----------     |
   |                       |                                                                                                                                                                                                                            |                1      |
   |                       |                                                                                                                                                                                                                            |       (1 row)         |
   +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | week                  | ISO 8601 standard. The first Thursday of a year is in the first week. Each week starts from Monday and ends on Sunday. The value ranges from 1 to 53.                                                                      | Return result:        |
   |                       |                                                                                                                                                                                                                            |                       |
   |                       | Obtain the week number of the given date in a year.                                                                                                                                                                        | ::                    |
   |                       |                                                                                                                                                                                                                            |                       |
   |                       | ::                                                                                                                                                                                                                         |     date_part         |
   |                       |                                                                                                                                                                                                                            |    -----------        |
   |                       |    SELECT EXTRACT(WEEK FROM TIMESTAMP '2001-02-16 20:38:40');                                                                                                                                                              |             7         |
   |                       |                                                                                                                                                                                                                            |    (1 row)            |
   +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | day                   | -  Extract the day in a month from a specified timestamp.                                                                                                                                                                  | -  Return result:     |
   |                       |                                                                                                                                                                                                                            |                       |
   |                       |    If **source** is **timestamp**, it indicates the day of a month (1-31).                                                                                                                                                 |    ::                 |
   |                       |                                                                                                                                                                                                                            |                       |
   |                       |    ::                                                                                                                                                                                                                      |        date_part      |
   |                       |                                                                                                                                                                                                                            |       -----------     |
   |                       |       SELECT EXTRACT(DAY FROM TIMESTAMP '2001-02-16 20:38:40');                                                                                                                                                            |               16      |
   |                       |                                                                                                                                                                                                                            |       (1 row)         |
   |                       | -  Extract the number of days from the interval.                                                                                                                                                                           |                       |
   |                       |                                                                                                                                                                                                                            | -  Return result:     |
   |                       |    If **source** is **interval**, it indicates the number of days.                                                                                                                                                         |                       |
   |                       |                                                                                                                                                                                                                            |    ::                 |
   |                       |    ::                                                                                                                                                                                                                      |                       |
   |                       |                                                                                                                                                                                                                            |        date_part      |
   |                       |       SELECT EXTRACT(DAY FROM INTERVAL '40 days 1 minute');                                                                                                                                                                |       -----------     |
   |                       |                                                                                                                                                                                                                            |               40      |
   |                       |                                                                                                                                                                                                                            |       (1 row)         |
   +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | hour                  | Extract the hour from the specified timestamp. The value ranges from 0 to 23.                                                                                                                                              | Return result:        |
   |                       |                                                                                                                                                                                                                            |                       |
   |                       | ::                                                                                                                                                                                                                         | ::                    |
   |                       |                                                                                                                                                                                                                            |                       |
   |                       |    SELECT EXTRACT(HOUR FROM TIMESTAMP '2001-02-16 20:38:40');                                                                                                                                                              |     date_part         |
   |                       |                                                                                                                                                                                                                            |    -----------        |
   |                       |                                                                                                                                                                                                                            |            20         |
   |                       |                                                                                                                                                                                                                            |    (1 row)            |
   +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | minute                | Extract the minute from the specified timestamp.                                                                                                                                                                           | Return result:        |
   |                       |                                                                                                                                                                                                                            |                       |
   |                       | ::                                                                                                                                                                                                                         | ::                    |
   |                       |                                                                                                                                                                                                                            |                       |
   |                       |    SELECT EXTRACT(MINUTE FROM TIMESTAMP '2001-02-16 20:38:40');                                                                                                                                                            |     date_part         |
   |                       |                                                                                                                                                                                                                            |    -----------        |
   |                       |                                                                                                                                                                                                                            |            38         |
   |                       |                                                                                                                                                                                                                            |    (1 row)            |
   +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | second                | Extract the second (including any fractional seconds) from the specified timestamp.                                                                                                                                        | Return result:        |
   |                       |                                                                                                                                                                                                                            |                       |
   |                       | ::                                                                                                                                                                                                                         | ::                    |
   |                       |                                                                                                                                                                                                                            |                       |
   |                       |    SELECT EXTRACT(SECOND FROM TIME '17:12:28.5');                                                                                                                                                                          |     date_part         |
   |                       |                                                                                                                                                                                                                            |    -----------        |
   |                       |                                                                                                                                                                                                                            |          28.5         |
   |                       |                                                                                                                                                                                                                            |    (1 row)            |
   +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | milliseconds          | Extract the millisecond from the specified timestamp. Full seconds are returned in milliseconds, that is, the second part (including fractional parts) multiplied by 1000.                                                 | Return result:        |
   |                       |                                                                                                                                                                                                                            |                       |
   |                       | ::                                                                                                                                                                                                                         | ::                    |
   |                       |                                                                                                                                                                                                                            |                       |
   |                       |    SELECT EXTRACT(MILLISECONDS FROM TIME '17:12:28.5');                                                                                                                                                                    |     date_part         |
   |                       |                                                                                                                                                                                                                            |    -----------        |
   |                       |                                                                                                                                                                                                                            |         28500         |
   |                       |                                                                                                                                                                                                                            |    (1 row)            |
   +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | microseconds          | Extract the microsecond from the specified timestamp. Full seconds are returned in microsecond, that is, the second part (including fractional parts) multiplied by 1,000,000.                                             | Return result:        |
   |                       |                                                                                                                                                                                                                            |                       |
   |                       | ::                                                                                                                                                                                                                         | ::                    |
   |                       |                                                                                                                                                                                                                            |                       |
   |                       |    SELECT EXTRACT(MICROSECONDS FROM TIME '17:12:28.5');                                                                                                                                                                    |     date_part         |
   |                       |                                                                                                                                                                                                                            |    -----------        |
   |                       |                                                                                                                                                                                                                            |      28500000         |
   |                       |                                                                                                                                                                                                                            |    (1 row)            |
   +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | epoch                 | -  If **source** is **timestamp with time zone**, it indicates the number of seconds since 1970-01-01 00:00:00-00 UTC (can be negative).                                                                                   | Return result:        |
   |                       |                                                                                                                                                                                                                            |                       |
   |                       |    ::                                                                                                                                                                                                                      | ::                    |
   |                       |                                                                                                                                                                                                                            |                       |
   |                       |       SELECT EXTRACT(EPOCH FROM TIMESTAMP WITH TIME ZONE '2001-02-16 20:38:40.12-08');                                                                                                                                     |      date_part        |
   |                       |                                                                                                                                                                                                                            |    --------------     |
   |                       | -  If **source** is **date** or **timestamp**, it indicates the number of seconds since 1970-01-01 00:00:00-00 local time.                                                                                                 |     982384720.12      |
   |                       |                                                                                                                                                                                                                            |    (1 row)            |
   |                       | -  If **source** is **interval**, it indicates the total number of seconds in the interval.                                                                                                                                |                       |
   |                       |                                                                                                                                                                                                                            | Return result:        |
   |                       |    ::                                                                                                                                                                                                                      |                       |
   |                       |                                                                                                                                                                                                                            | ::                    |
   |                       |       SELECT EXTRACT(EPOCH FROM INTERVAL '5 days 3 hours');                                                                                                                                                                |                       |
   |                       |                                                                                                                                                                                                                            |     date_part         |
   |                       | Convert an epoch value back to a timestamp:                                                                                                                                                                                |    -----------        |
   |                       |                                                                                                                                                                                                                            |        442800         |
   |                       | ::                                                                                                                                                                                                                         |    (1 row)            |
   |                       |                                                                                                                                                                                                                            |                       |
   |                       |    SELECT TIMESTAMP WITH TIME ZONE 'epoch' + 982384720.12 * INTERVAL '1 second' AS RESULT;                                                                                                                                 |                       |
   |                       |              result                                                                                                                                                                                                        |                       |
   |                       |    ---------------------------                                                                                                                                                                                             |                       |
   |                       |     2001-02-17 12:38:40.12+08                                                                                                                                                                                              |                       |
   |                       |    (1 row)                                                                                                                                                                                                                 |                       |
   +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | doy                   | Obtain the day of a year in a specified timestamp. The value ranges from 1 to 365/366.                                                                                                                                     | Return result:        |
   |                       |                                                                                                                                                                                                                            |                       |
   |                       | ::                                                                                                                                                                                                                         | ::                    |
   |                       |                                                                                                                                                                                                                            |                       |
   |                       |    SELECT EXTRACT(DOY FROM TIMESTAMP '2001-02-16 20:38:40');                                                                                                                                                               |     date_part         |
   |                       |                                                                                                                                                                                                                            |    -----------        |
   |                       |                                                                                                                                                                                                                            |            47         |
   |                       |                                                                                                                                                                                                                            |    (1 row)            |
   +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | isodow                | Obtain the day of a week in a specified timestamp. Monday is 1, and Sunday is 7. The value ranges from 1 to 7.                                                                                                             | Return result:        |
   |                       |                                                                                                                                                                                                                            |                       |
   |                       | ::                                                                                                                                                                                                                         | ::                    |
   |                       |                                                                                                                                                                                                                            |                       |
   |                       |    SELECT EXTRACT(ISODOW FROM TIMESTAMP '2001-02-18 20:38:40');                                                                                                                                                            |     date_part         |
   |                       |                                                                                                                                                                                                                            |    -----------        |
   |                       |                                                                                                                                                                                                                            |             7         |
   |                       |                                                                                                                                                                                                                            |    (1 row)            |
   +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | dow                   | Obtain the day of a week in a specified timestamp. Sunday is 0, and Saturday is 6. The value ranges from 0 to 6.                                                                                                           | Return result:        |
   |                       |                                                                                                                                                                                                                            |                       |
   |                       | ::                                                                                                                                                                                                                         | ::                    |
   |                       |                                                                                                                                                                                                                            |                       |
   |                       |    SELECT EXTRACT(DOW FROM TIMESTAMP '2001-02-16 20:38:40');                                                                                                                                                               |     date_part         |
   |                       |                                                                                                                                                                                                                            |    -----------        |
   |                       |                                                                                                                                                                                                                            |             5         |
   |                       |                                                                                                                                                                                                                            |    (1 row)            |
   +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | isoyear               | Obtain the ISO 8601 week-numbering year of a specified date (not applicable to interval).                                                                                                                                  | Return result:        |
   |                       |                                                                                                                                                                                                                            |                       |
   |                       | Each ISO year begins with the Monday of the week containing January 4, so in early January or late December the ISO year may be different from the Gregorian year. For details, see the **week** field.                    | ::                    |
   |                       |                                                                                                                                                                                                                            |                       |
   |                       | ::                                                                                                                                                                                                                         |     date_part         |
   |                       |                                                                                                                                                                                                                            |    -----------        |
   |                       |    SELECT EXTRACT(ISOYEAR FROM DATE '2006-01-01');                                                                                                                                                                         |          2005         |
   |                       |                                                                                                                                                                                                                            |    (1 row)            |
   |                       | ::                                                                                                                                                                                                                         |                       |
   |                       |                                                                                                                                                                                                                            | Return result:        |
   |                       |    SELECT EXTRACT(ISOYEAR FROM DATE '2006-01-02');                                                                                                                                                                         |                       |
   |                       |                                                                                                                                                                                                                            | ::                    |
   |                       |                                                                                                                                                                                                                            |                       |
   |                       |                                                                                                                                                                                                                            |     date_part         |
   |                       |                                                                                                                                                                                                                            |    -----------        |
   |                       |                                                                                                                                                                                                                            |          2006         |
   |                       |                                                                                                                                                                                                                            |    (1 row)            |
   +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | timezone              | The time zone offset from UTC, measured in seconds. A positive value indicates the time zone east of UTC, and a negative value indicates the time zone west of UTC.                                                        | Return result:        |
   |                       |                                                                                                                                                                                                                            |                       |
   |                       | ::                                                                                                                                                                                                                         | ::                    |
   |                       |                                                                                                                                                                                                                            |                       |
   |                       |    SELECT EXTRACT(timezone FROM TIMETZ '17:12:28');                                                                                                                                                                        |     date_part         |
   |                       |                                                                                                                                                                                                                            |    -----------        |
   |                       |                                                                                                                                                                                                                            |       28800           |
   |                       |                                                                                                                                                                                                                            |    (1 row)            |
   +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | timezone_hour         | Extract the hour component of the time zone offset from a time value with a time zone.                                                                                                                                     | Return result:        |
   |                       |                                                                                                                                                                                                                            |                       |
   |                       | ::                                                                                                                                                                                                                         | ::                    |
   |                       |                                                                                                                                                                                                                            |                       |
   |                       |    SELECT EXTRACT(timezone_hour FROM TIMETZ '17:12:28');                                                                                                                                                                   |     date_part         |
   |                       |                                                                                                                                                                                                                            |    -----------        |
   |                       |                                                                                                                                                                                                                            |           8           |
   |                       |                                                                                                                                                                                                                            |    (1 row)            |
   +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | timezone_minute       | Extract the minute component of the time zone offset from a time value with a time zone.                                                                                                                                   | Return result:        |
   |                       |                                                                                                                                                                                                                            |                       |
   |                       | ::                                                                                                                                                                                                                         | ::                    |
   |                       |                                                                                                                                                                                                                            |                       |
   |                       |    SELECT EXTRACT(timezone_minute FROM TIMETZ '17:12:28');                                                                                                                                                                 |     date_part         |
   |                       |                                                                                                                                                                                                                            |    -----------        |
   |                       |                                                                                                                                                                                                                            |             0         |
   |                       |                                                                                                                                                                                                                            |    (1 row)            |
   +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
