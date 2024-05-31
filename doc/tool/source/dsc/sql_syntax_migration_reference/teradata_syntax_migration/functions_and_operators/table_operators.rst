:original_name: dws_16_0048.html

.. _dws_16_0048:

.. _en-us_topic_0000001819336101:

Table Operators
===============

The functions that can be called in the FROM clause of a query are from the table operator.

**Input: Table operator with RETURNS**

::

   SELECT *
     FROM TABLE( sales_retrieve (9005) RETURNS ( store INTEGER, item CLOB, quantity BYTEINT) ) AS ret;

**Output**

::

   SELECT *
     FROM sales_retrieve(9005) AS ret (store, item, quantity);
