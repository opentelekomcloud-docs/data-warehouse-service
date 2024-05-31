:original_name: dws_06_0319.html

.. _dws_06_0319:

Text Search Operators
=====================

@@
--

Description: Specifies whether the **tsvector**-typed words match the **tsquery**-typed words.

Example:

::

   SELECT to_tsvector('fat cats ate rats') @@ to_tsquery('cat & rat') AS RESULT;
    result
   --------
    t
   (1 row)


@@@
---

Description: Synonym for @@

Example:

::

   SELECT to_tsvector('fat cats ate rats') @@@ to_tsquery('cat & rat') AS RESULT;
    result
   --------
    t
   (1 row)


&&
--

Description: Performs the AND operation on two **tsquery**-typed words.

Example:

::

   SELECT 'fat | rat'::tsquery && 'cat'::tsquery AS RESULT;
             result
   ---------------------------
    ( 'fat' | 'rat' ) & 'cat'
   (1 row)


\|\|
----

Description: Performs the OR operation on two **tsquery**-typed words.

Example:

::

   SELECT 'fat | rat'::tsquery || 'cat'::tsquery AS RESULT;
             result
   ---------------------------
    ( 'fat' | 'rat' ) | 'cat'
   (1 row)
   SELECT 'a:1 b:2'::tsvector || 'c:1 d:2 b:3'::tsvector AS RESULT;
             result
   ---------------------------
    'a':1 'b':2,5 'c':3 'd':4
   (1 row)


!!
--

Description: **NOT** a **tsquery**

Example:

::

   SELECT !! 'cat'::tsquery AS RESULT;
    result
   --------
    !'cat'
   (1 row)


@>
--

Description: Specifies whether a **tsquery**-typed word contains another **tsquery**-typed word.

Example:

::

   SELECT 'cat'::tsquery @> 'cat & rat'::tsquery AS RESULT;
    result
   --------
    t
   (1 row)


<@
--

Description: Specifies whether a **tsquery**-typed word is contained in another **tsquery**-typed word.

Example:

::

   SELECT 'cat'::tsquery <@ 'cat & rat'::tsquery AS RESULT;
    result
   --------
    t
   (1 row)

In addition to the preceding operators, the ordinary B-tree comparison operators (including = and <) are defined for types **tsvector** and **tsquery**.
