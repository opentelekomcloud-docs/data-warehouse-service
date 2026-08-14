:original_name: dws_06_0421.html

.. _dws_06_0421:

Functions for Obtaining the Current Time and Date
=================================================

.. _en-us_topic_0000002510381166__section16306173145714:

clock_timestamp()
-----------------

Description: Returns the current timestamp of the real-time clock.

Return type: timestamp with time zone

Example:

::

   SELECT clock_timestamp();
           clock_timestamp
   -------------------------------
    2026-01-14 15:18:30.766999+08
   (1 row)

.. _en-us_topic_0000002510381166__section346892875718:

current_date
------------

Description: Returns the current date.

Return type: date

Example:

::

   SELECT current_date;
       date
   ------------
    2026-01-14
   (1 row)

.. _en-us_topic_0000002510381166__section7383172465718:

curdate()
---------

Description: Returns the current date. This function is compatible with MySQL. This parameter is supported only by clusters of version 8.2.0 or later.

Return type: date

Example:

::

   SELECT curdate();
     curdate
   ------------
    2026-01-14
   (1 row)

.. _en-us_topic_0000002510381166__section83901821145715:

current_time
------------

Description: Returns the current time.

Return type: time with time zone

Example:

::

   SELECT current_time;
          timetz
   --------------------
    16:58:07.086215+08
   (1 row)

.. _en-us_topic_0000002510381166__section25941215155714:

curtime([fsp])
--------------

Description: Returns the current time.

**fsp** is an optional parameter. It specifies a fractional seconds precision and its value is an integer. This parameter is supported only by clusters of version 8.2.0 or later.

Return type: time with time zone

Example:

::

   SELECT curtime();
          timetz
   --------------------
    16:58:07.086215+08
   (1 row)
   SELECT curtime(2);
          timetz
   --------------------
    16:58:07.08+08
   (1 row)

.. _en-us_topic_0000002510381166__section1595531112571:

current_timestamp
-----------------

Description: Returns the current date and time (start time of the current transaction).

Return type: timestamp with time zone

Example:

::

   SELECT current_timestamp;
           pg_systimestamp
   -------------------------------
    2026-01-14 15:30:51.717927+08
   (1 row)

.. _en-us_topic_0000002510381166__section19891164265518:

localtime
---------

Description: Returns the current time.

Return type: time

Example:

::

   SELECT localtime;
         time
   -----------------
    15:40:54.885857
   (1 row)

.. _en-us_topic_0000002510381166__section32161140195516:

localtimestamp
--------------

Description: Returns the current date and time.

Return type: timestamp

Example:

::

   SELECT localtimestamp;
           timestamp
   ----------------------------
    2026-01-14 15:41:49.111089
   (1 row)

.. _en-us_topic_0000002510381166__section9466504557:

statement_timestamp()
---------------------

Description: Returns the current date and time (start time of the current transaction).

Return type: timestamp with time zone

Example:

::

   SELECT statement_timestamp();
         statement_timestamp
   -------------------------------
    2026-01-14 15:43:14.907913+08
   (1 row)

.. _en-us_topic_0000002510381166__section5689157185410:

sysdate
-------

Description: Returns the current date and time of the system.

Return type: timestamp

Example:

::

   SELECT sysdate;
          sysdate
   ---------------------
    2026-01-14 15:47:58
   (1 row)

.. _en-us_topic_0000002510381166__section137791454205413:

timeofday()
-----------

Description: Returns the current date and time (similar to :ref:`clock_timestamp() <en-us_topic_0000002510381166__section16306173145714>`, but the return value is of the text type).

Return type: text

Example:

::

   SELECT timeofday();
                 timeofday
   -------------------------------------
    Wed Jan 14 15:51:04.053480 2026 CST
   (1 row)

.. _en-us_topic_0000002510381166__section541355185411:

transaction_timestamp()
-----------------------

Description: Returns the system date and time when the current transaction starts. It is equivalent to **current_timestamp**.

Return type: timestamp with time zone

Example:

::

   SELECT transaction_timestamp();

        transaction_timestamp
   -------------------------------
    2026-01-14 16:59:52.045873+08
   (1 row)
