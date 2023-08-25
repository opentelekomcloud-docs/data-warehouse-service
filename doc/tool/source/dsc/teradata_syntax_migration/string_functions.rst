:original_name: dws_mt_0101.html

.. _dws_mt_0101:

String Functions
================

CHAR Function
-------------

**Input: CHAR**

::

   CHAR( expression1 )

**Output**

::

   LENGTH( expression1 )

CHARACTERS Function
-------------------

**Input: CHARACTERS**

::

   CHARACTERS( expression1 )

**Output**

::

   LENGTH( expression1 )

INDEX
-----

**Input: INDEX**

::

   SELECT INDEX(expr1/string, substring)
     FROM tab1
    WHERE … ;

**Output**

::

    SELECT INSTR(expr1/string, substring)
     FROM tab1
    WHERE … ;

STRREPLACE
----------

**Input: STRREPLACE**

::

   SELECT STRREPLACE(c2, '.', '')
     FROM tab1
    WHERE ...;

**Output**

::

   SELECT REPLACE(c2, '.', '')
     FROM tab1
    WHERE ...;

OREPLACE
--------

**Input: OREPLACE**

::

   SELECT OREPLACE (c2, '.', '')
     FROM tab1
    WHERE … ;

**Output**

::

    SELECT REPLACE(c2, '.', '')
     FROM tab1
    WHERE … ;
