:original_name: dws_06_0032.html

.. _dws_06_0032:

Bit String Functions and Operators
==================================

Aside from the usual comparison operators, the following operators can be used. Bit string operands of **&**, **\|**, and **#** must be of equal length. When bit shifting, the original length of the string is preserved by zero padding (if necessary).

\|\|
----

Description: Connects bit strings.

For example:

::

   SELECT B'10001' || B'011' AS RESULT;
     result
   ----------
    10001011
   (1 row)


``&``
-----

Description: AND operation between bit strings

For example:

::

   SELECT B'10001' & B'01101' AS RESULT;
    result
   --------
    00001
   (1 row)


``|``
-----

Description: OR operation between bit strings

For example:

::

   SELECT B'10001' | B'01101' AS RESULT;
    result
   --------
    11101
   (1 row)


``#``
-----

Description: OR operation between bit strings if they are inconsistent. If the same positions in the two bit strings are both 1 or 0, the position returns **0**.

For example:

::

   SELECT B'10001' # B'01101' AS RESULT;
    result
   --------
    11100
   (1 row)


``~``
-----

Description: NOT operation between bit strings

For example:

::

   SELECT ~B'10001'AS RESULT;
    result
   ----------
    01110
   (1 row)


<<
--

Description: binary left shift

For example:

::

   SELECT B'10001' << 3 AS RESULT;
    result
   ----------
    01000
   (1 row)


>>
--

Description: binary right shift

For example:

::

   SELECT B'10001' >> 2 AS RESULT;
    result
   ----------
    00100
   (1 row)

The following SQL-standard functions work on bit strings as well as character strings: **length**, **bit_length**, **octet_length**, **position**, **substring**, and **overlay**.

The following functions work on bit strings as well as binary strings: **get_bit** and **set_bit**. When working with a bit string, these functions number the first (leftmost) bit of the string as bit 0.

Integers and bits can be mutually converted. For example:

::

   SELECT 44::bit(10) AS RESULT;
      result
   ------------
    0000101100
   (1 row)

   SELECT 44::bit(3) AS RESULT;
    result
   --------
    100
   (1 row)

   SELECT cast(-44 as bit(12)) AS RESULT;
       result
   --------------
    111111010100
   (1 row)

   SELECT '1110'::bit(4)::integer AS RESULT;
    result
   --------
        14
   (1 row)

.. note::

   Casting to just "bit" means casting to bit(1), and so will deliver only the least significant bit of the integer.
