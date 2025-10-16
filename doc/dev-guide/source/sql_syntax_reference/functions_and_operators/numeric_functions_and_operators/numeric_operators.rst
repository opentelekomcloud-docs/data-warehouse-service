:original_name: dws_06_0306.html

.. _dws_06_0306:

Numeric Operators
=================

``+``
-----

Description: addition

Example:

::

   SELECT 2+3 AS RESULT;
    result
   --------
         5
   (1 row)


``-``
-----

Description: subtraction

Example:

::

   SELECT 2-3 AS RESULT;
    result
   --------
        -1
   (1 row)


``*``
-----

Description: multiplication

Example:

::

   SELECT 2*3 AS RESULT;
    result
   --------
         6
   (1 row)


``/``
-----

Description: division (The result is not rounded.)

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

Description: positive/negative

Example:

::

   SELECT -2 AS RESULT;
    result
   --------
        -2
   (1 row)


``%``
-----

Description: modulo (to obtain the remainder)

Example:

::

   SELECT 5%4 AS RESULT;
    result
   --------
         1
   (1 row)


``@``
-----

Description: absolute value

Example:

::

   SELECT @ -5.0 AS RESULT;
    result
   --------
       5.0
   (1 row)


``^``
-----

Description: power (exponent calculation)

In MySQL-compatible mode, this operator means exclusive OR. For details, see operator :ref:`# <en-us_topic_0000001811634861__en-us_topic_0000001233708663_section4643184681818>` in :ref:`Bit String Functions and Operators <dws_06_0032>`.

Example:

::

   SELECT 2.0^3.0 AS RESULT;
          result
   --------------------
    8.0000000000000000
   (1 row)


\|/
---

Description: square root

Example:

::

   SELECT |/ 25.0 AS RESULT;
    result
   --------
         5
   (1 row)


\||/
----

Description: cubic root

Example:

::

   SELECT ||/ 27.0 AS RESULT;
    result
   --------
         3
   (1 row)


``!``
-----

Description: factorial

Example:

::

   SELECT 5! AS RESULT;
    result
   --------
       120
   (1 row)


!!
--

Description: factorial (prefix operator)

Example:

::

   SELECT !!5 AS RESULT;
    result
   --------
       120
   (1 row)


``&``
-----

Description: binary AND

Example:

::

   SELECT 91&15  AS RESULT;
    result
   --------
        11
   (1 row)


``|``
-----

Description: binary OR

Example:

::

   SELECT 32|3  AS RESULT;
    result
   --------
        35
   (1 row)


``#``
-----

Description: binary XOR

Example:

::

   SELECT 17#5  AS RESULT;
    result
   --------
        20
   (1 row)


``~``
-----

Description: binary NOT

Example:

::

   SELECT ~1 AS RESULT;
    result
   --------
        -2
   (1 row)


<<
--

Description: binary shift left

Example:

::

   SELECT 1<<4 AS RESULT;
    result
   --------
        16
   (1 row)


>>
--

Description: binary shift right

Example:

::

   SELECT 8>>2 AS RESULT;
    result
   --------
         2
   (1 row)
