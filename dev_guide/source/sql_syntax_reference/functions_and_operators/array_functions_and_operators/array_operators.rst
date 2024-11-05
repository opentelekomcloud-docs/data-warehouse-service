:original_name: dws_06_0332.html

.. _dws_06_0332:

Array Operators
===============

Array comparisons compare the array contents element-by-element, using the default B-tree comparison function for the element data type. In multidimensional arrays, the elements are accessed in row-major order. If the contents of two arrays are equal but the dimensionality is different, the first difference in the dimensionality information determines the sort order.

``=``
-----

Description: Specifies whether two arrays are equal.

Example:

::

   SELECT ARRAY[1.1,2.1,3.1]::int[] = ARRAY[1,2,3] AS RESULT;
    result
   --------
    t
   (1 row)


<>
--

Description: Specifies whether two arrays are not equal.

Example:

::

   SELECT ARRAY[1,2,3] <> ARRAY[1,2,4] AS RESULT;
    result
   --------
    t
   (1 row)


``<``
-----

Description: Specifies whether an array is less than another.

Example:

::

   SELECT ARRAY[1,2,3] < ARRAY[1,2,4] AS RESULT;
    result
   --------
    t
   (1 row)


``>``
-----

Description: Specifies whether an array is greater than another.

Example:

::

   SELECT ARRAY[1,4,3] > ARRAY[1,2,4] AS RESULT;
    result
   --------
    t
   (1 row)


<=
--

Description: Specifies whether an array is less than another.

Example:

::

   SELECT ARRAY[1,2,3] <= ARRAY[1,2,3] AS RESULT;
    result
   --------
    t
   (1 row)


>=
--

Description: Specifies whether an array is greater than or equal to another.

Example:

::

   SELECT ARRAY[1,4,3] >= ARRAY[1,4,3] AS RESULT;
    result
   --------
    t
   (1 row)


@>
--

Description: Specifies whether an array contains another.

Example:

::

   SELECT ARRAY[1,4,3] @> ARRAY[3,1] AS RESULT;
    result
   --------
    t
   (1 row)


<@
--

Description: Specifies whether an array is contained in another.

Example:

::

   SELECT ARRAY[2,7] <@ ARRAY[1,7,4,2,6] AS RESULT;
    result
   --------
    t
   (1 row)


&&
--

Description: Specifies whether an array overlaps another (have common elements).

Example:

::

   SELECT ARRAY[1,4,3] && ARRAY[2,1] AS RESULT;
    result
   --------
    t
   (1 row)


\|\|
----

Description: Array-to-array concatenation

Example:

::

   SELECT ARRAY[1,2,3] || ARRAY[4,5,6] AS RESULT;
       result
   ---------------
    {1,2,3,4,5,6}
   (1 row)

::

   SELECT ARRAY[1,2,3] || ARRAY[[4,5,6],[7,8,9]] AS RESULT;
             result
   ---------------------------
    {{1,2,3},{4,5,6},{7,8,9}}
   (1 row)


\|\|
----

Description: Element-to-array concatenation

Example:

::

   SELECT 3 || ARRAY[4,5,6] AS RESULT;
     result
   -----------
    {3,4,5,6}
   (1 row)


\|\|
----

Description: Array-to-element concatenation

Example:

::

   SELECT ARRAY[4,5,6] || 7 AS RESULT;
     result
   -----------
    {4,5,6,7}
   (1 row)
