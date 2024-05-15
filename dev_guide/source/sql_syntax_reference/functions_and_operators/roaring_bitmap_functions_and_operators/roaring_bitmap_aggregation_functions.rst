:original_name: dws_06_0324.html

.. _dws_06_0324:

Roaring Bitmap Aggregation Functions
====================================

Since 8.1.3, GaussDB(DWS) supports efficient bitmap processing functions and operators, which can be used in user profiling and precision marketing, greatly improving query performance.

rb_build_agg(int)
-----------------

Description: Aggregates int values in a group into a RoaringBitmap value.

Return type: RoaringBitmap

Example:

::

   CREATE TABLE t1 (a int ,b int);
   NOTICE:  The 'DISTRIBUTE BY' clause is not specified. Using round-robin as the distribution mode by default.
   HINT:  Please use 'DISTRIBUTE BY' clause to specify suitable data distribution column.
   CREATE TABLE
   INSERT INTO t1 SELECT generate_series(1,10),generate_series(1,20,2);
   INSERT 0 10

::

   SELECT rb_iterate(rb_build_agg(b)) FROM t1;
   rb_iterate
   ------------
   1
   3
   5
   7
   9
   11
   13
   15
   17
   19
   (10 rows)

rb_and_agg(roaringbitmap)
-------------------------

Description: Aggregates data of the Roaring bitmaps in a group into a Roaring bitmap set based on the INTERSECT operation.

Example:

::

   CREATE TABLE r1(a int ,b roaringbitmap);
   INSERT INTO r1 SELECT a, rb_build_agg(b) FROM t1 GROUP BY a;
   INSERT INTO t1 SELECT generate_series(1,10),generate_series(1,20,4);
   INSERT INTO r1 SELECT a, rb_build_agg(b) FROM t1 GROUP BY a;
   SELECT a, rb_to_array(rb_and_agg(b)) FROM r1 GROUP BY a ORDER BY a;
    a  | rb_to_array
   ----+-------------
    1  | {1}
    2  | {3}
    3  | {5}
    4  | {7}
    5  | {9}
    6  | {11}
    7  | {13}
    8  | {15}
    9  | {17}
   10  | {19}
   (10 rows)

rb_or_agg(roaringbitmap)
------------------------

Description: Combines Roaring bitmaps in a group into one Roaring bitmap based on the UNION logic.

Example:

::

   SELECT a, rb_to_array(rb_or_agg(b)) FROM r1 GROUP BY a ORDER BY a;
    a  | rb_to_array
   ----+-------------
    1  | {1}
    2  | {3,5}
    3  | {5,9}
    4  | {7,13}
    5  | {9,17}
    6  | {1,11}
    7  | {5,13}
    8  | {9,15}
    9  | {13,17}
    10 | {17,19}
   (10 rows)

rb_xor_agg(roaringbitmap)
-------------------------

Description: Combines Roaring bitmaps in a group into one Roaring bitmap based on the XOR logic.

Example:

::

   SELECT a, rb_to_array(rb_xor_agg(b)) FROM r1 GROUP BY a ORDER BY a;
   a   | rb_to_array
   ----+-------------
    1  | {}
    2  | {5}
    3  | {9}
    4  | {13}
    5  | {17}
    6  | {1}
    7  | {5}
    8  | {9}
    9  | {13}
    10 | {17}
   (10 rows)

rb_and_cardinality_agg(roaringbitmap)
-------------------------------------

Description: Cardinality of Roaring bitmaps in a group calculated based on INTERSECT.

Return type: int

Example:

::

   SELECT a, rb_and_cardinality_agg(b) FROM r1 GROUP BY a ORDER BY 1;
    a  | rb_and_cardinality_agg
   ----+------------------------
    1  |                      1
    2  |                      1
    3  |                      1
    4  |                      1
    5  |                      1
    6  |                      1
    7  |                      1
    8  |                      1
    9  |                      1
    10 |                      1
   (10 rows)

rb_or_cardinality_agg(roaringbitmap)
------------------------------------

Description: Cardinality of Roaring bitmaps in a group calculated based on UNION.

Example:

::

   SELECT a, rb_or_cardinality_agg(b) FROM r1 GROUP BY a ORDER BY 1;
    a  | rb_or_cardinality_agg
   ----+-----------------------
    1  |                     1
    2  |                     2
    3  |                     2
    4  |                     2
    5  |                     2
    6  |                     2
    7  |                     2
    8  |                     2
    9  |                     2
    10 |                     2
   (10 rows)

rb_xor_cardinality_agg(roaringbitmap)
-------------------------------------

Description: Cardinality of Roaring bitmaps in a group calculated based on XOR.

Example:

::

   SELECT a, rb_xor_cardinality_agg(b) FROM r1 GROUP BY a ORDER BY 1;
    a  | rb_xor_cardinality_agg
   ----+------------------------
    1  |                      0
    2  |                      1
    3  |                      1
    4  |                      1
    5  |                      1
    6  |                      1
    7  |                      1
    8  |                      1
    9  |                      1
    10 |                      1
   (10 rows)
