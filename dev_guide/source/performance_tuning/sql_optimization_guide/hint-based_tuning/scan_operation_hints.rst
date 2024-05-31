:original_name: dws_04_0460.html

.. _dws_04_0460:

Scan Operation Hints
====================

Function
--------

These hints specify a scan operation, which can be **tablescan**, **indexscan**, or **indexonlyscan**.

Syntax
------

::

   [no] tablescan|indexscan|indexonlyscan(table [index])

Parameter Description
---------------------

-  **no** indicates that the specified hint will not be used for a join.

-  *table* specifies the table to be scanned. You can specify only one table. Use a table alias (if any) instead of a table name.
-  *index* indicates the index for **indexscan** or **indexonlyscan**. You can specify only one index.

.. note::

   **indexscan** and **indexonlyscan** hints can be used only when the specified index belongs to the table.

   Scan operation hints can be used for row-store tables, column-store tables, HDFS tables, HDFS foreign tables, OBS tables, and subquery tables. HDFS tables include primary tables and delta tables. The delta tables are invisible to users. Therefore, scan operation hints are used only for primary tables.

Example
-------

To specify an index-based hint for a scan, create an index named **i** on the **i_item_sk** column of the **item** table.

::

   create index i on item(i_item_sk);

Hint the query plan in :ref:`Examples <en-us_topic_0000001188642062__section671421102912>` as follows:

::

   explain
   select /*+ indexscan(item i) */ i_product_name product_name ...

**item** is scanned based on an index. The optimized plan is as follows:

|image1|

.. |image1| image:: /_static/images/en-us_image_0000001188323806.png
