:original_name: dws_06_0070.html

.. _dws_06_0070:

Simple Expressions
==================

Logical Expressions
-------------------

:ref:`Logical Operators <dws_06_0028>` lists the operators and calculation rules of logical expressions.

Comparative Expressions
-----------------------

:ref:`Comparison Operators <dws_06_0029>` lists the common comparative operators.

In addition to comparative operators, you can also use the following sentence structure:

-  BETWEEN

   The operator BETWEEN...AND selects a data range between two values. These values can be numeric, text, or date.

   The expression **a BETWEEN x AND y** is equivalent to the expression **a >= x AND a <= y**.

   ::

      SELECT 2 BETWEEN 1 AND 3 AS RESULT;
       result
      ----------
       t
      (1 row)

      SELECT 2 >= 1 AND 2 <= 3 AS RESULT;
       result
      ----------
       t
      (1 row)

   **a NOT BETWEEN x AND y** is equivalent to **a < x OR a > y**.

   ::

      SELECT 2 NOT BETWEEN 1 AND 3 AS RESULT;
       result
      ----------
       f
      (1 row)

      SELECT 2 < 1 OR 2 > 3 AS RESULT;
       result
      ----------
       f
      (1 row)

-  To check whether a value is null, use:

   expression IS NULL

   expression IS NOT NULL

   ::

      SELECT 2+2 IS NULL AS RESULT;
       result
      ----------
       f
      (1 row)

      SELECT 2+2 IS NOT NULL AS RESULT;
       result
      ----------
       t
      (1 row)

   or an equivalent (non-standard) sentence structure:

   expression ISNULL

   expression NOTNULL

   .. important::

      Do not write **expression=NULL** or **expression<>(!=)NULL**, because **NULL** represents an unknown value, and these expressions cannot determine whether two unknown values are equal.

   ::

      SELECT 2+2 ISNULL AS RESULT;
       result
      ----------
       f
      (1 row)

      SELECT 2+2 NOTNULL AS RESULT;
       result
      ----------
       t
      (1 row)

   ::

      SELECT 2+2 IS DISTINCT FROM NULL AS RESULT;
       result
      ----------
       t
      (1 row)

      SELECT 2+2 IS NOT DISTINCT FROM NULL AS RESULT;
       result
      ----------
       f
      (1 row)
