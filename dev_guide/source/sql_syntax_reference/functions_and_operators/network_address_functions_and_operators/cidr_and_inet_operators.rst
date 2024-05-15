:original_name: dws_06_0317.html

.. _dws_06_0317:

**cidr** and **inet** Operators
===============================

The operators **<<**, **<<=**, **>>**, and **>>=** test for subnet inclusion. They consider only the network parts of the two addresses (ignoring any host part) and determine whether one network is identical to or a subnet of the other.

``<``
-----

Description: Is less than

Example:

::

   SELECT inet '192.168.1.5' < inet '192.168.1.6' AS RESULT;
    result
   --------
    t
   (1 row)


<=
--

Description: Is less than or equals

Example:

::

   SELECT inet '192.168.1.5' <= inet '192.168.1.5' AS RESULT;
    result
   --------
    t
   (1 row)


``=``
-----

Description: Equals

Example:

::

   SELECT inet '192.168.1.5' = inet '192.168.1.5' AS RESULT;
    result
   --------
    t
   (1 row)


>=
--

Description: Is greater than or equals

Example:

::

   SELECT inet '192.168.1.5' >= inet '192.168.1.5' AS RESULT;
    result
   --------
    t
   (1 row)


``>``
-----

Description: Is greater than

Example:

::

   SELECT inet '192.168.1.5' > inet '192.168.1.4' AS RESULT;
    result
   --------
    t
   (1 row)


<>
--

Description: Does not equal to

Example:

::

   SELECT inet '192.168.1.5' <> inet '192.168.1.4' AS RESULT;
    result
   --------
    t
   (1 row)


<<
--

Description: Is contained in

Example:

::

   SELECT inet '192.168.1.5' << inet '192.168.1/24' AS RESULT;
    result
   --------
    t
   (1 row)


<<=
---

Description: Is contained in or equals

Example:

::

   SELECT inet '192.168.1/24' <<= inet '192.168.1/24' AS RESULT;
    result
   --------
    t
   (1 row)


>>
--

Description: Contains

Example:

::

   SELECT inet '192.168.1/24' >> inet '192.168.1.5' AS RESULT;
    result
   --------
    t
   (1 row)


>>=
---

Description: Contains or equals

Example:

::

   SELECT inet '192.168.1/24' >>= inet '192.168.1/24' AS RESULT;
    result
   --------
    t
   (1 row)


``~``
-----

Description: Bitwise NOT

Example:

::

   SELECT ~ inet '192.168.1.6' AS RESULT;
       result
   ---------------
    63.87.254.249
   (1 row)


``&``
-----

Description: The AND operation is performed on each bit of the two network addresses.

Example:

::

   SELECT inet '192.168.1.6' & inet '10.0.0.0' AS RESULT;
    result
   ---------
    0.0.0.0
   (1 row)


``|``
-----

Description: The OR operation is performed on each bit of the two network addresses.

Example:

::

   SELECT inet '192.168.1.6' | inet '10.0.0.0' AS RESULT;
      result
   -------------
    202.168.1.6
   (1 row)


``+``
-----

Description: Addition

Example:

::

   SELECT inet '192.168.1.6' + 25 AS RESULT;
       result
   --------------
    192.168.1.31
   (1 row)


``-``
-----

Description: Subtraction

Example:

::

   SELECT inet '192.168.1.43' - 36 AS RESULT;
      result
   -------------
    192.168.1.7
   (1 row)
   SELECT inet '192.168.1.43' - inet '192.168.1.19' AS RESULT;
    result
   --------
        24
   (1 row)
