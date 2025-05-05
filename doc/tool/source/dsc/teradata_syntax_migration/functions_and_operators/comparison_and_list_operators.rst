:original_name: dws_16_0047.html

.. _dws_16_0047:

.. _en-us_topic_0000001860318805:

Comparison and List Operators
=============================

The following comparison and list operators are supported:

.. note::

   The comparison operators LT, LE, GT, GE, EQ, and NE must not be used as TABLE alias or COLUMN alias.

.. _en-us_topic_0000001860318805__en-us_topic_0000001384071352_section6417514422:

^= and GT
---------

**Input: Comparison operations (^= and GT)**

::

   SELECT t1.c1, t2.c2
     FROM tab1 t1, tab2 t2
    WHERE t1.c3 ^= t1.c3
      AND t2.c4 GT 100;

**Output:**

::

   SELECT t1.c1, t2.c2
     FROM tab1 t1, tab2 t2
    WHERE t1.c3 <> t1.c3
      AND t2.c4 > 100;

.. _en-us_topic_0000001860318805__en-us_topic_0000001384071352_section1154914204319:

EQ and NE
---------

**Input: Comparison operations (EQ and NE)**

::

   SELECT t1.c1, t2.c2
     FROM tab1 t1 INNER JOIN tab2 t2
       ON t1.c2 EQ t2.c2
    WHERE t1.c6 NE 1000;

**Output:**

::

    SELECT t1.c1, t2.c2
     FROM tab1 t1 INNER JOIN tab2 t2
       ON t1.c2 = t2.c2
    WHERE
           t1.c6 <> 1000;

.. _en-us_topic_0000001860318805__en-us_topic_0000001384071352_section17665223164312:

LE and GE
---------

**Input: Comparison operations (LE and GE)**

::

   SELECT t1.c1, t2.c2
     FROM tab1 t1, tab2 t2
    WHERE t1.c3 LE 200
      AND t2.c4 GE 100;

**Output:**

::

    SELECT t1.c1, t2.c2
      FROM tab1 t1, tab2 t2
     WHERE t1.c3 <= 200
       AND t2.c4 >= 100;

.. _en-us_topic_0000001860318805__en-us_topic_0000001384071352_section54741454124311:

NOT= and LT
-----------

**Input: Comparison operations (NOT= and LT)**

::

   SELECT t1.c1, t2.c2
     FROM tab1 t1, tab2 t2
    WHERE t1.c3 NOT= t1.c3
      AND t2.c4 LT 100;

**Output:**

::

   SELECT t1.c1, t2.c2
     FROM tab1 t1, tab2 t2
    WHERE t1.c3 <> t1.c3
      AND t2.c4 < 100;

.. _en-us_topic_0000001860318805__en-us_topic_0000001384071352_section11180165912431:

**IN and NOT IN**
-----------------

For details, see **IN and NOT IN Conversion**.

**Input: IN and NOT IN**

::

    SELECT c1, c2
      FROM tab1
     WHERE c1 IN 'XY';

**Output:**

::

   SELECT c1, c2
     FROM tab1
    WHERE c1 = 'XY';

.. note::

   GaussDB(DWS) does not support **IN** and **NOT IN** operators in some specific scenarios.

.. _en-us_topic_0000001860318805__en-us_topic_0000001384071352_section1960913364411:

IS NOT IN
---------

**Input: IS NOT IN**

::

   SELECT c1, c2
     FROM tab1
    WHERE c1 IS NOT IN (subquery);

**Output:**

::

   SELECT c1, c2
     FROM tab1
    WHERE c1 NOT IN (subquery);

.. _en-us_topic_0000001860318805__en-us_topic_0000001384071352_section992210864416:

LIKE ALL/NOT LIKE ALL
---------------------

**Input: LIKE ALL / NOT LIKE ALL**

::

   SELECT c1, c2
     FROM tab1
    WHERE c3 NOT LIKE ALL ('%STR1%', '%STR2%', '%STR3%');

**Output:**

::

   SELECT c1, c2
     FROM tab1
    WHERE c3 NOT LIKE ALL (ARRAY[ '%STR1%', '%STR2%', '%STR3%' ]);

.. _en-us_topic_0000001860318805__en-us_topic_0000001384071352_section8492111613446:

LIKE ANY/NOT LIKE ANY
---------------------

**Input: LIKE ANY / NOT LIKE ANY**

::

   SELECT c1, c2
     FROM tab1
    WHERE c3 LIKE ANY ('STR1%', 'STR2%', 'STR3%');

**Output:**

::

   SELECT c1, c2
     FROM tab1
    WHERE c3 LIKE ANY (ARRAY[ 'STR1%', 'STR2%', 'STR3%' ]);
