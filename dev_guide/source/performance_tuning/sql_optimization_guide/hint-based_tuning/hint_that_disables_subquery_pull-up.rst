:original_name: dws_04_1076.html

.. _dws_04_1076:

Hint That Disables Subquery Pull-up
===================================

Function
--------

To optimize query logic, the optimizer usually pulls up subqueries for execution. However, sometimes the pulled up subqueries do not run much faster than others, and may even be slower due to enlarged search scope. In this case, you can specify the **no merge** hint to disable pull-up. This hint is not recommended in most cases.

Syntax
------

.. code-block:: console

   no merge[@block_name]
   no merge ([@block_name1] subquery_name[@block_name2])

Description
-----------

-  *block_name* indicates the block name of the statement block. For details, see :ref:`block_name <en-us_topic_0000001460722632__li99021444551>`.
-  **subquery_name** indicates the name of a subquery. It can also be a view or CTE name. The specified subquery will not be unnested during logic optimization. If **subquery_name** is not specified, the current query will not be unnested.

Example
-------

Create tables **t1**, **t2**, and **t3**.

::

   create table t1(a1 int,b1 int,c1 int,d1 int);
   create table t2(a2 int,b2 int,c2 int,d2 int);
   create table t3(a3 int,b3 int,c3 int,d3 int);

The original statement is as follows:

::

   explain select * from t3, (select a1,b2,c1,d2 from t1,t2 where t1.a1=t2.a2) s1 where t3.b3=s1.b2;

|image1|

In this query, you can use the following methods to disable the pull-up of subquery **s1**:

-  Method 1:

   ::

      explain select /*+ no merge(s1) */ * from t3, (select a1,b2,c1,d2 from t1,t2 where t1.a1=t2.a2) s1 where t3.b3=s1.b2;

-  Method 2:

   ::

      explain select * from t3, (select /*+ no merge */ a1,b2,c1,d2 from t1,t2 where t1.a1=t2.a2) s1 where t3.b3=s1.b2;

Outcome:

|image2|

.. |image1| image:: /_static/images/en-us_image_0000001460563364.png
.. |image2| image:: /_static/images/en-us_image_0000001460882872.png
