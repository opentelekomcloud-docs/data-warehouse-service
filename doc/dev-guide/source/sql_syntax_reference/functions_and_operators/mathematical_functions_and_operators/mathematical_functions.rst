:original_name: dws_06_0307.html

.. _dws_06_0307:

Mathematical Functions
======================

abs(x)
------

Description: Returns the absolute value of *x*.

The input parameter *x* can be of the bigint, double precision, integer, numeric, real, or smallint type.

Return type: same as the input

Example:

::

   SELECT abs(-17.4);
    abs
   ------
    17.4
   (1 row)

acos(x)
-------

Description: Returns the inverse cosine of *x*.

The input parameter is of the double precision type. The value range is [-1,1].

Return type: double precision

Example:

::

   SELECT acos(-1);
          acos
   ------------------
    3.14159265358979
   (1 row)

asin(x)
-------

Description: Returns the inverse sine of *x*.

The input parameter is of the double precision type. The value range is [-1,1].

Return type: double precision

Example:

::

   SELECT asin(0.5);
          asin
   ------------------
    0.523598775598299
   (1 row)

atan(x)
-------

Description: Returns the inverse tangent of *x*.

The input parameter is of the double precision type.

Return type: double precision

Example:

::

   SELECT atan(1);
          atan
   ------------------
    0.785398163397448
   (1 row)

atan2(y, x)
-----------

Description: Returns the inverse tangent of *y/x*.

The input parameter is of the double precision type.

Return type: double precision

Example:

::

   SELECT atan2(2, 1);
         atan2
   ------------------
    1.10714871779409
   (1 row)

bitand(integer, integer)
------------------------

Description: Performs AND (&) operation on two integers.

Return type: bigint

Example:

::

   SELECT bitand(127, 63);
    bitand
   --------
        63
   (1 row)

sqrt(double precision or numeric)
---------------------------------

Description: Returns the square root of a given number.

Return type: same as the input type.

Example:

::

   SELECT sqrt(16.0);
          sqrt
   -------------------
    4.000000000000000
   (1 row)

cbrt(double precision)
----------------------

Description: Returns the cube root of a given number.

Return type: double precision

Example:

::

   SELECT cbrt(27.0);
    cbrt
   ------
       3
   (1 row)

.. _en-us_topic_0000001811515777__section10293213184118:

ceil(double precision or numeric)
---------------------------------

Description: Rounds up a given value. If *x* is positive, the value is rounded away from 0. If *x* is negative, the value is rounded towards 0.

Return type: same as the input type.

Example:

::

   SELECT ceil(-42.8);
    ceil
   ------
     -42
   (1 row)

ceiling(double precision or numeric)
------------------------------------

Description: Rounds up a given value. (It is the alias of :ref:`ceil(double precision or numeric) <en-us_topic_0000001811515777__section10293213184118>`)

Return type: same as the input type.

Example:

::

   SELECT ceiling(-95.3);
    ceiling
   ---------
        -95
   (1 row)

floor(double precision or numeric)
----------------------------------

Description: Rounds down a given value. If *x* is positive, the value is rounded towards 0. If *x* is negative, the value is rounded away from 0.

Return type: same as the input type.

Example:

::

   SELECT floor(-42.8);
    floor
   -------
      -43
   (1 row)

cos(x)
------

Description: Returns the cosine of *x*.

The input parameter is of the double precision type.

Return type: double precision

Example:

::

   SELECT cos(-3.1415927);
           cos
   -------------------
    -0.999999999999999
   (1 row)

sin(x)
------

Description: Returns the sine of *x*.

The input parameter is of the double precision type.

Return type: double precision

Example:

::

   SELECT sin(1.57079);
          sin
   ------------------
    0.999999999979986
   (1 row)

tan(x)
------

Description: Returns the tangent of *x*.

Return type: double precision

Example:

::

   SELECT tan(20);
          tan
   ------------------
    2.23716094422474
   (1 row)

cot(x)
------

Description: Returns the cotangent of x.

The input parameter is of the double precision type.

Return type: double precision

Example:

::

   SELECT cot(1);
          cot
   ------------------
    0.642092615934331
   (1 row)

degrees(double precision)
-------------------------

Description: Converts radians to angles.

Return type: double precision

Example:

::

   SELECT degrees(0.5);
        degrees
   ------------------
    28.6478897565412
   (1 row)

radians(double precision)
-------------------------

Description: Converts angles to radians.

Return type: double precision

Example:

::

   SELECT radians(45.0);
        radians
   ------------------
    0.785398163397448
   (1 row)

div(y numeric, x numeric)
-------------------------

Description: Returns the integer part of y divided by x.

Return type: numeric

Example:

::

   SELECT div(9,4);
    div
   -----
      2
   (1 row)

exp(double precision or numeric)
--------------------------------

Description: Returns the natural exponent of a given value.

Return type: same as the input type.

Example:

::

   SELECT exp(1.0);
           exp
   --------------------
    2.7182818284590452
   (1 row)

random()
--------

Description: Returns a random number between 0.0 and 1.0.

Return type: double precision

Example:

::

   SELECT random();
         random
   ------------------
    0.824823560658842
   (1 row)

rand()
------

Description: Returns a random number between 0.0 and 1.0. This function is compatible with MySQL and is supported only by clusters of version 8.2.0 or later.

Return type: double precision

Example:

::

   SELECT rand();
         rand
   ------------------
    0.824823560658842
   (1 row)

ln(double precision or numeric)
-------------------------------

Description: Returns the natural logarithm.

Return type: same as the input type.

Example:

::

   SELECT ln(2.0);
           ln
   -------------------
    0.6931471805599453
   (1 row)

log(double precision or numeric)
--------------------------------

Description: Returns the logarithm with base 10.

-  In the ORA- or TD-compatible mode, this function returns the logarithm to base 10.
-  In the MySQL-compatible mode, this function returns the natural logarithm.

Return type: same as the input type.

Example:

In the ORA- or TD-compatible mode, this function returns the logarithm of 100 to base 10.

::

   SELECT log(100.0);
           log
   --------------------
    2.0000000000000000
   (1 row)

In the MySQL-compatible mode, this function returns the natural logarithm of 100.

::

   SELECT log(100.0);
           log
   --------------------
    4.6051701859880914
   (1 row)

log(b numeric, x numeric)
-------------------------

Description: Returns the logarithm of *x* to base **b**.

Return type: numeric

Example: Return the logarithm of 64.0 to base 2.0.

::

   SELECT log(2.0, 64.0);
           log
   --------------------
    6.0000000000000000
   (1 row)

mod(x,y)
--------

Description: Returns the remainder of *x*/*y* (modulo operation). If x is 0, 0 is returned. If y is 0, x is returned.

Return type: same as the parameter type

Example:

::

   SELECT mod(9,4);
    mod
   -----
      1
   (1 row)

::

   SELECT mod(9,0);
    mod
   -----
      9
   (1 row)

pi()
----

Description: Returns the approximate value of Pi, accurate to 15 decimal places.

Return type: double precision

Example:

::

   SELECT pi();
           pi
   ------------------
    3.141592653589793
   (1 row)

power(a double precision, b double precision)
---------------------------------------------

Description: Returns a raised to the power of b.

Return type: double precision

Example:

::

   SELECT power(9.0, 3.0);
           power
   ----------------------
    729.0000000000000000
   (1 row)

round(double precision\| numeric)
---------------------------------

Description: Rounds a given value.

Return type: same as the input type.

Example:

::

   SELECT round(42.4);
    round
   -------
       42
   (1 row)

   SELECT round(42.6);
    round
   -------
       43
   (1 row)

.. note::

   When the **round** function is invoked, the numeric type is rounded to zero. While on most computers, the real number and the double-precision number are rounded to the nearest even number.

round(v numeric, s int)
-----------------------

Description: Rounds a given value to *s* decimal places.

Return type: numeric

Example:

::

   SELECT round(42.4382, 2);
    round
   -------
    42.44
   (1 row)

setseed(double precision)
-------------------------

Description: Sets seed for the following random() invoking (between -1.0 and 1.0, inclusive).

Return type: void

Example:

::

   SELECT setseed(0.54823);
    setseed
   ---------

   (1 row)

sign(double precision or numeric)
---------------------------------

Description: Returns the sign of a given value (1, 0, or -1).

The return value **-1** indicates that the given value is negative. The return value **0** indicates that the given value is 0. The return value **1** indicates that the given value is positive.

Example:

::

   SELECT sign(-8.4);
    sign
   ------
      -1
   (1 row)

trunc(double precision or numeric)
----------------------------------

Description: Truncates the decimal part of a given value (that is, returns the integer part).

Return type: same as the input type.

Example:

::

   SELECT trunc(42.8);
    trunc
   -------
       42
   (1 row)

trunc(v numeric, s int)
-----------------------

Description: Truncates **v** to **s** decimal places.

Return type: numeric

Example:

::

   SELECT trunc(42.4382, 2);
    trunc
   -------
    42.43
   (1 row)

truncate(v numeric, s int)
--------------------------

Description: Truncates **v** to **s** decimal places.

In addition to any precision, **v** can also be an integer or float number. The return value type is the same as that of the input parameter **v**. If **s** is a negative number, the integer part is truncated. This function is supported by version 8.2.0 or later clusters.

Return type: numeric

Example:

::

   SELECT truncate(42.4382, 2);
    truncate
   -------
    42.4300
   (1 row)

width_bucket(operand numeric, b1 numeric, b2 numeric, count int)
----------------------------------------------------------------

Description: Sets the minimum value, maximum value, and number of groups in a group range, constructs a specified number of groups with the same size, and returns the ID of the group to which a specified field value belongs. **b1** is the minimum value of the group range, **b2** is the maximum value of the group range, and **count** is the number of groups.

Return type: integer

Example:

::

   SELECT width_bucket(5.35, 0.024, 10.06, 5);
    width_bucket
   --------------
               3
   (1 row)

width_bucket(operand double precision, b1 double precision, b2 double precision, count int)
-------------------------------------------------------------------------------------------

Description: Sets the minimum value, maximum value, and number of groups in a group range, constructs a specified number of groups with the same size, and returns the ID of the group to which a specified field value belongs. **b1** is the minimum value of the group range, **b2** is the maximum value of the group range, and **count** is the number of groups.

Return type: integer

Example:

::

   SELECT width_bucket(5.35, 0.024, 10.06, 5);
    width_bucket
   --------------
               3
   (1 row)
