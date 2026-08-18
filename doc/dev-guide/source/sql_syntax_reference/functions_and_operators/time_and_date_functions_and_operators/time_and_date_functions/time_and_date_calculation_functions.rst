:original_name: dws_06_0312.html

.. _dws_06_0312:

Time and Date Calculation Functions
===================================

.. _en-us_topic_0000002510221158__section5554191145815:

age(timestamp, timestamp)
-------------------------

Description: Subtracts arguments and returns the difference. If the result is negative, the returned result is also negative.

Return type: interval

Example:

::

   SELECT age(timestamp '2001-04-10', timestamp '1957-06-13');
              age
   -------------------------
    43 years 9 mons 27 days
   (1 row)

.. _en-us_topic_0000002510221158__section1646778185817:

age(timestamp)
--------------

Description: Subtracts the argument from the current date.

Return type: interval

Example:

::

   SELECT age(timestamp '1957-06-13');
              age
   -------------------------
    68 years 7 mons 7 days
   (1 row)

.. _en-us_topic_0000002510221158__section5485357588:

adddate(date, interval \| int)
------------------------------

Description: Returns the result of a given datetime plus the time interval of a specified unit. The default unit is day (when the second parameter is an integer).

Return type: timestamp

Examples:

Set the input parameter to the text type.

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

Set the input parameter to the date type.

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

.. _en-us_topic_0000002510221158__section2345185695713:

subdate(date, interval \| int)
------------------------------

Description: Returns the result of a given datetime minus the time interval of a specified unit. The default unit is day (when the second parameter is an integer).

Return type: timestamp

Examples:

Set the input parameter to the text type.

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

Set the input parameter to the date type.

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

.. _en-us_topic_0000002510221158__section1599418589575:

addtime(timestamp \| time \| text, interval \| text)
----------------------------------------------------

Description: Returns the result of a given date or time plus the time interval of a specified unit. This parameter is supported only by clusters of version 8.2.0 or later.

Return type: same as the type of the first input parameter.

Example:

::

   SELECT ADDTIME('2020-11-13 01:01:01', '23:59:59');
          addtime
   ---------------------
    2020-11-14 01:01:00
   (1 row)

addtime(time, interval)

::

   SELECT ADDTIME('20:1:1'::TIME, '20:59:59+8');
     addtime
   -----------
    233:01:00
   (1 row)

   SELECT ADDTIME('20:1:1'::TIME, '8 20:59:59');
     addtime
   -----------
    233:01:00
   (1 row)

addtime(text, interval)

::

   SELECT ADDTIME('20:1:1', '20:59:59+8');
     addtime
   -----------
    233:01:00
   (1 row)
   SELECT ADDTIME('20:1:1', '8 20:59:59');
     addtime
   -----------
    233:01:00
   (1 row)

.. _en-us_topic_0000002510221158__section6187184914578:

subtime(timestamp \| time \| text, interval \| text)
----------------------------------------------------

Description: Returns the result of a given date or time minus the time interval of a specified unit. This parameter is supported only by clusters of version 8.2.0 or later.

Return type: same as the type of the first input parameter.

Example:

::

   SELECT subtime('2020-11-13 01:01:01', '23:59:59');
          subtime
   ---------------------
    2020-11-12 01:01:02
   (1 row)

.. _en-us_topic_0000002510221158__section1556074615572:

date_add(date, interval)
------------------------

Description: Returns the result of a given datetime plus the time interval of a specified unit. It is equivalent to :ref:`adddate(date, interval | int) <en-us_topic_0000002510221158__section5485357588>`.

Return type: timestamp

.. _en-us_topic_0000002510221158__section11590154335710:

date_sub(date, interval)
------------------------

Description: Returns the result of a given datetime minus the time interval of a specified unit. It is equivalent to :ref:`subdate(date, interval | int) <en-us_topic_0000002510221158__section2345185695713>`.

Return type: timestamp

.. _en-us_topic_0000002510221158__section124754095716:

timestampadd(field, numeric, timestamp)
---------------------------------------

Description: Adds an integer interval in the unit of **field** (the number of seconds can be a decimal) to a datetime expression. If the value is negative, the corresponding time interval is subtracted from the given datetime expression. The **field** can be **year**, **month**, **quarter**, **day**, **week**, **hour**, **minute**, **second**, or **microsecond**.

.. note::

   When you pass a time expression in the **yyyy-mon-day+{value}** format to the function, the **{value}** part represents the time zone for the given date. For example, **2023-01-31+1** means January 31, 2023, in the UTC+1 time zone. In DWS, this format is handled differently in MySQL. MySQL processes this format only if the loose parsing rule is enabled, interpreting it as 01:00 on January 31, 2023, in the system's default time zone.

Return type: timestamp

Example:

::

   SELECT timestampadd(year, 1, timestamp '2020-2-29');
       timestampadd
   ---------------------
    2021-02-28 00:00:00
   (1 row)

   SELECT timestampadd(second, 2.354156, timestamp '2020-11-13');
           timestampadd
   ----------------------------
    2020-11-13 00:00:02.354156
   (1 row)

.. _en-us_topic_0000002510221158__section180353655719:

timestampdiff(field, timestamp1, timestamp2)
--------------------------------------------

Description: Subtracts **timestamp1** from **timestamp2** and returns the difference in the unit of **field**. If the difference is negative, this function returns it normally. The **field** can be **year**, **month**, **quarter**, **day**, **week**, **hour**, **minute**, **second**, or **microsecond**.

Return type: bigint

Example:

::

   SELECT timestampdiff(day, timestamp '2001-02-01', timestamp '2003-05-01 12:05:55');
    timestampdiff
   ---------------
         819
   (1 row)

.. _en-us_topic_0000002510221158__section169068337572:

timediff(timestamp \| time \| text, timestamp \| time \| text)
--------------------------------------------------------------

Description: Subtracts a date from another date. If the difference is negative, this function returns it normally. The types of the two input parameters must be the same. This parameter is supported only by clusters of version 8.2.0 or later.

Return type: time

Example:

::

   SELECT timediff('2022-7-5 1:1:1', '2021-7-5 1:1:1');
      timediff
   ---------------
     8760:00:00
   (1 row)

.. _en-us_topic_0000002510221158__section1424572145716:

datediff(date1, date2)
----------------------

Description: Returns the number of days between two given dates.

Return type: bigint

Example:

::

   SELECT datediff(date '2020-11-13', date '2012-10-16');
    datediff
   ----------
        2950
   (1 row)

.. _en-us_topic_0000002510221158__section14401912125515:

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

.. _en-us_topic_0000002510221158__section9345199557:

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

.. _en-us_topic_0000002510221158__section10654241125415:

add_months(d,n)
---------------

Description: Calculates the date value after date is added by *N* months.

Return type: timestamp

Example:

::

   SELECT add_months(to_date('2017-5-29', 'yyyy-mm-dd'), 11) FROM dual;
        add_months
   ---------------------
    2018-04-29 00:00:00
   (1 row)
