:original_name: dws_16_0181.html

.. _dws_16_0181:

.. _en-us_topic_0000001819336237:

Division Expressions
====================

In MySQL, in a division expression, if the divisor is 0, null is returned. GaussDB(DWS) reports an error. Therefore, the division expression is converted by adding an if condition expression.

**Input**

.. code-block::

   select sum(c1) / c2 as result from table_t1;
   select sum(c1) / count (c3/c4) as result from table_t1;

**Output**

.. code-block::

   SELECT  (if (c2 = 0, null, sum(c1) / c2)) AS "result" FROM  table_t1;
   SELECT  (if (count(if (c4 = 0, null, c3 / c4)) = 0, null, sum(c1) / count(if (c4 = 0, null, c3 / c4)))) AS "result" FROM  table_t1;
