:original_name: dws_04_0459.html

.. _dws_04_0459:

Stream Operation Hints
======================

Function
--------

These hints specify a stream operation, which can be **broadcast** or **redistribute**.

Syntax
------

::

   [no] broadcast|redistribute(table_list)

Parameter Description
---------------------

-  **no** indicates that the specified hint will not be used for a join.

-  *table_list* specifies the tables to be joined. For details, see :ref:`Parameter Description <en-us_topic_0000001099134840__section35948678143011>`.

Examples
--------

Hint the query plan in :ref:`Examples <en-us_topic_0000001098974750__section671421102912>` as follows:

::

   explain
   select /*+ no redistribute(store_sales store_returns item store) leading(((store_sales store_returns item store) customer)) */ i_product_name product_name ...

In the original plan, the join result of **store_sales**, **store_returns**, **item**, and **store** is redistributed before it is joined with **customer**. After the hinting, the redistribution is disabled and the join order is retained. The optimized plan is as follows:

|image1|

.. |image1| image:: /_static/images/en-us_image_0000001145495247.png
