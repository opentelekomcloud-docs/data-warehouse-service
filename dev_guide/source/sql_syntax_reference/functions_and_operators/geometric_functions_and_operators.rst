:original_name: dws_06_0037.html

.. _dws_06_0037:

Geometric Functions and Operators
=================================

Geometric Operators
-------------------

-  +

   Description: Translation

   For example:

   ::

      SELECT box '((0,0),(1,1))' + point '(2.0,0)' AS RESULT;
         result
      -------------
       (3,1),(2,0)
      (1 row)

-  ``-``

   Description: Translation

   For example:

   ::

      SELECT box '((0,0),(1,1))' - point '(2.0,0)' AS RESULT;
          result
      ---------------
       (-1,1),(-2,0)
      (1 row)

-  \*

   Description: Scaling out/rotation

   For example:

   ::

      SELECT box '((0,0),(1,1))' * point '(2.0,0)' AS RESULT;
         result
      -------------
       (2,2),(0,0)
      (1 row)

-  /

   Description: Scaling in/rotation

   For example:

   ::

      SELECT box '((0,0),(2,2))' / point '(2.0,0)' AS RESULT;
         result
      -------------
       (1,1),(0,0)
      (1 row)

-  #

   Description: Point or box of intersection

   For example:

   ::

      SELECT box'((1,-1),(-1,1))' # box'((1,1),(-1,-1))' AS RESULT;
       result
      --------
      (1,1),(-1,-1)
      (1 row)

-  #

   Description: Number of paths or polygon vertexs

   For example:

   ::

      SELECT # path'((1,0),(0,1),(-1,0))' AS RESULT;
       result
      --------
            3
      (1 row)

-  @-@

   Description: Length or circumference

   For example:

   ::

      SELECT @-@ path '((0,0),(1,0))' AS RESULT;
       result
      --------
            2
      (1 row)

-  @@

   Description: Center of box

   For example:

   ::

      SELECT @@ circle '((0,0),10)' AS RESULT;
       result
      --------
       (0,0)
      (1 row)

-  ##

   Description: Closest point to first figure on second figure.

   For example:

   ::

      SELECT point '(0,0)' ## box '((2,0),(0,2))' AS RESULT;
       result
      --------
       (0,0)
      (1 row)

-  <->

   Description: Distance between the two figures.

   For example:

   ::

      SELECT circle '((0,0),1)' <-> circle '((5,0),1)' AS RESULT;
       result
      --------
            3
      (1 row)

-  &&

   Description: Overlaps? (One point in common makes this true.)

   For example:

   ::

      SELECT box '((0,0),(1,1))' && box '((0,0),(2,2))' AS RESULT;
       result
      --------
       t
      (1 row)

-  <<

   Description: Is strictly left of (no common horizontal coordinate)?

   For example:

   ::

      SELECT circle '((0,0),1)' << circle '((5,0),1)' AS RESULT;
       result
      --------
       t
      (1 row)

-  >>

   Description: Is strictly right of (no common horizontal coordinate)?

   For example:

   ::

      SELECT circle '((5,0),1)' >> circle '((0,0),1)' AS RESULT;
       result
      --------
       t
      (1 row)

-  &<

   Description: Does not extend to the right of?

   For example:

   ::

      SELECT box '((0,0),(1,1))' &< box '((0,0),(2,2))' AS RESULT;
       result
      --------
       t
      (1 row)

-  &>

   Description: Does not extend to the left of?

   For example:

   ::

      SELECT box '((0,0),(3,3))' &> box '((0,0),(2,2))' AS RESULT;
       result
      --------
       t
      (1 row)

-  <<\|

   Description: Is strictly below (no common horizontal coordinate)?

   For example:

   ::

      SELECT box '((0,0),(3,3))' <<| box '((3,4),(5,5))' AS RESULT;
       result
      --------
       t
      (1 row)

-  \|>>

   Description: Is strictly above (no common horizontal coordinate)?

   For example:

   ::

      SELECT box '((3,4),(5,5))' |>> box '((0,0),(3,3))' AS RESULT;
       result
      --------
       t
      (1 row)

-  &<\|

   Description: Does not extend above?

   For example:

   ::

      SELECT box '((0,0),(1,1))' &<| box '((0,0),(2,2))' AS RESULT;
       result
      --------
       t
      (1 row)

-  \|&>

   Description: Does not extend below?

   For example:

   ::

      SELECT box '((0,0),(3,3))' |&> box '((0,0),(2,2))' AS RESULT;
       result
      --------
       t
      (1 row)

-  <^

   Description: Is below (allows touching)?

   For example:

   ::

      SELECT box '((0,0),(-3,-3))' <^ box '((0,0),(2,2))' AS RESULT;
       result
      --------
       t
      (1 row)

-  >^

   Description: Is above (allows touching)?

   For example:

   ::

      SELECT box '((0,0),(2,2))' >^ box '((0,0),(-3,-3))'  AS RESULT;
       result
      --------
       t
      (1 row)

-  ?#

   Description: Intersect?

   For example:

   ::

      SELECT lseg '((-1,0),(1,0))' ?# box '((-2,-2),(2,2))' AS RESULT;
       result
      --------
       t
      (1 row)

-  ?-

   Description: Is horizontal?

   For example:

   ::

      SELECT ?- lseg '((-1,0),(1,0))' AS RESULT;
       result
      --------
       t
      (1 row)

-  ?-

   Description: Are horizontally aligned?

   For example:

   ::

      SELECT point '(1,0)' ?- point '(0,0)' AS RESULT;
       result
      --------
       t
      (1 row)

-  ?\|

   Description: Is vertical?

   For example:

   ::

      SELECT ?| lseg '((-1,0),(1,0))' AS RESULT;
       result
      --------
       f
      (1 row)

-  ?\|

   Description: Are vertically aligned?

   For example:

   ::

      SELECT point '(0,1)' ?| point '(0,0)' AS RESULT;
       result
      --------
       t
      (1 row)

-  ?-\|

   Description: Are perpendicular?

   For example:

   ::

      SELECT lseg '((0,0),(0,1))' ?-| lseg '((0,0),(1,0))' AS RESULT;
       result
      --------
       t
      (1 row)

-  ?|\|

   Description: Are parallel?

   For example:

   ::

      SELECT lseg '((-1,0),(1,0))' ?|| lseg '((-1,2),(1,2))' AS RESULT;
       result
      --------
       t
      (1 row)

-  @>

   Description: Contains?

   For example:

   ::

      SELECT circle '((0,0),2)' @> point '(1,1)' AS RESULT;
       result
      --------
       t
      (1 row)

-  <@

   Description: Contained in or on?

   For example:

   ::

      SELECT point '(1,1)' <@ circle '((0,0),2)' AS RESULT;
       result
      --------
       t
      (1 row)

-  ~=

   Description: Same as?

   For example:

   ::

      SELECT polygon '((0,0),(1,1))' ~= polygon '((1,1),(0,0))' AS RESULT;
       result
      --------
       t
      (1 row)

Geometric Functions
-------------------

-  area(object)

   Description: Area calculation

   Return type: double precision

   For example:

   ::

      SELECT area(box '((0,0),(1,1))') AS RESULT;
       result
      --------
            1
      (1 row)

-  center(object)

   Description: Figure center calculation

   Return type: point

   For example:

   ::

      SELECT center(box '((0,0),(1,2))') AS RESULT;
       result
      ---------
       (0.5,1)
      (1 row)

-  diameter(circle)

   Description: Circle diameter calculation

   Return type: double precision

   For example:

   ::

      SELECT diameter(circle '((0,0),2.0)') AS RESULT;
       result
      --------
            4
      (1 row)

-  height(box)

   Description: Vertical size of box

   Return type: double precision

   For example:

   ::

      SELECT height(box '((0,0),(1,1))') AS RESULT;
       result
      --------
            1
      (1 row)

-  isclosed(path)

   Description: A closed path?

   Return type: boolean

   For example:

   ::

      SELECT isclosed(path '((0,0),(1,1),(2,0))') AS RESULT;
       result
      --------
       t
      (1 row)

-  isopen(path)

   Description: An open path?

   Return type: boolean

   For example:

   ::

      SELECT isopen(path '[(0,0),(1,1),(2,0)]') AS RESULT;
       result
      --------
       t
      (1 row)

-  length(object)

   Description: Length calculation

   Return type: double precision

   For example:

   ::

      SELECT length(path '((-1,0),(1,0))') AS RESULT;
       result
      --------
            4
      (1 row)

-  npoints(path)

   Description: Number of points in path

   Return type: int

   For example:

   ::

      SELECT npoints(path '[(0,0),(1,1),(2,0)]') AS RESULT;
       result
      --------
            3
      (1 row)

-  npoints(polygon)

   Description: Number of points in polygon

   Return type: int

   For example:

   ::

      SELECT npoints(polygon '((1,1),(0,0))') AS RESULT;
       result
      --------
            2
      (1 row)

-  pclose(path)

   Description: Converts path to closed.

   Return type: path

   For example:

   ::

      SELECT pclose(path '[(0,0),(1,1),(2,0)]') AS RESULT;
             result
      ---------------------
       ((0,0),(1,1),(2,0))
      (1 row)

-  popen(path)

   Description: Converts path to open.

   Return type: path

   For example:

   ::

      SELECT popen(path '((0,0),(1,1),(2,0))') AS RESULT;
             result
      ---------------------
       [(0,0),(1,1),(2,0)]
      (1 row)

-  radius(circle)

   Description: Circle diameter calculation

   Return type: double precision

   For example:

   ::

      SELECT radius(circle '((0,0),2.0)') AS RESULT;
       result
      --------
            2
      (1 row)

-  width(box)

   Description: Horizontal size of box

   Return type: double precision

   For example:

   ::

      SELECT width(box '((0,0),(1,1))') AS RESULT;
       result
      --------
            1
      (1 row)

Geometric Type Conversion Functions
-----------------------------------

-  box(circle)

   Description: Circle to box

   Return type: box

   For example:

   ::

      SELECT box(circle '((0,0),2.0)') AS RESULT;
                                        result
      ---------------------------------------------------------------------------
       (1.41421356237309,1.41421356237309),(-1.41421356237309,-1.41421356237309)
      (1 row)

-  box(point, point)

   Description: Points to box

   Return type: box

   For example:

   ::

      SELECT box(point '(0,0)', point '(1,1)') AS RESULT;
         result
      -------------
       (1,1),(0,0)
      (1 row)

-  box(polygon)

   Description: Polygon to box

   Return type: box

   For example:

   ::

      SELECT box(polygon '((0,0),(1,1),(2,0))') AS RESULT;
         result
      -------------
       (2,1),(0,0)
      (1 row)

-  circle(box)

   Description: Box to circle

   Return type: circle

   For example:

   ::

      SELECT circle(box '((0,0),(1,1))') AS RESULT;
                  result
      -------------------------------
       <(0.5,0.5),0.707106781186548>
      (1 row)

-  circle(point, double precision)

   Description: Center and radius to circle

   Return type: circle

   For example:

   ::

      SELECT circle(point '(0,0)', 2.0) AS RESULT;
        result
      -----------
       <(0,0),2>
      (1 row)

-  circle(polygon)

   Description: Polygon to circle

   Return type: circle

   For example:

   ::

      SELECT circle(polygon '((0,0),(1,1),(2,0))') AS RESULT;
                        result
      -------------------------------------------
       <(1,0.333333333333333),0.924950591148529>
      (1 row)

-  lseg(box)

   Description: Box diagonal to line segment

   Return type: lseg

   For example:

   ::

      SELECT lseg(box '((-1,0),(1,0))') AS RESULT;
           result
      ----------------
       [(1,0),(-1,0)]
      (1 row)

-  lseg(point, point)

   Description: Points to line segment

   Return type: lseg

   For example:

   ::

      SELECT lseg(point '(-1,0)', point '(1,0)') AS RESULT;
           result
      ----------------
       [(-1,0),(1,0)]
      (1 row)

-  path(polygon)

   Description: Polygon to path

   Return type: path

   For example:

   ::

      SELECT path(polygon '((0,0),(1,1),(2,0))') AS RESULT;
             result
      ---------------------
       ((0,0),(1,1),(2,0))
      (1 row)

-  point(double precision, double precision)

   Description: Points

   Return type: point

   For example:

   ::

      SELECT point(23.4, -44.5) AS RESULT;
          result
      --------------
       (23.4,-44.5)
      (1 row)

-  point(box)

   Description: Center of box

   Return type: point

   For example:

   ::

      SELECT point(box '((-1,0),(1,0))') AS RESULT;
       result
      --------
       (0,0)
      (1 row)

-  point(circle)

   Description: Center of circle

   Return type: point

   For example:

   ::

      SELECT point(circle '((0,0),2.0)') AS RESULT;
       result
      --------
       (0,0)
      (1 row)

-  point(lseg)

   Description: Center of line segment

   Return type: point

   For example:

   ::

      SELECT point(lseg '((-1,0),(1,0))') AS RESULT;
       result
      --------
       (0,0)
      (1 row)

-  point(polygon)

   Description: Center of polygon

   Return type: point

   For example:

   ::

      SELECT point(polygon '((0,0),(1,1),(2,0))') AS RESULT;
              result
      -----------------------
       (1,0.333333333333333)
      (1 row)

-  polygon(box)

   Description: Box to 4-point polygon

   Return type: polygon

   For example:

   ::

      SELECT polygon(box '((0,0),(1,1))') AS RESULT;
                result
      ---------------------------
       ((0,0),(0,1),(1,1),(1,0))
      (1 row)

-  polygon(circle)

   Description: Circle to 12-point polygon

   Return type: polygon

   For example:

   ::

      SELECT polygon(circle '((0,0),2.0)') AS RESULT;
                                                                                                                                                      result

      -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
       ((-2,0),(-1.73205080756888,1),(-1,1.73205080756888),(-1.22464679914735e-16,2),(1,1.73205080756888),(1.73205080756888,1),(2,2.44929359829471e-16),(1.73205080756888,-0.999999999999999),(1,-1.73205080756888),(3.67394039744206e-16,-2),(-0.999999999999999,-1.73205080756888),(-1.73205080756888,-1))
      (1 row)

-  polygon(npts, circle)

   Description: Circle to **npts**-point polygon

   Return type: polygon

   For example:

   ::

      SELECT polygon(12, circle '((0,0),2.0)') AS RESULT;
                                                                                                                                                      result

      -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
       ((-2,0),(-1.73205080756888,1),(-1,1.73205080756888),(-1.22464679914735e-16,2),(1,1.73205080756888),(1.73205080756888,1),(2,2.44929359829471e-16),(1.73205080756888,-0.999999999999999),(1,-1.73205080756888),(3.67394039744206e-16,-2),(-0.999999999999999,-1.73205080756888),(-1.73205080756888,-1))
      (1 row)

-  polygon(path)

   Description: Path to polygon

   Return type: polygon

   For example:

   ::

      SELECT polygon(path '((0,0),(1,1),(2,0))') AS RESULT;
             result
      ---------------------
       ((0,0),(1,1),(2,0))
      (1 row)
