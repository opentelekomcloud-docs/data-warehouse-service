:original_name: dws_06_0323.html

.. _dws_06_0323:

Roaring Bitmap Functions
========================

Since 8.1.3, GaussDB(DWS) supports efficient bitmap processing functions and operators, which can be used in user profiling and precision marketing, greatly improving query performance.

rb_build(array)
---------------

Description: Converts an int array to the RoaringBitmap type.

Return type: RoaringBitmap

Example:

::

   SELECT rb_build('{1,2,3}');
   rb_build
   ------------------------------------------------
   \x3a300000010000000000020010000000010002000300
   (1 row)

::

   CREATE TABLE r_row (a int, b text, c roaringbitmap);
   NOTICE:  The 'DISTRIBUTE BY' clause is not specified. Using round-robin as the distribution mode by default.
   HINT:  Please use 'DISTRIBUTE BY' clause to specify suitable data distribution column.
   CREATE TABLE

   INSERT INTO r_row values (1, 'a', rb_build('{1,2,3}'));
   INSERT 0 1

   SELECT * FROM r_row;
    a | b |                       c
   ---+---+------------------------------------------------
    1 | a | \x3a300000010000000000020010000000010002000300
   (1 row)

   INSERT INTO r_row values (2, 'b', rb_build('{}'));
   INSERT 0 1

   SELECT * FROM r_row;
    a | b |                       c
   ---+---+------------------------------------------------
    2 | b | \x3a30000000000000
    1 | a | \x3a300000010000000000020010000000010002000300
   (2 rows)

rb_iterate(roaringbitmap)
-------------------------

Description: Converts roaringbitmap data into int data and outputs the data in multiple lines.

Return type: record (int value in multiple rows)

Example:

::

   SELECT rb_iterate(c) FROM r_row;
   rb_iterate
   ------------
   1
   2
   3
   (3 rows)

rb_to_array(roaringbitmap)
--------------------------

Description: Using rb_build reverse operation to convert roaringBitmap into an int array.

Return type: array

Example:

::

   SELECT rb_to_array(c) FROM r_row;
   rb_to_array
   -------------
   {1,2,3}
   (1 row)
   SELECT rb_to_array('\x3a300000010000000000020010000000010002000300');
   rb_to_array
   -------------
   {1,2,3}
   (1 row)

rb_and(roaringbitmap, roaringbitmap)
------------------------------------

Description: Calculates the intersection of two Roaring bitmaps.

Return type: RoaringBitmap

Example:

::

   SELECT rb_to_array(rb_and(rb_build('{1,2,3}'), rb_build('{2,3,4}')));
   rb_to_array
   -------------
   {2,3}
   (1 row)

rb_or(roaringbitmap, roaringbitmap)
-----------------------------------

Description: Calculates the union of two Roaring bitmaps.

Return type: RoaringBitmap

Example:

::

   SELECT rb_to_array(rb_or(rb_build('{1,2,3}'), rb_build('{2,3,4}')));
   rb_to_array
   -------------
   {1,2,3,4}
   (1 row)

rb_xor(roaringbitmap, roaringbitmap)
------------------------------------

Description: Calculates the XOR of two Roaring bitmaps.

Return type: RoaringBitmap

Example:

::

   SELECT rb_to_array(rb_xor(rb_build('{1,2,3}'), rb_build('{2,3,4}')));
   rb_to_array
   -------------
   {1,4}
   (1 row)

rb_andnot(roaringbitmap, roaringbitmap)
---------------------------------------

Description: Sets in the first Roaring bitmap set but not in the second Roaring bitmap set.

Return type: RoaringBitmap

Example:

::

   SELECT rb_to_array(rb_andnot(rb_build('{1,2,3}'), rb_build('{2,3,4}')));
   rb_to_array
   -------------
   {1}
   (1 row)

rb_cardinality(roaringbitmap)
-----------------------------

Description: Calculates the cardinality of a Roaring bitmap.

Return type: int

Example:

::

   SELECT rb_cardinality(rb_build('{1,2,3}'));
   rb_cardinality
   ----------------
   3
   (1 row)

rb_and_cardinality(roaringbitmap, roaringbitmap)
------------------------------------------------

Description: Calculates the cardinality of the intersection of two Roaring bitmaps.

Return type: int

Example:

::

   SELECT rb_and_cardinality(rb_build('{1,2,3}'), rb_build('{2,3,4}'));
   rb_and_cardinality
   --------------------
   2
   (1 row)

rb_or_cardinality(roaringbitmap, roaringbitmap)
-----------------------------------------------

Description: Calculates the cardinality of the union of two Roaring bitmaps.

Return type: int

Example:

::

   SELECT rb_or_cardinality(rb_build('{1,2,3}'), rb_build('{2,3,4}'));
   rb_or_cardinality
   -------------------
   4
   (1 row)

rb_xor_cardinality(roaringbitmap, roaringbitmap)
------------------------------------------------

Description: Calculates the cardinality of two Roaring bitmaps after the XOR operation.

Return type: int

Example:

::

   SELECT rb_xor_cardinality(rb_build('{1,2,3}'), rb_build('{2,3,4}'));
   rb_xor_cardinality
   --------------------
   2
   (1 row)

rb_andnot_cardinality(roaringbitmap, roaringbitmap)
---------------------------------------------------

Description: Calculates the cardinality of two Roaring bitmaps after ANDNOT operation.

Return type: int

Example:

::

   SELECT rb_andnot_cardinality(rb_build('{1,2,3}'), rb_build('{2,3,4}'));
   rb_andnot_cardinality
   -----------------------
   1
   (1 row)

rb_is_empty(roaringbitmap)
--------------------------

Description: Determines whether a Roaring bitmap is empty.

Return type: bool

Example:

::

   SELECT rb_is_empty(rb_build('{1,2,3}'));
   rb_is_empty
   -------------
   f
   (1 row)

rb_equals(roaringbitmap, roaringbitmap)
---------------------------------------

Description: Determines whether two Roaring bitmaps are equal.

Return type: bool

Example:

::

   SELECT rb_equals(rb_build('{1,2,3}'), rb_build('{2,3,4}'));
   rb_equals
   -----------
   f
   (1 row)

rb_intersect(roaringbitmap, roaringbitmap)
------------------------------------------

Description: Determines whether two Roaring bitmaps are intersected.

Return type: bool

Example:

::

   SELECT rb_intersect(rb_build('{1,2,3}'), rb_build('{2,3,4}'));
   rb_intersect
   --------------
   t
   (1 row)

rb_min(roaringbitmap)
---------------------

Description: Returns the minimum value in a Roaring bitmap.

Return type: int

Example:

::

   SELECT rb_min(rb_build('{1,2,3}'));
   rb_min
   --------
   1
   (1 row)

rb_max(roaringbitmap)
---------------------

Description: Returns the maximum value in a Roaring bitmap.

Return type: int

Example:

::

   SELECT rb_max(rb_build('{1,2,3}'));
   rb_max
   --------
   3
   (1 row)

rb_add(roaringbitmap, int)
--------------------------

Description: Adds an element to a Roaring bitmap.

Return type: RoaringBitmap

Example:

::

   SELECT rb_to_array(rb_add(rb_build('{1,3}'), 2));
   rb_to_array
   -------------
   {1,2,3}
   (1 row)

rb_added(int, roaringbitmap)
----------------------------

Description: Adds an element to a Roaring bitmap.

Return type: RoaringBitmap

Example:

::

   SELECT rb_to_array(rb_added(2, rb_build('{1,3}')));
   rb_to_array
   -------------
   {1,2,3}
   (1 row)

rb_contain(roaringbitmap,int)
-----------------------------

Description: Determines whether a Roaring bitmap contains the specified element.

Return type: bool

Example:

::

   SELECT rb_contain(rb_build('{1,3}'), 2);
   rb_contain
   ------------
   f
   (1 row)

rb_containedby(int,roaringbitmap)
---------------------------------

Description: Determines whether the given element is included in a given Roaring bitmap.

Example:

::

   SELECT rb_containedby(2,rb_build('{1,3}'));
   rb_containedby
   ----------------
   f
   (1 row)

rb_contain_rb(roaringbitmap,roaringbitmap)
------------------------------------------

Description: Determines whether the first Roaring bitmap contains the second Roaring bitmap.

Return type: bool

Example:

::

   SELECT rb_contain_rb(rb_build('{1,3}'), rb_build('{2,3}'));
   rb_contain_rb
   ---------------
   f
   (1 row)

rb_containedby_rb(roaringbitmap,roaringbitmap)
----------------------------------------------

Description: Determines whether the second Roaring bitmap contains the first Roaring bitmap.

Return type: bool

Example:

::

   SELECT rb_containedby_rb(rb_build('{1,3}'), rb_build('{2,3}'));
   rb_containedby_rb
   ---------------
   f
   (1 row)

rb_remove(roaringbitmap,int)
----------------------------

Description: Removes elements from a Roaring bitmap.

Return type: RoaringBitmap

Example:

::

   SELECT rb_to_array(rb_remove(rb_build('{1,3}'),1));
   rb_to_array
   -------------
   {3}
   (1 row)

rb_clear(roaringbitmap,int,int)
-------------------------------

Description: Clears elements within a specified range from roaring bitmaps.

Return type: RoaringBitmap

Example:

::

   SELECT rb_to_array(rb_clear(rb_build('{1,2,3}'),1,2));                                                                                                                                                                                                                        rb_to_array                                                                                                                                                                                                                                                                            -------------                                                                                                                                                                                                                                                                            {2,3}                                                                                                                                                                                                                                                                                  (1 row)

rb_flip(roaringbitmap,int,int)
------------------------------

Description: Reverses elements in a specified range.

Example:

::

   SELECT rb_to_array(rb_flip(rb_build('{1,2,3,7,9}'), 1,10));
   rb_to_array
   --------------
   {4,5,6,8,10}
   (1 row)

rb_rank(roaringbitmap,int)
--------------------------

Description: Returns the cardinality of the set of values less than the specified value.

Return type: int

Example:

::

   SELECT rb_rank(rb_build('{1,10,100}'),99);
   rb_rank
   ---------
   2
   (1 row)
