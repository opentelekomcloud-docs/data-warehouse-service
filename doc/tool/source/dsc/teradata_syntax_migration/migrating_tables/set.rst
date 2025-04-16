:original_name: dws_16_0066.html

.. _dws_16_0066:

.. _en-us_topic_0000001860199041:

SET
===

**SET** is a unique feature in Teradata. It does not allow duplicate records. It is addressed using the **MINUS** set operator. Migration tool supports MULTISET and SET tables. SET table can be used with VOLATILE.

**Input: SET TABLE**

::

   CREATE SET VOLATILE TABLE tab1 ... ;
   INSERT INTO tab1
   SELECT expr1, expr2, ...
   FROM tab1, ...
   WHERE .... ;

**Output:**

::

   CREATE LOCAL TEMPORARY TABLE tab1
   ... ; INSERT INTO tab1
   SELECT expr1, expr2, ...
   FROM tab1, ...
   WHERE ....
   MINUS
   SELECT * FROM tab1 ;
