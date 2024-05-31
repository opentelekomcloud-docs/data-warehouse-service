:original_name: dws_06_0015.html

.. _dws_06_0015:

Geometric Types
===============

:ref:`Table 1 <en-us_topic_0000001188429056__t6242ed1ede044daab7e8e566a9568667>` lists the geometric types that can be used in GaussDB(DWS). The most fundamental type, the point, forms the basis for all of the other types.

.. _en-us_topic_0000001188429056__t6242ed1ede044daab7e8e566a9568667:

.. table:: **Table 1** Geometric Type

   +---------+---------------+----------------------------------+-------------------------------------+
   | Name    | Storage Space | Description                      | Representation                      |
   +=========+===============+==================================+=====================================+
   | point   | 16 bytes      | Point on a plane                 | (x,y)                               |
   +---------+---------------+----------------------------------+-------------------------------------+
   | lseg    | 32 bytes      | Finite line segment              | ((x1,y1),(x2,y2))                   |
   +---------+---------------+----------------------------------+-------------------------------------+
   | box     | 32 bytes      | Rectangular Box                  | ((x1,y1),(x2,y2))                   |
   +---------+---------------+----------------------------------+-------------------------------------+
   | path    | 16+16n bytes  | Closed path (similar to polygon) | ((x1,y1),...)                       |
   +---------+---------------+----------------------------------+-------------------------------------+
   | path    | 16+16n bytes  | Open path                        | [(x1,y1),...]                       |
   +---------+---------------+----------------------------------+-------------------------------------+
   | polygon | 40+16n bytes  | Polygon (similar to closed path) | ((x1,y1),...)                       |
   +---------+---------------+----------------------------------+-------------------------------------+
   | circle  | 24 bytes      | Circle                           | <(x,y),r> (center point and radius) |
   +---------+---------------+----------------------------------+-------------------------------------+

A rich set of functions and operators is available in GaussDB(DWS) to perform various geometric operations, such as scaling, translation, rotation, and determining intersections. For details, see :ref:`Geometric Functions and Operators <dws_06_0037>`.

**Points**
----------

Points are the fundamental two-dimensional building block for geometric types. Values of the **point** type are specified using either of the following syntaxes:

.. code-block::

   ( x , y )
   x , y

where x and y are the respective coordinates, as floating-point numbers.

Points are output using the first syntax.

**Line Segments**
-----------------

Line segments (**lseg**) are represented by pairs of points. Values of the **lseg** type are specified using any of the following syntaxes:

.. code-block::

   [ ( x1 , y1 ) , ( x2 , y2 ) ]
   ( ( x1 , y1 ) , ( x2 , y2 ) )
   ( x1 , y1 ) , ( x2 , y2 )
   x1 , y1   ,   x2 , y2

where (x1,y1) and (x2,y2) are the end points of the line segment.

Line segments are output using the first syntax.

Rectangular Box
---------------

Boxes are represented by pairs of points that are opposite corners of the box. Values of the **box** type are specified using any of the following syntaxes:

.. code-block::

   ( ( x1 , y1 ) , ( x2 , y2 ) )
   ( x1 , y1 ) , ( x2 , y2 )
   x1 , y1   ,   x2 , y2

where (x1,y1) and (x2,y2) are any two opposite corners of the box.

Boxes are output using the second syntax.

Any two opposite corners can be supplied on input, but in this order, the values will be reordered as needed to store the upper right and lower left corners.

Path
----

Paths are represented by lists of connected points. Paths can be open, where the first and last points in the list are considered not connected, or closed, where the first and last points are considered connected.

Values of the **path** type are specified using any of the following syntaxes:

.. code-block::

   [ ( x1 , y1 ) , ... , ( xn , yn ) ]
   ( ( x1 , y1 ) , ... , ( xn , yn ) )
   ( x1 , y1 ) , ... , ( xn , yn )
   ( x1 , y1   , ... ,   xn , yn )
   x1 , y1   , ... ,   xn , yn

where the points are the end points of the line segments comprising the path. Square brackets ([]) indicate an open path, while parentheses (()) indicate a closed path. When the outermost parentheses are omitted, as in the third through fifth syntaxes, a closed path is assumed.

Paths are output using the first or second syntax.

**Polygons**
------------

Polygons are represented by lists of points (the vertexes of the polygon). Polygons are very similar to closed paths, but are stored differently and have their own set of support functions.

Values of the **polygon** type are specified using any of the following syntaxes:

.. code-block::

   ( ( x1 , y1 ) , ... , ( xn , yn ) )
   ( x1 , y1 ) , ... , ( xn , yn )
   ( x1 , y1   , ... ,   xn , yn )
   x1 , y1   , ... ,   xn , yn

where the points are the end points of the line segments comprising the boundary of the polygon.

Polygons are output using the first syntax.

Circle
------

Circles are represented by a center point and radius. Values of the **circle** type are specified using any of the following syntaxes:

.. code-block::

   < ( x , y ) , r >
   ( ( x , y ) , r )
   ( x , y ) , r
   x , y   , r

where **(x,y)** is the center point and **r** is the radius of the circle.

Circles are output using the first syntax.
