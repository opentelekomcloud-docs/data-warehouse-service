:original_name: dws_06_0014.html

.. _dws_06_0014:

Date/Time Types
===============

:ref:`Table 1 <en-us_topic_0000001233510097__te36aedd5ecb747abb055a0d329a83c75>` lists date and time types supported by GaussDB(DWS). For the operators and built-in functions of the types, see :ref:`Date and Time Processing Functions and Operators <dws_06_0035>`.

.. note::

   If the time format of another database is different from that of GaussDB(DWS), modify the value of the **DateStyle** parameter to keep them consistent.

.. _en-us_topic_0000001233510097__te36aedd5ecb747abb055a0d329a83c75:

.. table:: **Table 1** Date/Time Types

   +------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------+
   | Name                               | Description                                                                                                                                                                                                                                                  | Storage Space                                      |
   +====================================+==============================================================================================================================================================================================================================================================+====================================================+
   | DATE                               | In Oracle compatibility mode, it is equivalent to timestamp(0) and records the date and time.                                                                                                                                                                | In Oracle compatibility mode, it occupies 8 bytes. |
   |                                    |                                                                                                                                                                                                                                                              |                                                    |
   |                                    | In other modes, it records the date.                                                                                                                                                                                                                         | In Oracle compatibility mode, it occupies 4 bytes. |
   +------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------+
   | TIME [(p)] [WITHOUT TIME ZONE]     | Specifies time within one day.                                                                                                                                                                                                                               | 8 bytes                                            |
   |                                    |                                                                                                                                                                                                                                                              |                                                    |
   |                                    | **p** indicates the precision after the decimal point. The value ranges from 0 to 6.                                                                                                                                                                         |                                                    |
   +------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------+
   | TIME [(p)] [WITH TIME ZONE]        | Specifies time within one day (with time zone).                                                                                                                                                                                                              | 12 bytes                                           |
   |                                    |                                                                                                                                                                                                                                                              |                                                    |
   |                                    | **p** indicates the precision after the decimal point. The value ranges from 0 to 6.                                                                                                                                                                         |                                                    |
   +------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------+
   | TIMESTAMP[(p)] [WITHOUT TIME ZONE] | Specifies the date and time.                                                                                                                                                                                                                                 | 8 bytes                                            |
   |                                    |                                                                                                                                                                                                                                                              |                                                    |
   |                                    | **p** indicates the precision after the decimal point. The value ranges from 0 to 6.                                                                                                                                                                         |                                                    |
   +------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------+
   | TIMESTAMP[(p)][WITH TIME ZONE]     | Specifies the date and time (with time zone). TIMESTAMP is also called TIMESTAMPTZ.                                                                                                                                                                          | 8 bytes                                            |
   |                                    |                                                                                                                                                                                                                                                              |                                                    |
   |                                    | **p** indicates the precision after the decimal point. The value ranges from 0 to 6.                                                                                                                                                                         |                                                    |
   +------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------+
   | SMALLDATETIME                      | Specifies the date and time (without time zone).                                                                                                                                                                                                             | 8 bytes                                            |
   |                                    |                                                                                                                                                                                                                                                              |                                                    |
   |                                    | The precision level is minute. 31s to 59s are rounded into 1 minute.                                                                                                                                                                                         |                                                    |
   +------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------+
   | INTERVAL DAY (l) TO SECOND (p)     | Specifies the time interval (X days X hours X minutes X seconds).                                                                                                                                                                                            | 16 bytes                                           |
   |                                    |                                                                                                                                                                                                                                                              |                                                    |
   |                                    | -  **l**: indicates the precision of days. The value ranges from 0 to 6. To adapt to Oracle syntax, the precision functions are not supported.                                                                                                               |                                                    |
   |                                    | -  **p**: indicates the precision of seconds. The value ranges from 0 to 6. The digit 0 at the end of a decimal number is not displayed.                                                                                                                     |                                                    |
   +------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------+
   | INTERVAL [FIELDS] [ (p) ]          | Specifies the time interval.                                                                                                                                                                                                                                 | 12 bytes                                           |
   |                                    |                                                                                                                                                                                                                                                              |                                                    |
   |                                    | -  fields: **YEAR**, **MONTH**, **DAY**, **HOUR**, **MINUTE**, **SECOND**, **DAY TO HOUR**, **DAY TO MINUTE**, **DAY TO SECOND**, **HOUR TO MINUTE**, **HOUR TO SECOND**, and **MINUTE TO SECOND**.                                                          |                                                    |
   |                                    |                                                                                                                                                                                                                                                              |                                                    |
   |                                    | -  **p**: indicates the precision of seconds. The value ranges from 0 to 6. **p** takes effect only when fields are **SECOND**, **DAY TO SECOND**, **HOUR TO SECOND**, or **MINUTE TO SECOND**. The digit 0 at the end of a decimal number is not displayed. |                                                    |
   +------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------+
   | reltime                            | Relative time interval. The format is:                                                                                                                                                                                                                       | 4 bytes                                            |
   |                                    |                                                                                                                                                                                                                                                              |                                                    |
   |                                    | *X* years *X* months *X* days *XX:XX:XX*                                                                                                                                                                                                                     |                                                    |
   |                                    |                                                                                                                                                                                                                                                              |                                                    |
   |                                    | -  The Julian calendar is used. It specifies that a year has 365.25 days and a month has 30 days. The relative time interval needs to be calculated based on the input value. The output format is POSTGRES.                                                 |                                                    |
   +------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------+

For example:

::

   --Create a table:
   CREATE TABLE date_type_tab(coll date);

   --Insert data:
   INSERT INTO date_type_tab VALUES (date '12-10-2010');

   -- View data:
   SELECT * FROM date_type_tab;
           coll
   ---------------------
    2010-12-10 00:00:00
   (1 row)

   -- Delete the tables:
   DROP TABLE date_type_tab;

   --Create a table:
   CREATE TABLE time_type_tab (da time without time zone ,dai time with time zone,dfgh timestamp without time zone,dfga timestamp with time zone, vbg smalldatetime);

   --Insert data:
   INSERT INTO time_type_tab VALUES ('21:21:21','21:21:21 pst','2010-12-12','2013-12-11 pst','2003-04-12 04:05:06');

   -- View data:
   SELECT * FROM time_type_tab;
       da    |     dai     |        dfgh         |          dfga          |         vbg
   ----------+-------------+---------------------+------------------------+---------------------
    21:21:21 | 21:21:21-08 | 2010-12-12 00:00:00 | 2013-12-11 16:00:00+08 | 2003-04-12 04:05:00
   (1 row)

   -- Delete the tables:
   DROP TABLE time_type_tab;

   --Create a table:
   CREATE TABLE day_type_tab (a int,b INTERVAL DAY(3) TO SECOND (4));

   --Insert data:
   INSERT INTO day_type_tab VALUES (1, INTERVAL '3' DAY);

   -- View data:
   SELECT * FROM day_type_tab;
    a |   b
   ---+--------
    1 | 3 days
   (1 row)

   -- Delete the tables:
   DROP TABLE day_type_tab;

   --Create a table:
   CREATE TABLE year_type_tab(a int, b interval year (6));

   --Insert data:
   INSERT INTO year_type_tab VALUES(1,interval '2' year);

   -- View data:
   SELECT * FROM year_type_tab;
    a |    b
   ---+---------
    1 | 2 years
   (1 row)

   -- Delete the tables:
   DROP TABLE year_type_tab;

Date Input
----------

Date and time input is accepted in almost any reasonable formats, including ISO 8601, SQL-compatible, and traditional POSTGRES. The system allows you to customize the sequence of day, month, and year in the date input. Set the **DateStyle** parameter to **MDY** to select month-day-year interpretation, **DMY** to select day-month-year interpretation, or **YMD** to select year-month-day interpretation.

Remember that any date or time literal input needs to be enclosed with single quotes, and the syntax is as follows:

type [ ( p ) ] 'value'

The **p** that can be selected in the precision statement is an integer, indicating the number of fractional digits in the **seconds** column. :ref:`Table 2 <en-us_topic_0000001233510097__tc495b297873743f4b54c2a2dc171b42a>` shows some possible inputs for the **date** type.

.. _en-us_topic_0000001233510097__tc495b297873743f4b54c2a2dc171b42a:

.. table:: **Table 2** Date input

   +-----------------------------------+------------------------------------------------------------+
   | Example                           | Description                                                |
   +===================================+============================================================+
   | 1999-01-08                        | ISO 8601 (recommended format). January 8, 1999 in any mode |
   +-----------------------------------+------------------------------------------------------------+
   | January 8, 1999                   | Unambiguous in any date input mode                         |
   +-----------------------------------+------------------------------------------------------------+
   | 1/8/1999                          | January 8 in **MDY** mode. August 1 in **DMY** mode        |
   +-----------------------------------+------------------------------------------------------------+
   | 1/18/1999                         | January 18 in **MDY** mode, rejected in other modes        |
   +-----------------------------------+------------------------------------------------------------+
   | 01/02/03                          | -  January 2, 2003 in **MDY** mode                         |
   |                                   | -  February 1, 2003 in **DMY** mode                        |
   |                                   | -  February 3, 2001 in **YMD** mode                        |
   +-----------------------------------+------------------------------------------------------------+
   | 1999-Jan-08                       | January 8 in any mode                                      |
   +-----------------------------------+------------------------------------------------------------+
   | Jan-08-1999                       | January 8 in any mode                                      |
   +-----------------------------------+------------------------------------------------------------+
   | 08-Jan-1999                       | January 8 in any mode                                      |
   +-----------------------------------+------------------------------------------------------------+
   | 99-Jan-08                         | January 8 in **YMD** mode, else error                      |
   +-----------------------------------+------------------------------------------------------------+
   | 08-Jan-99                         | January 8, except error in **YMD** mode                    |
   +-----------------------------------+------------------------------------------------------------+
   | Jan-08-99                         | January 8, except error in **YMD** mode                    |
   +-----------------------------------+------------------------------------------------------------+
   | 19990108                          | ISO 8601. January 8, 1999 in any mode                      |
   +-----------------------------------+------------------------------------------------------------+
   | 990108                            | ISO 8601. January 8, 1999 in any mode                      |
   +-----------------------------------+------------------------------------------------------------+
   | 1999.008                          | Year and day of year                                       |
   +-----------------------------------+------------------------------------------------------------+
   | J2451187                          | Julian date                                                |
   +-----------------------------------+------------------------------------------------------------+
   | January 8, 99 BC                  | Year 99 BC                                                 |
   +-----------------------------------+------------------------------------------------------------+

For example:

::

   --Create a table:
   CREATE TABLE date_type_tab(coll date);

   --Insert data:
   INSERT INTO date_type_tab VALUES (date '12-10-2010');

   -- View data:
   SELECT * FROM date_type_tab;
           coll
   ---------------------
    2010-12-10 00:00:00
   (1 row)

   -- View the date format:
   SHOW datestyle;
    DateStyle
   -----------
    ISO, MDY
   (1 row)

   -- Configure the date format:
   SET datestyle='YMD';
   SET

   -- Insert data:
   INSERT INTO date_type_tab VALUES(date '2010-12-11');

   -- View data:
   SELECT * FROM date_type_tab;
           coll
   ---------------------
    2010-12-10 00:00:00
    2010-12-11 00:00:00
   (2 rows)

   -- Delete the tables:
   DROP TABLE date_type_tab;

Times
-----

The time-of-day types are **TIME [(p)] [WITHOUT TIME ZONE]** and **TIME [(p)] [WITH TIME ZONE]**. **TIME** alone is equivalent to **TIME WITHOUT TIME ZONE**.

If a time zone is specified in the input for **TIME WITHOUT TIME ZONE**, it is silently ignored.

For details about the time input types, see :ref:`Table 3 <en-us_topic_0000001233510097__t24429c065d474feba61c1b0e490f9dac>`. For details about time zone input types, see :ref:`Table 4 <en-us_topic_0000001233510097__t63d0318275dc486081a76f7677ab0a5f>`.

.. _en-us_topic_0000001233510097__t24429c065d474feba61c1b0e490f9dac:

.. table:: **Table 3** Time input

   ============== =======================================
   Example        Description
   ============== =======================================
   05:06.8        ISO 8601
   4:05:06        ISO 8601
   4:05           ISO 8601
   40506          ISO 8601
   4:05 AM        Same as 04:05. AM does not affect value
   4:05 PM        Same as 16:05. Input hour must be <= 12
   04:05:06.789-8 ISO 8601
   04:05:06-08:00 ISO 8601
   04:05-08:00    ISO 8601
   040506-08      ISO 8601
   04:05:06 PST   Time zone specified by abbreviation
   ============== =======================================

.. _en-us_topic_0000001233510097__t63d0318275dc486081a76f7677ab0a5f:

.. table:: **Table 4** Time zone input

   ======= ========================================
   Example Description
   ======= ========================================
   PST     Abbreviation (for Pacific Standard Time)
   -8:00   ISO-8601 offset for PST
   -800    ISO-8601 offset for PST
   -8      ISO-8601 offset for PST
   ======= ========================================

For example:

::

   SELECT time '04:05:06';
      time
   ----------
    04:05:06
   (1 row)

   SELECT time '04:05:06 PST';
      time
   ----------
    04:05:06
   (1 row)

   SELECT time with time zone '04:05:06 PST';
      timetz
   -------------
    04:05:06-08
   (1 row)

Special Values
--------------

The special values supported by GaussDB(DWS) are converted to common date/time values when being read. For details, see :ref:`Table 5 <en-us_topic_0000001233510097__t5e86ad23ea5649969935ea26bf746e0f>`.

.. _en-us_topic_0000001233510097__t5e86ad23ea5649969935ea26bf746e0f:

.. table:: **Table 5** Special Values

   +--------------+-----------------------+------------------------------------------------+
   | Input String | Applicable Type       | Description                                    |
   +==============+=======================+================================================+
   | epoch        | date, timestamp       | 1970-01-01 00:00:00+00 (Unix system time zero) |
   +--------------+-----------------------+------------------------------------------------+
   | infinity     | timestamp             | Later than any other timestamps                |
   +--------------+-----------------------+------------------------------------------------+
   | -infinity    | timestamp             | Earlier than any other timestamps              |
   +--------------+-----------------------+------------------------------------------------+
   | now          | date, time, timestamp | Start time of the current transaction          |
   +--------------+-----------------------+------------------------------------------------+
   | today        | date, timestamp       | Today midnight                                 |
   +--------------+-----------------------+------------------------------------------------+
   | tomorrow     | date, timestamp       | Tomorrow midnight                              |
   +--------------+-----------------------+------------------------------------------------+
   | yesterday    | date, timestamp       | Yesterday midnight                             |
   +--------------+-----------------------+------------------------------------------------+
   | allballs     | time                  | 00:00:00.00 UTC                                |
   +--------------+-----------------------+------------------------------------------------+

Interval Input
--------------

The input of **reltime** can be any valid interval in TEXT format. It can be a number (negative numbers and decimals are also allowed) or a specific time, which must be in SQL standard format, ISO-8601 format, or POSTGRES format. In addition, the text input needs to be enclosed with single quotation marks ('').

For details, see :ref:`Table 6 <en-us_topic_0000001233510097__table1747116463276>`.

.. _en-us_topic_0000001233510097__table1747116463276:

.. table:: **Table 6** Interval input

   +--------------------------------+-------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Input                          | Output                              | Description                                                                                                                                                                             |
   +================================+=====================================+=========================================================================================================================================================================================+
   | 60                             | 2 mons                              | Numbers are used to indicate intervals. The default unit is day. Decimals and negative numbers are also allowed. Particularly, a negative interval syntactically means how long before. |
   +--------------------------------+-------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | 31.25                          | 1 mons 1 days 06:00:00              |                                                                                                                                                                                         |
   +--------------------------------+-------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | -365                           | -12 mons -5 days                    |                                                                                                                                                                                         |
   +--------------------------------+-------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | 1 years 1 mons 8 days 12:00:00 | 1 years 1 mons 8 days 12:00:00      | Intervals are in POSTGRES format. They can contain both positive and negative numbers and are case-insensitive. Output is a simplified POSTGRES interval converted from the input.      |
   +--------------------------------+-------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | -13 months -10 hours           | -1 years -25 days -04:00:00         |                                                                                                                                                                                         |
   +--------------------------------+-------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | -2 YEARS +5 MONTHS 10 DAYS     | -1 years -6 mons -25 days -06:00:00 |                                                                                                                                                                                         |
   +--------------------------------+-------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | P-1.1Y10M                      | -3 mons -5 days -06:00:00           | Intervals are in ISO-8601 format. They can contain both positive and negative numbers and are case-insensitive. Output is a simplified POSTGRES interval converted from the input.      |
   +--------------------------------+-------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | -12H                           | -12:00:00                           |                                                                                                                                                                                         |
   +--------------------------------+-------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

For example:

::

   -- Create a table.
   CREATE TABLE reltime_type_tab(col1 character(30), col2 reltime);

   -- Insert data.
   INSERT INTO reltime_type_tab VALUES ('90', '90');
   INSERT INTO reltime_type_tab VALUES ('-366', '-366');
   INSERT INTO reltime_type_tab VALUES ('1975.25', '1975.25');
   INSERT INTO reltime_type_tab VALUES ('-2 YEARS +5 MONTHS 10 DAYS', '-2 YEARS +5 MONTHS 10 DAYS');
   INSERT INTO reltime_type_tab VALUES ('30 DAYS 12:00:00', '30 DAYS 12:00:00');
   INSERT INTO reltime_type_tab VALUES ('P-1.1Y10M', 'P-1.1Y10M');

   -- View data.
   SELECT * FROM reltime_type_tab;
                 col1              |                col2
   --------------------------------+-------------------------------------
    1975.25                        | 5 years 4 mons 29 days
    -2 YEARS +5 MONTHS 10 DAYS     | -1 years -6 mons -25 days -06:00:00
    P-1.1Y10M                      | -3 mons -5 days -06:00:00
    -366                           | -1 years -18:00:00
    90                             | 3 mons
    30 DAYS 12:00:00               | 1 mon 12:00:00
   (6 rows)

   -- Delete tables.
   DROP TABLE reltime_type_tab;
