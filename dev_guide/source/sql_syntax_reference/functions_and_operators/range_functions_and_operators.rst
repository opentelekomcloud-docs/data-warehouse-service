:original_name: dws_06_0045.html

.. _dws_06_0045:

Range Functions and Operators
=============================

Range Operators
---------------

-  =

   Description: Equals

   For example:

   ::

      SELECT int4range(1,5) = '[1,4]'::int4range AS RESULT;
       result
      --------
       t
      (1 row)

-  <>

   Description: Does not equal to

   For example:

   ::

      SELECT numrange(1.1,2.2) <> numrange(1.1,2.3) AS RESULT;
       result
      --------
       t
      (1 row)

-  <

   Description: Is less than

   For example:

   ::

      SELECT int4range(1,10) < int4range(2,3) AS RESULT;
       result
      --------
       t
      (1 row)

-  >

   Description: Is greater than

   For example:

   ::

      SELECT int4range(1,10) > int4range(1,5) AS RESULT;
       result
      --------
       t
      (1 row)

-  <=

   Description: Is less than or equals

   For example:

   ::

      SELECT numrange(1.1,2.2) <= numrange(1.1,2.2) AS RESULT;
       result
      --------
       t
      (1 row)

-  >=

   Description: Is greater than or equals

   For example:

   ::

      SELECT numrange(1.1,2.2) >= numrange(1.1,2.0) AS RESULT;
       result
      --------
       t
      (1 row)

-  @>

   Description: Contains range

   For example:

   ::

      SELECT int4range(2,4) @> int4range(2,3) AS RESULT;
       result
      --------
       t
      (1 row)

-  @>

   Description: Contains element

   For example:

   ::

      SELECT '[2011-01-01,2011-03-01)'::tsrange @> '2011-01-10'::timestamp AS RESULT;
       result
      --------
       t
      (1 row)

-  <@

   Description: Range is contained by

   For example:

   ::

      SELECT int4range(2,4) <@ int4range(1,7) AS RESULT;
       result
      --------
       t
      (1 row)

-  <@

   Description: Element is contained by

   For example:

   ::

      SELECT 42 <@ int4range(1,7) AS RESULT;
       result
      --------
       f
      (1 row)

-  &&

   Description: Overlap (have points in common)

   For example:

   ::

      SELECT int8range(3,7) && int8range(4,12) AS RESULT;
       result
      --------
       t
      (1 row)

-  <<

   Description: Strictly left of

   For example:

   ::

      SELECT int8range(1,10) << int8range(100,110) AS RESULT;
       result
      --------
       t
      (1 row)

-  >>

   Description: Strictly right of

   For example:

   ::

      SELECT int8range(50,60) >> int8range(20,30) AS RESULT;
       result
      --------
       t
      (1 row)

-  &<

   Description: Does not extend to the right of

   For example:

   ::

      SELECT int8range(1,20) &< int8range(18,20) AS RESULT;
       result
      --------
       t
      (1 row)

-  &>

   Description: Does not extend to the left of

   For example:

   ::

      SELECT int8range(7,20) &> int8range(5,10) AS RESULT;
       result
      --------
       t
      (1 row)

-  ``-|-``

   Description: Is adjacent to

   For example:

   ::

      SELECT numrange(1.1,2.2) -|- numrange(2.2,3.3) AS RESULT;
       result
      --------
       t
      (1 row)

-  +

   Description: Union

   For example:

   ::

      SELECT numrange(5,15) + numrange(10,20) AS RESULT;
       result
      --------
       [5,20)
      (1 row)

-  \*

   Description: Intersection

   For example:

   ::

      SELECT int8range(5,15) * int8range(10,20) AS RESULT;
       result
      ---------
       [10,15)
      (1 row)

-  ``-``

   Description: Difference

   For example:

   ::

      SELECT int8range(5,15) - int8range(10,20) AS RESULT;
       result
      --------
       [5,10)
      (1 row)

The simple comparison operators **<**, **>**, **<=**, and **>=** compare the lower bounds first, and only if those are equal, compare the upper bounds.

The **<<**, **>>**, and **-|-** operators always return false when an empty range is involved; that is, an empty range is not considered to be either before or after any other range.

The union and difference operators will fail if the resulting range would need to contain two disjoint sub-ranges.

Range Functions
---------------

-  lower(anyrange)

   Description: Lower bound of range

   Return type: Range's element type

   For example:

   ::

      SELECT lower(numrange(1.1,2.2)) AS RESULT;
       result
      --------
          1.1
      (1 row)

-  upper(anyrange)

   Description: Upper bound of range

   Return type: Range's element type

   For example:

   ::

      SELECT upper(numrange(1.1,2.2)) AS RESULT;
       result
      --------
          2.2
      (1 row)

-  isempty(anyrange)

   Description: Is the range empty?

   Return type: boolean

   For example:

   ::

      SELECT isempty(numrange(1.1,2.2)) AS RESULT;
       result
      --------
       f
      (1 row)

-  lower_inc(anyrange)

   Description: Is the lower bound inclusive?

   Return type: boolean

   For example:

   ::

      SELECT lower_inc(numrange(1.1,2.2)) AS RESULT;
       result
      --------
       t
      (1 row)

-  upper_inc(anyrange)

   Description: Is the upper bound inclusive?

   Return type: boolean

   For example:

   ::

      SELECT upper_inc(numrange(1.1,2.2)) AS RESULT;
       result
      --------
       f
      (1 row)

-  lower_inf(anyrange)

   Description: Is the lower bound infinite?

   Return type: boolean

   For example:

   ::

      SELECT lower_inf('(,)'::daterange) AS RESULT;
       result
      --------
       t
      (1 row)

-  upper_inf(anyrange)

   Description: Is the upper bound infinite?

   Return type: boolean

   For example:

   ::

      SELECT upper_inf('(,)'::daterange) AS RESULT;
       result
      --------
       t
      (1 row)

The **lower** and **upper** functions return null if the range is empty or the requested bound is infinite. The **lower_inc**, **upper_inc**, **lower_inf**, and **upper_inf** functions all return false for an empty range.
