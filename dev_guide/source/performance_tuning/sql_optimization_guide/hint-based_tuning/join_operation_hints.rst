:original_name: dws_04_0457.html

.. _dws_04_0457:

Join Operation Hints
====================

Function
--------

Specifies the join method. It can be nested loop join, hash join, or merge join.

Syntax
------

.. code-block:: console

   [no] nestloop|hashjoin|mergejoin([@block_name] table_list)

.. _en-us_topic_0000001510402197__section35948678143011:

Parameter Description
---------------------

-  **no** indicates that the specified hint will not be used for a join.
-  *block_name* indicates the block name of the statement block. For details, see :ref:`block_name <en-us_topic_0000001460722632__li99021444551>`.
-  *table_list* specifies the tables to be joined. The values are the same as those of :ref:`join_table_list <en-us_topic_0000001460722632__section1280444714345>` but contain no parentheses.

For example:

**no nestloop(t1 t2 t3)**: **nestloop** is not used for joining **t1**, **t2**, and **t3**. The three tables may be joined in either of the two ways: Join **t2** and **t3**, and then **t1**; join **t1** and **t2**, and then **t3**. This hint takes effect only for the last join. If necessary, you can hint other joins. For example, you can add **no nestloop(t2 t3)** to join **t2** and **t3** first and to forbid the use of **nestloop**.

Examples
--------

Hint the query plan in :ref:`Examples <en-us_topic_0000001460562888__section671421102912>` as follows:

::

   explain
   select /*+ nestloop(store_sales store_returns item) */ i_product_name product_name ...

**nestloop** is used for the last join between **store_sales**, **store_returns**, and **item**. The optimized plan is as follows:

|image1|

.. |image1| image:: /_static/images/en-us_image_0000001460723036.png
