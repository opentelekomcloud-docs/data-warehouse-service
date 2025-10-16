:original_name: dws_06_0315.html

.. _dws_06_0315:

Geometric Functions
===================

area(object)
------------

Description: Area calculation

Return type: double precision

Example:

::

   SELECT area(box '((0,0),(1,1))') AS RESULT;
    result
   --------
         1
   (1 row)

center(object)
--------------

Description: Figure center calculation

Return type: point

Example:

::

   SELECT center(box '((0,0),(1,2))') AS RESULT;
    result
   ---------
    (0.5,1)
   (1 row)

diameter(circle)
----------------

Description: Circle diameter calculation

Return type: double precision

Example:

::

   SELECT diameter(circle '((0,0),2.0)') AS RESULT;
    result
   --------
         4
   (1 row)

height(box)
-----------

Description: Vertical size of box

Return type: double precision

Example:

::

   SELECT height(box '((0,0),(1,1))') AS RESULT;
    result
   --------
         1
   (1 row)

isclosed(path)
--------------

Description: A closed path?

Return type: boolean

Example:

::

   SELECT isclosed(path '((0,0),(1,1),(2,0))') AS RESULT;
    result
   --------
    t
   (1 row)

isopen(path)
------------

Description: An open path?

Return type: boolean

Example:

::

   SELECT isopen(path '[(0,0),(1,1),(2,0)]') AS RESULT;
    result
   --------
    t
   (1 row)

length(object)
--------------

Description: Length calculation

Return type: double precision

Example:

::

   SELECT length(path '((-1,0),(1,0))') AS RESULT;
    result
   --------
         4
   (1 row)

npoints(path)
-------------

Description: Number of points in path

Return type: int

Example:

::

   SELECT npoints(path '[(0,0),(1,1),(2,0)]') AS RESULT;
    result
   --------
         3
   (1 row)

npoints(polygon)
----------------

Description: Number of points in polygon

Return type: int

Example:

::

   SELECT npoints(polygon '((1,1),(0,0))') AS RESULT;
    result
   --------
         2
   (1 row)

pclose(path)
------------

Description: Converts path to closed.

Return type: path

Example:

::

   SELECT pclose(path '[(0,0),(1,1),(2,0)]') AS RESULT;
          result
   ---------------------
    ((0,0),(1,1),(2,0))
   (1 row)

popen(path)
-----------

Description: Converts path to open.

Return type: path

Example:

::

   SELECT popen(path '((0,0),(1,1),(2,0))') AS RESULT;
          result
   ---------------------
    [(0,0),(1,1),(2,0)]
   (1 row)

radius(circle)
--------------

Description: Circle diameter calculation

Return type: double precision

Example:

::

   SELECT radius(circle '((0,0),2.0)') AS RESULT;
    result
   --------
         2
   (1 row)

width(box)
----------

Description: Horizontal size of box

Return type: double precision

Example:

::

   SELECT width(box '((0,0),(1,1))') AS RESULT;
    result
   --------
         1
   (1 row)
