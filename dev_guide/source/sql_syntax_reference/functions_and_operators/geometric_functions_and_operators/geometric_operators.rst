:original_name: dws_06_0314.html

.. _dws_06_0314:

Geometric Operators
===================

``+``
-----

Description: Translation

Example:

::

   SELECT box '((0,0),(1,1))' + point '(2.0,0)' AS RESULT;
      result
   -------------
    (3,1),(2,0)
   (1 row)


``-``
-----

Description: Translation

Example:

::

   SELECT box '((0,0),(1,1))' - point '(2.0,0)' AS RESULT;
       result
   ---------------
    (-1,1),(-2,0)
   (1 row)


``*``
-----

Description: Scaling out/rotation

Example:

::

   SELECT box '((0,0),(1,1))' * point '(2.0,0)' AS RESULT;
      result
   -------------
    (2,2),(0,0)
   (1 row)


``/``
-----

Description: Scaling in/rotation

Example:

::

   SELECT box '((0,0),(2,2))' / point '(2.0,0)' AS RESULT;
      result
   -------------
    (1,1),(0,0)
   (1 row)


``#``
-----

Description: Point or box of intersection

Example:

::

   SELECT box'((1,-1),(-1,1))' # box'((1,1),(-1,-1))' AS RESULT;
    result
   --------
   (1,1),(-1,-1)
   (1 row)


``#``
-----

Description: Number of paths or polygon vertexs

Example:

::

   SELECT # path'((1,0),(0,1),(-1,0))' AS RESULT;
    result
   --------
         3
   (1 row)


@-@
---

Description: Length or circumference

Example:

::

   SELECT @-@ path '((0,0),(1,0))' AS RESULT;
    result
   --------
         2
   (1 row)


@@
--

Description: Center of box

Example:

::

   SELECT @@ circle '((0,0),10)' AS RESULT;
    result
   --------
    (0,0)
   (1 row)


##
--

Description: Closest point to first figure on second figure.

Example:

::

   SELECT point '(0,0)' ## box '((2,0),(0,2))' AS RESULT;
    result
   --------
    (0,0)
   (1 row)


<->
---

Description: Distance between the two figures.

Example:

::

   SELECT circle '((0,0),1)' <-> circle '((5,0),1)' AS RESULT;
    result
   --------
         3
   (1 row)


&&
--

Description: Overlaps? (One point in common makes this true.)

Example:

::

   SELECT box '((0,0),(1,1))' && box '((0,0),(2,2))' AS RESULT;
    result
   --------
    t
   (1 row)


<<
--

Description: Is strictly left of (no common horizontal coordinate)?

Example:

::

   SELECT circle '((0,0),1)' << circle '((5,0),1)' AS RESULT;
    result
   --------
    t
   (1 row)


>>
--

Description: Is strictly right of (no common horizontal coordinate)?

Example:

::

   SELECT circle '((5,0),1)' >> circle '((0,0),1)' AS RESULT;
    result
   --------
    t
   (1 row)


&<
--

Description: Does not extend to the right of?

Example:

::

   SELECT box '((0,0),(1,1))' &< box '((0,0),(2,2))' AS RESULT;
    result
   --------
    t
   (1 row)


&>
--

Description: Does not extend to the left of?

Example:

::

   SELECT box '((0,0),(3,3))' &> box '((0,0),(2,2))' AS RESULT;
    result
   --------
    t
   (1 row)


<<\|
----

Description: Is strictly below (no common horizontal coordinate)?

Example:

::

   SELECT box '((0,0),(3,3))' <<| box '((3,4),(5,5))' AS RESULT;
    result
   --------
    t
   (1 row)


\|>>
----

Description: Is strictly above (no common horizontal coordinate)?

Example:

::

   SELECT box '((3,4),(5,5))' |>> box '((0,0),(3,3))' AS RESULT;
    result
   --------
    t
   (1 row)


&<\|
----

Description: Does not extend above?

Example:

::

   SELECT box '((0,0),(1,1))' &<| box '((0,0),(2,2))' AS RESULT;
    result
   --------
    t
   (1 row)


\|&>
----

Description: Does not extend below?

Example:

::

   SELECT box '((0,0),(3,3))' |&> box '((0,0),(2,2))' AS RESULT;
    result
   --------
    t
   (1 row)


<^
--

Description: Is below (allows touching)?

Example:

::

   SELECT box '((0,0),(-3,-3))' <^ box '((0,0),(2,2))' AS RESULT;
    result
   --------
    t
   (1 row)


>^
--

Description: Is above (allows touching)?

Example:

::

   SELECT box '((0,0),(2,2))' >^ box '((0,0),(-3,-3))'  AS RESULT;
    result
   --------
    t
   (1 row)


?#
--

Description: Intersect?

Example:

::

   SELECT lseg '((-1,0),(1,0))' ?# box '((-2,-2),(2,2))' AS RESULT;
    result
   --------
    t
   (1 row)


?-
--

Description: Is horizontal?

Example:

::

   SELECT ?- lseg '((-1,0),(1,0))' AS RESULT;
    result
   --------
    t
   (1 row)


?-
--

Description: Are horizontally aligned?

Example:

::

   SELECT point '(1,0)' ?- point '(0,0)' AS RESULT;
    result
   --------
    t
   (1 row)


?\|
---

Description: Is vertical?

Example:

::

   SELECT ?| lseg '((-1,0),(1,0))' AS RESULT;
    result
   --------
    f
   (1 row)


?\|
---

Description: Are vertically aligned?

Example:

::

   SELECT point '(0,1)' ?| point '(0,0)' AS RESULT;
    result
   --------
    t
   (1 row)


?-\|
----

Description: Are perpendicular?

Example:

::

   SELECT lseg '((0,0),(0,1))' ?-| lseg '((0,0),(1,0))' AS RESULT;
    result
   --------
    t
   (1 row)


?|\|
----

Description: Are parallel?

Example:

::

   SELECT lseg '((-1,0),(1,0))' ?|| lseg '((-1,2),(1,2))' AS RESULT;
    result
   --------
    t
   (1 row)


@>
--

Description: Contains?

Example:

::

   SELECT circle '((0,0),2)' @> point '(1,1)' AS RESULT;
    result
   --------
    t
   (1 row)


<@
--

Description: Contained in or on?

Example:

::

   SELECT point '(1,1)' <@ circle '((0,0),2)' AS RESULT;
    result
   --------
    t
   (1 row)


~=
--

Description: Same as?

Example:

::

   SELECT polygon '((0,0),(1,1))' ~= polygon '((1,1),(0,0))' AS RESULT;
    result
   --------
    t
   (1 row)
