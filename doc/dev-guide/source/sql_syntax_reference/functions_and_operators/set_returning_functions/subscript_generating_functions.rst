:original_name: dws_06_0337.html

.. _dws_06_0337:

Subscript Generating Functions
==============================

generate_subscripts(array anyarray, dim int)
--------------------------------------------

Description: Generates a series comprising the given array's subscripts.

Return type: setof int

Example:

::

   SELECT generate_subscripts('{NULL,1,NULL,2}'::int[], 1) AS s;
    s
   ---
    1
    2
    3
    4
   (4 rows)

generate_subscripts(array anyarray, dim int, reverse boolean)
-------------------------------------------------------------

Description: Generates a series comprising the given array's subscripts. When **reverse** is true, the series is returned in reverse order.

Return type: setof int

Example:

::

   SELECT generate_subscripts('{NULL,1,NULL,2}'::int[], 1,TRUE) AS s;
    s
   ---
    4
    3
    2
    1
   (4 rows)

**generate_subscripts** is a function that generates the set of valid subscripts for the specified dimension of a given array. Zero rows are returned for arrays that do not have the requested dimension, or for NULL arrays (but valid subscripts are returned for NULL array elements). Example:

::

   -- unnest a 2D array
   CREATE OR REPLACE FUNCTION unnest2(anyarray)
   RETURNS SETOF anyelement AS $$
   SELECT $1[i][j]
      FROM generate_subscripts($1,1) g1(i),
           generate_subscripts($1,2) g2(j);
   $$ LANGUAGE sql IMMUTABLE;

   SELECT * FROM unnest2(ARRAY[[1,2],[3,4]]);
    unnest2
   ---------
          1
          2
          3
          4
   (4 rows)
