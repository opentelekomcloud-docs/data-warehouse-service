:original_name: dws_06_0336.html

.. _dws_06_0336:

Series Generating Functions
===========================

**generate_series()** returns a series-based set based on the specified start value (**start**), end value (**stop**), and step (**step**).

If **step** is a positive number and **start** is greater than **stop**, zero row is returned. If **step** is a negative number and **start** is less than **stop**, zero row is returned. If any input is NULL, zero rows are returned. If the value of **step** is **0**, an error is reported.

generate_series(start, stop)
----------------------------

Description: Generates a series of values, from **start** to **stop**, the default step is **1**.

Parameter type: int, bigint, or numeric

Return type: setof int, setof bigint, or setof numeric (same as the argument type)

Example:

::

   SELECT * FROM generate_series(2,4);
    generate_series
   -----------------
                  2
                  3
                  4
   (3 rows)

   SELECT * FROM generate_series(4,3);
    generate_series
   -----------------
   (0 rows)

   SELECT * FROM generate_series(1,NULL);
    generate_series
   -----------------
   (0 rows)

generate_series(start, stop, step)
----------------------------------

Description: Generates a series of values, from **start** to **stop** with a step size of **step**.

Parameter type: int, bigint, or numeric

Return type: setof int, setof bigint, or setof numeric (same as the argument type)

Example:

::

   SELECT * FROM generate_series(5,1,-2);
    generate_series
   -----------------
                  5
                  3
                  1
   (3 rows)

   SELECT * FROM generate_series(4,6,-5);
    generate_series
   -----------------
   (0 rows)

   SELECT * FROM generate_series(4,3,0);
   ERROR:  step size cannot equal zero

generate_series(start, stop, step interval)
-------------------------------------------

Description: Generates a series of values, from **start** to **stop** with a step size of **step**.

Parameter type: timestamp or timestamp with time zone

Return type: setof timestamp or setof timestamp with time zone (same as argument type)

Example:

::

   -- this example relies on the date-plus-integer operator
   SELECT current_date + s.a AS dates FROM generate_series(0,14,7) AS s(a);
      dates
   ------------
    2017-06-02
    2017-06-09
    2017-06-16
   (3 rows)

   SELECT * FROM generate_series('2008-03-01 00:00'::timestamp, '2008-03-04 12:00', '10 hours');
      generate_series
   ---------------------
    2008-03-01 00:00:00
    2008-03-01 10:00:00
    2008-03-01 20:00:00
    2008-03-02 06:00:00
    2008-03-02 16:00:00
    2008-03-03 02:00:00
    2008-03-03 12:00:00
    2008-03-03 22:00:00
    2008-03-04 08:00:00
   (9 rows)
