:original_name: dws_04_0461.html

.. _dws_04_0461:

Sublink Name Hints
==================

Function
--------

These hints specify the name of a sublink block.

Syntax
------

.. code-block:: console

   blockname ([@block_name] table)

Precautions
-----------

-  This block name hint is used by an outer query only when a sublink is pulled up. Currently, only the **Agg** equivalent join, **IN**, and **EXISTS** sublinks can be pulled up. This hint is usually used together with the hints described in the previous sections.

-  The subquery after the **FROM** keyword is hinted by using the subquery alias. In this case, **block_name hint** becomes invalid.
-  If a sublink contains multiple tables, the tables will be joined with the outer-query tables in a random sequence after the sublink is pulled up. In this case, **blockname** also becomes invalid.

Parameter Description
---------------------

-  *block_name* indicates the block name of the statement block. For details, see :ref:`block_name <en-us_topic_0000002080515442__en-us_topic_0000001460722632_li99021444551>`.
-  *table* indicates the name you have specified for a sublink block.

   .. note::

      -  The syntax format of the table is as follows:

         [schema.]table[@block_name]

         The table name can contain the schema name or block name before the subquery statement block is promoted. If the subquery statement block is optimized and rewritten by the optimizer, the value of **block_name** is different from that of **block_name** in leading.

      -  If a table has an alias, the alias is preferentially used to represent the table.

Example
-------

::

   explain select /*+nestloop(store_sales tt) */ * from store_sales where ss_item_sk in (select /*+blockname(tt)*/ i_item_sk from item group by 1);

**tt** indicates the sublink block name. After being pulled up, the sublink is joined with the outer-query table **store_sales** by using **nestloop**. The optimized plan is as follows:

|image1|

.. |image1| image:: /_static/images/en-us_image_0000001510523041.png
