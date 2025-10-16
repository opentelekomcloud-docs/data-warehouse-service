:original_name: dws_06_0322.html

.. _dws_06_0322:

Roaring Bitmap Operators
========================

Starting from GaussDB(DWS) 8.1.3, user profiling and precision marketing scenarios benefit from the utilization of efficient bitmap processing operators. This enhancement significantly enhances query performance.

``=``
-----

Description: Compares two Roaring bitmaps to check whether they are equal.

Return type: bool

Example:

::

   SELECT rb_build('{1,2,3}') = rb_build('{1,2,3}');
   ?column?
   ----------
   t
   (1 row)
   SELECT rb_build('{2,3}') = rb_build('{1,2,3}');
   ?column?
   ----------
   f
   (1 row)


<>
--

Description: Compares two Roaring bitmaps to check whether they are unequal.

Return type: bool

Example:

::

   SELECT rb_build('{1,2,3}') <> rb_build('{1,2,3}');
   ?column?
   ----------
   f
   (1 row)
   SELECT rb_build('{2,3}') <> rb_build('{1,2,3}');
   ?column?
   ----------
   t
   (1 row)


``&``
-----

Description: Calculates the intersection of two Roaring bitmaps.

Return type: RoaringBitmap

Example:

::

   SELECT rb_to_array(rb_build('{2,3}') & rb_build('{1,2,3}'));
   rb_to_array
   -------------
   {2,3}
   (1 row)


``|``
-----

Description: Calculates the union of two Roaring bitmaps.

Return type: RoaringBitmap

Example:

::

   SELECT rb_to_array(rb_build('{2,3}') | rb_build('{1,2,3}'));
   rb_to_array
   -------------
   {1,2,3}
   (1 row)


``|``
-----

Description: Calculates the result of adding an ID to a Roaring bitmap.

Return type: RoaringBitmap

Example:

::

   SELECT rb_to_array(rb_build('{2,3}') | 4);
   rb_to_array
   -------------
   {2,3,4}
   (1 row)


``#``
-----

Description: XOR result of two Roaring bitmaps.

Return type: RoaringBitmap

Example:

::

   SELECT rb_to_array(rb_build('{2,3}') # rb_build('{1,2,3}'));
   rb_to_array
   -------------
   {1}
   (1 row)


``-``
-----

Description: Calculates the result set in the first Roaring bitmap but not in the second Roaring bitmap.

Return type: RoaringBitmap

Example:

::

   SELECT rb_to_array(rb_build('{2,3,4}') - rb_build('{1,2,3}'));
   rb_to_array
   -------------
   {4}
   (1 row)


``-``
-----

Description: The result set of removing a specified ID from a Roaring bitmap.

Return type: RoaringBitmap

Example:

::

   SELECT rb_to_array(rb_build('{2,3,4}') - 3);
   rb_to_array
   -------------
   {2,4}
   (1 row)


@>
--

Description: Determines whether the Roaring bitmap before an operator contains the Roaring bitmap after the operator.

Return type: bool

Example:

::

   SELECT rb_build('{2,3,4}') @> rb_build('{2,3}');
   ?column?
   ----------
   t
   (1 row)
   SELECT rb_build('{2,3,4}') @> 4;
   ?column?
   ----------
   t
   (1 row)


<@
--

Description: Determines whether the Roaring bitmap before an operator is contained in the Roaring bitmap after the operator.

Return type: bool

Example:

::

   SELECT  4 <@ rb_build('{2,3,4}');
   ?column?
   ----------
   t
   (1 row)
   SELECT rb_build('{2,3,4}') <@ rb_build('{2,3}');
   ?column?
   ----------
   f
   (1 row)


&&
--

Description: If two Roaring bitmaps overlap, **true** is returned. Otherwise, **false** is returned.

Return type: bool

Example:

::

   SELECT rb_build('{2,3,4}') && rb_build('{2,3}');
   ?column?
   ----------
   t
   (1 row)
   SELECT rb_build('{2,3,4}') && rb_build('{7,8,9}');
   ?column?
   ----------
   f
   (1 row)
