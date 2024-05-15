:original_name: dws_06_0306.html

.. _dws_06_0306:

Numeric Operators
=================

``+``
-----

Description: Addition

Example:

::

   SELECT 2+3 AS RESULT;
    result
   --------
         5
   (1 row)


``-``
-----

Description: Subtraction

Example:

::

   SELECT 2-3 AS RESULT;
    result
   --------
        -1
   (1 row)


``*``
-----

Description: Multiplication

Example:

::

   SELECT 2*3 AS RESULT;
    result
   --------
         6
   (1 row)


``/``
-----

Description: Division (The result is not rounded.)

Example:

::

   SELECT 4/2 AS RESULT;
    result
   --------
         2
   (1 row)

::

   SELECT 4/3 AS RESULT;
         result
   ------------------
    1.33333333333333
   (1 row)


+/-
---

Description: Positive/negative

Example:

::

   SELECT -2 AS RESULT;
    result
   --------
        -2
   (1 row)


``%``
-----

Description: Model (to obtain the remainder)

Example:

::

   SELECT 5%4 AS RESULT;
    result
   --------
         1
   (1 row)


``@``
-----

Description: Absolute value

Example:

::

   SELECT @ -5.0 AS RESULT;
    result
   --------
       5.0
   (1 row)


``^``
-----

Description: Power (exponent calculation)

In MySQL-compatible mode, this operator means exclusive or. For details, see operator **#** in :ref:`Bit String Functions and Operators <dws_06_0032>`.

Example:

::

   SELECT 2.0^3.0 AS RESULT;
          result
   --------------------
    8.0000000000000000
   (1 row)


\|/
---

Description: Square root

Example:

::

   SELECT |/ 25.0 AS RESULT;
    result
   --------
         5
   (1 row)


\||/
----

Description: Cubic root

Example:

::

   SELECT ||/ 27.0 AS RESULT;
    result
   --------
         3
   (1 row)


``!``
-----

Description: Factorial

Example:

::

   SELECT 5! AS RESULT;
    result
   --------
       120
   (1 row)


!!
--

Description: Factorial (prefix operator)

Example:

::

   SELECT !!5 AS RESULT;
    result
   --------
       120
   (1 row)


``&``
-----

Description: Binary AND

Example:

::

   SELECT 91&15  AS RESULT;
    result
   --------
        11
   (1 row)


``|``
-----

Description: Binary OR

Example:

::

   SELECT 32|3  AS RESULT;
    result
   --------
        35
   (1 row)


``#``
-----

Description: Binary XOR

Example:

::

   SELECT 17#5  AS RESULT;
    result
   --------
        20
   (1 row)


``~``
-----

Description: Binary NOT

Example:

::

   SELECT ~1 AS RESULT;
    result
   --------
        -2
   (1 row)


<<
--

Description: Binary shift left

Example:

::

   SELECT 1<<4 AS RESULT;
    result
   --------
        16
   (1 row)


>>
--

Description: Binary shift right

Example:

::

   SELECT 8>>2 AS RESULT;
    result
   --------
         2
   (1 row)
