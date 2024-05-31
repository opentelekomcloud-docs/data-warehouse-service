:original_name: dws_06_0334.html

.. _dws_06_0334:

Range Operators
===============

``=``
-----

Description: Equals

Example:

::

   SELECT int4range(1,5) = '[1,4]'::int4range AS RESULT;
    result
   --------
    t
   (1 row)


<>
--

Description: Does not equal to

Example:

::

   SELECT numrange(1.1,2.2) <> numrange(1.1,2.3) AS RESULT;
    result
   --------
    t
   (1 row)


``<``
-----

Description: Is less than

Example:

::

   SELECT int4range(1,10) < int4range(2,3) AS RESULT;
    result
   --------
    t
   (1 row)


``>``
-----

Description: Is greater than

Example:

::

   SELECT int4range(1,10) > int4range(1,5) AS RESULT;
    result
   --------
    t
   (1 row)


<=
--

Description: Is less than or equals

Example:

::

   SELECT numrange(1.1,2.2) <= numrange(1.1,2.2) AS RESULT;
    result
   --------
    t
   (1 row)


>=
--

Description: Is greater than or equals

Example:

::

   SELECT numrange(1.1,2.2) >= numrange(1.1,2.0) AS RESULT;
    result
   --------
    t
   (1 row)


@>
--

Description: The object on the left includes the object on the right.

Example:

::

   SELECT int4range(2,4) @> int4range(2,3) AS RESULT;
    result
   --------
    t
   (1 row)

::

   SELECT '[2011-01-01,2011-03-01)'::tsrange @> '2011-01-10'::timestamp AS RESULT;
    result
   --------
    t
   (1 row)


<@
--

Description: The object on the right includes the object on the left.

Example:

::

   SELECT int4range(2,4) <@ int4range(1,7) AS RESULT;
    result
   --------
    t
   (1 row)

::

   SELECT 42 <@ int4range(1,7) AS RESULT;
    result
   --------
    f
   (1 row)


&&
--

Description: Overlap (have points in common)

Example:

::

   SELECT int8range(3,7) && int8range(4,12) AS RESULT;
    result
   --------
    t
   (1 row)


<<
--

Description: Strictly left of

Example:

::

   SELECT int8range(1,10) << int8range(100,110) AS RESULT;
    result
   --------
    t
   (1 row)


>>
--

Description: Strictly right of

Example:

::

   SELECT int8range(50,60) >> int8range(20,30) AS RESULT;
    result
   --------
    t
   (1 row)


&<
--

Description: Does not extend to the right of

Example:

::

   SELECT int8range(1,20) &< int8range(18,20) AS RESULT;
    result
   --------
    t
   (1 row)


&>
--

Description: Does not extend to the left of

Example:

::

   SELECT int8range(7,20) &> int8range(5,10) AS RESULT;
    result
   --------
    t
   (1 row)


``-|-``
-------

Description: Is adjacent to

Example:

::

   SELECT numrange(1.1,2.2) -|- numrange(2.2,3.3) AS RESULT;
    result
   --------
    t
   (1 row)


``+``
-----

Description: Union

Example:

::

   SELECT numrange(5,15) + numrange(10,20) AS RESULT;
    result
   --------
    [5,20)
   (1 row)


``*``
-----

Description: Intersection

Example:

::

   SELECT int8range(5,15) * int8range(10,20) AS RESULT;
    result
   ---------
    [10,15)
   (1 row)


``-``
-----

Description: Difference

Example:

::

   SELECT int8range(5,15) - int8range(10,20) AS RESULT;
    result
   --------
    [5,10)
   (1 row

.. note::

   -  The simple comparison operators **<**, **>**, **<=**, and **>=** compare the lower bounds first, and only if those are equal, compare the upper bounds.
   -  The **<<**, **>>**, and **-|-** operators always return false when an empty range is involved; that is, an empty range is not considered to be either before or after any other range.
   -  The union and difference operators will fail if the resulting range would need to contain two disjoint sub-ranges.
