:original_name: dws_04_0459.html

.. _dws_04_0459:

Stream Operation Hints
======================

Function
--------

Specifies the stream method, which can be broadcast, redistribute, or specifying the distribution key for **Agg** redistribution.

.. note::

   Specifies the hint for the distribution column druing the Agg process.. This parameter is supported only by clusters of version 8.1.3.100 or later.

Syntax
------

::

   [no] broadcast | redistribute(table_list) | redistribute ((*) (columns))

Parameter Description
---------------------

-  **no** indicates that the hinted stream method is not used. When the hint is specified for the distribution columns in the **Agg** redistribution, **no** is invalid.

-  *table_list* specifies the tables to be joined. For details, see :ref:`Parameter Description <en-us_topic_0000001233563309__section35948678143011>`.
-  When hints are specified for distribution columns, the asterisk (*) is fixed and the table name cannot be specified.
-  **columns** specifies one or more columns in the **GROUP BY** clause. When there are no **GROUP BY** clauses, it can specify the columns in the **DISTINCT** clause.'

.. note::

   -  The specified distribution column must be represented by the column number in **GROUP BY** or **DISTINCT**. The column name cannot be specified.
   -  For a multi-layer query, you can specify the distribution column hint at each layer. The hint takes effect only at the corresponding layer.
   -  If the optimizer finds that redistribution is not required after estimation, the specified distribution column is invalid.

Tips
----

-  Generally, the optimizer selects a group of non-skew distribution keys for data redistribution based on statistics. If the default distribution keys have data skew, you can manually specify the distribution columns to avoid data skew.
-  When selecting a distribution key, select a group of columns with high distinct values as the distribution key based on data distribution features. In this way, data can be evenly distributed to each DN after redistribution.
-  After writing hints, you can run **explain verbose** to print the execution plan and check whether the specified distribution key is valid. If the specified distribution key is invalid, a warning is displayed.

Example
-------

-  Hint the query plan in :ref:`Examples <en-us_topic_0000001188642062__section671421102912>` as follows:

   ::

      explain
      select /*+ no redistribute(store_sales store_returns item store) leading(((store_sales store_returns item store) customer)) */ i_product_name product_name ...

   In the original plan, the join result of **store_sales**, **store_returns**, **item**, and **store** is redistributed before it is joined with **customer**. After the hinting, the redistribution is disabled and the join order is retained. The optimized plan is as follows:

   |image1|

-  Specifies the distribution columns for Agg redistribution.

   ::

      explain (verbose on, costs off, nodes off)
      select /*+ redistribute ((*) (2 3)) */ a1, b1, c1, count(c1)  from t1 group by a1, b1, c1 having  count(c1) > 10 and sum(d1) > 100

   In the following example, the last two columns of the specified **GROUP BY** columns are used as distribution keys.

   |image2|

-  If the statement does not contain the **GROUP BY** clause, specify the distinct column as the distribution columns.

   ::

      explain (verbose on, costs off, nodes off)
      select /*+ redistribute ((*) (3 1)) */ distinct a1, b1, c1 from t1;

   |image3|

.. |image1| image:: /_static/images/en-us_image_0000001285555909.png
.. |image2| image:: /_static/images/en-us_image_0000001241395974.png
.. |image3| image:: /_static/images/en-us_image_0000001241715218.png
