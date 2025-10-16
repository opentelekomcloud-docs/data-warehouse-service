:original_name: dws_06_0313.html

.. _dws_06_0313:

time_format
===========

time_format(time, fmt)
----------------------

Description: The **time_format** function converts the **date** parameter into a string in the format specified by **fmt**. It is similar to the **date_format** function, but the format string can contain only hour, minute, second, and microsecond format specifiers. If other specifiers are contained, **NULL** or **0** is returned.

Return type: text

Example:

::

   SELECT time_format('2009-10-04 22:23:00', '%M %D %W');
       time_format
   --------------------

   (1 row)
   SELECT time_format('2021-02-20 08:30:45', '%Y-%m-%d %H:%i:%S');
        time_format
   ---------------------
    0000-00-00 08:30:45
   (1 row)
   SELECT time_format('2021-02-20 18:10:15', '%r-%T');
        time_format
   ----------------------
    06:10:15 PM-18:10:15
   (1 row)

.. important::

   **time_format** supports only time-related formats (%f, %H, %h, %I, %i, %k, %l, %p, %r, %S, %s, and %T) and does not support date-related formats. In other cases, **time_format** is output as common characters.

str_to_date(str, format)
------------------------

Description: Converts a string of the date/time type to a value of the date type according to the provided display format.

Return type: timestamp

Example:

::

   SELECT str_to_date('01,5,2021','%d,%m,%Y');
        str_to_date
   ---------------------
    2021-05-01 00:00:00
   (1 row)
   SELECT str_to_date('01,5,2021,09,30,17','%d,%m,%Y,%h,%i,%s');
        str_to_date
   ---------------------
    2021-05-01 09:30:17
   (1 row)

For details about the input format types applicable to **str_to_date**, see :ref:`Table 1 <en-us_topic_0000001764675318__table17541102011>`. Only input values of the date or date/time type can be converted. Use **str_to_time** when only values of the time type are input.

str_to_time(str, format)
------------------------

Description: Converts a string of the time type to a value of the time type according to the provided display format.

Return type: time

Example:

::

   SELECT str_to_time('09:30:17','%h:%i:%s');
    str_to_time
   -------------
    09:30:17
   (1 row)

For details about the input format types applicable to **str_to_time**, see :ref:`Table 1 <en-us_topic_0000001764675318__table17541102011>`. Only input values of the time type can be converted. Use **str_to_date** when values of the date or date/time type are input.

week(date[, mode])
------------------

Description: Returns the number of weeks in the year of the specified datetime. The default value is **0**.

Return type: integer

.. table:: **Table 1** Working principle of the mode in the week function

   +--------+---------------------+------------+--------------------------------------------------+
   | Schema | First Day of a Week | Week Range | Rule for Determining the First Week              |
   +========+=====================+============+==================================================+
   | 0      | Sun                 | 0-53       | Week of the first Sunday after New Year's Day    |
   +--------+---------------------+------------+--------------------------------------------------+
   | 1      | Mon                 | 0-53       | Week with four or more days after New Year's Day |
   +--------+---------------------+------------+--------------------------------------------------+
   | 2      | Sun                 | 1-53       | Week of the first Sunday after New Year's Day    |
   +--------+---------------------+------------+--------------------------------------------------+
   | 3      | Mon                 | 1-53       | Week with four or more days after New Year's Day |
   +--------+---------------------+------------+--------------------------------------------------+
   | 4      | Sun                 | 0-53       | Week with four or more days after New Year's Day |
   +--------+---------------------+------------+--------------------------------------------------+
   | 5      | Mon                 | 0-53       | Week of the first Monday after New Year's Day    |
   +--------+---------------------+------------+--------------------------------------------------+
   | 6      | Sun                 | 1-53       | Week with four or more days after New Year's Day |
   +--------+---------------------+------------+--------------------------------------------------+
   | 7      | Mon                 | 1-53       | Week of the first Monday after New Year's Day    |
   +--------+---------------------+------------+--------------------------------------------------+

Example:

::

   select week('2018-01-01');
    week
   ------
       0
   (1 row)

   select week('2018-01-01', 0);
    week
   ------
       0
   (1 row)

   select week('2020-12-31', 1);
    week
   ------
      53
   (1 row)

   select week('2020-12-31', 5);
    week
   ------
      52
   (1 row)

weekday(date)
-------------

Description: Returns the week index corresponding to the given date, with Monday as the start day of the week.

Value range: 0 to 6

Return type: integer

Example:

::

   select weekday('2020-11-06');
    weekday
   ---------
          4
   (1 row)

weekofyear(date)
----------------

Description: Returns the number of weeks in the current year for the week of the given date. The value ranges from 1 to 53, which is equivalent to week(date, 3).

Return type: integer

Example:

::

   select weekofyear('2020-12-30');
    weekofyear
   ------------
            53
   (1 row)

year(date)
----------

Description: Obtains the year of the **date**.

Return type: integer

Example:

::

   select year('2020-11-13');
    year
   ------
    2020
   (1 row)

yearweek(date[, mode])
----------------------

Description: Returns the year and the number of weeks in the current year for the given date. The number of weeks ranges from 1 to 53.

Return type: integer

Example:

::

   select yearweek('2019-12-31');
    yearweek
   ----------
      201952
   (1 row)

   select yearweek('2019-1-1');
    yearweek
   ----------
      201852
   (1 row)
