:original_name: dws_06_0316.html

.. _dws_06_0316:

Geometric Type Conversion Functions
===================================

box(circle)
-----------

Description: Circle to box

Return type: box

Example:

::

   SELECT box(circle '((0,0),2.0)') AS RESULT;
                                     result
   ---------------------------------------------------------------------------
    (1.41421356237309,1.41421356237309),(-1.41421356237309,-1.41421356237309)
   (1 row)

box(point, point)
-----------------

Description: Points to box

Return type: box

Example:

::

   SELECT box(point '(0,0)', point '(1,1)') AS RESULT;
      result
   -------------
    (1,1),(0,0)
   (1 row)

box(polygon)
------------

Description: Polygon to box

Return type: box

Example:

::

   SELECT box(polygon '((0,0),(1,1),(2,0))') AS RESULT;
      result
   -------------
    (2,1),(0,0)
   (1 row)

circle(box)
-----------

Description: Box to circle

Return type: circle

Example:

::

   SELECT circle(box '((0,0),(1,1))') AS RESULT;
               result
   -------------------------------
    <(0.5,0.5),0.707106781186548>
   (1 row)

circle(point, double precision)
-------------------------------

Description: Center and radius to circle

Return type: circle

Example:

::

   SELECT circle(point '(0,0)', 2.0) AS RESULT;
     result
   -----------
    <(0,0),2>
   (1 row)

circle(polygon)
---------------

Description: Polygon to circle

Return type: circle

Example:

::

   SELECT circle(polygon '((0,0),(1,1),(2,0))') AS RESULT;
                     result
   -------------------------------------------
    <(1,0.333333333333333),0.924950591148529>
   (1 row)

lseg(box)
---------

Description: Box diagonal to line segment

Return type: lseg

Example:

::

   SELECT lseg(box '((-1,0),(1,0))') AS RESULT;
        result
   ----------------
    [(1,0),(-1,0)]
   (1 row)

lseg(point, point)
------------------

Description: Points to line segment

Return type: lseg

Example:

::

   SELECT lseg(point '(-1,0)', point '(1,0)') AS RESULT;
        result
   ----------------
    [(-1,0),(1,0)]
   (1 row)

path(polygon)
-------------

Description: Polygon to path

Return type: path

Example:

::

   SELECT path(polygon '((0,0),(1,1),(2,0))') AS RESULT;
          result
   ---------------------
    ((0,0),(1,1),(2,0))
   (1 row)

point(double precision, double precision)
-----------------------------------------

Description: Points

Return type: point

Example:

::

   SELECT point(23.4, -44.5) AS RESULT;
       result
   --------------
    (23.4,-44.5)
   (1 row)

point(box)
----------

Description: Center of box

Return type: point

Example:

::

   SELECT point(box '((-1,0),(1,0))') AS RESULT;
    result
   --------
    (0,0)
   (1 row)

point(circle)
-------------

Description: Center of circle

Return type: point

Example:

::

   SELECT point(circle '((0,0),2.0)') AS RESULT;
    result
   --------
    (0,0)
   (1 row)

point(lseg)
-----------

Description: Center of line segment

Return type: point

Example:

::

   SELECT point(lseg '((-1,0),(1,0))') AS RESULT;
    result
   --------
    (0,0)
   (1 row)

point(polygon)
--------------

Description: Center of polygon

Return type: point

Example:

::

   SELECT point(polygon '((0,0),(1,1),(2,0))') AS RESULT;
           result
   -----------------------
    (1,0.333333333333333)
   (1 row)

polygon(box)
------------

Description: Box to 4-point polygon

Return type: polygon

Example:

::

   SELECT polygon(box '((0,0),(1,1))') AS RESULT;
             result
   ---------------------------
    ((0,0),(0,1),(1,1),(1,0))
   (1 row)

polygon(circle)
---------------

Description: Circle to 12-point polygon

Return type: polygon

Example:

::

   SELECT polygon(circle '((0,0),2.0)') AS RESULT;
                                                                                                                                                   result

   -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
    ((-2,0),(-1.73205080756888,1),(-1,1.73205080756888),(-1.22464679914735e-16,2),(1,1.73205080756888),(1.73205080756888,1),(2,2.44929359829471e-16),(1.73205080756888,-0.999999999999999),(1,-1.73205080756888),(3.67394039744206e-16,-2),(-0.999999999999999,-1.73205080756888),(-1.73205080756888,-1))
   (1 row)

polygon(npts, circle)
---------------------

Description: Circle to **npts**-point polygon

Return type: polygon

Example:

::

   SELECT polygon(12, circle '((0,0),2.0)') AS RESULT;
                                                                                                                                                   result

   -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
    ((-2,0),(-1.73205080756888,1),(-1,1.73205080756888),(-1.22464679914735e-16,2),(1,1.73205080756888),(1.73205080756888,1),(2,2.44929359829471e-16),(1.73205080756888,-0.999999999999999),(1,-1.73205080756888),(3.67394039744206e-16,-2),(-0.999999999999999,-1.73205080756888),(-1.73205080756888,-1))
   (1 row)

polygon(path)
-------------

Description: Path to polygon

Return type: polygon

Example:

::

   SELECT polygon(path '((0,0),(1,1),(2,0))') AS RESULT;
          result
   ---------------------
    ((0,0),(1,1),(2,0))
   (1 row)
