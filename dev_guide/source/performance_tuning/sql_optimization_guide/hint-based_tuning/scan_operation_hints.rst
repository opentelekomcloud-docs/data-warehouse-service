:original_name: dws_04_0460.html

.. _dws_04_0460:

Scan Operation Hints
====================

Function
--------

These hints specify a scan operation, which can be **tablescan**, **indexscan**, or **indexonlyscan**.

Syntax
------

.. code-block:: console

   [no] tablescan|indexscan|indexonlyscan([@block_name] table [index])

Parameter Description
---------------------

-  **no** indicates that the specified hint will not be used for a join.
-  *block_name* indicates the block name of the statement block. For details, see :ref:`block_name <en-us_topic_0000001460722632__li99021444551>`.
-  *table* specifies the table to be scanned. You can specify only one table. Use a table alias (if any) instead of a table name.

   .. note::

      -  The syntax format of the table is as follows:

         [schema.]table[@block_name]

         The table name can contain the schema name or block name before the subquery statement block is promoted. If the subquery statement block is optimized and rewritten by the optimizer, the value of **block_name** is different from that of **block_name** in leading.

      -  If a table has an alias, the alias is preferentially used to represent the table.

-  *index* indicates the index for **indexscan** or **indexonlyscan**. You can specify only one index.

   .. note::

      **indexscan** and **indexonlyscan** hints can be used only when the specified index belongs to the table.

      Scan operation hints can be used for row-store tables, column-store tables, HDFS tables, HDFS foreign tables, OBS tables, and subquery tables. HDFS tables include primary tables and delta tables. The delta tables are invisible to users. Therefore, scan operation hints are used only for primary tables.

      If **indexscan** is specified, **indexscan** or **indexonlyscan** takes effect. **indexscan** and **indexonlyscan** can also take effect at the same time. When **indexscan** and **indexonlyscan hints** appear at the same time, **indexonlyscan** takes effect first.

Example
-------

To specify an index-based hint for a scan, create an index named **i** on the **i_item_sk** column of the **item** table.

::

   create index i on item(i_item_sk);

Hint the query plan in :ref:`Examples <en-us_topic_0000001460562888__section671421102912>` as follows:

::

   explain
   select /*+ indexscan(item i) */ i_product_name product_name ...

**item** is scanned based on an index. The optimized plan is as follows:

|image1|

.. |image1| image:: /_static/images/en-us_image_0000001460723188.png
