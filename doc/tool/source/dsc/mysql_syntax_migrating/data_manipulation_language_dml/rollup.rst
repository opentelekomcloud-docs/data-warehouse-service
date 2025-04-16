:original_name: dws_16_0183.html

.. _dws_16_0183:

.. _en-us_topic_0000001813598888:

ROLLUP
======

**group by column with rollup** in MySQL needs to be converted to **group by rollup (column)** in GaussDB(DWS).

**Input**

::

   select id,product_id,count(1) from czb_account.equity_account_log
   where id in (6957343,6957397,6957519,6957541,6957719)
   group by 1, 2 with rollup;

**Output**

::

   SELECT
     id,
     product_id,
     count(1)
   FROM
     czb_account.equity_account_log
   WHERE
     id IN (6957343, 6957397, 6957519, 6957541, 6957719)
   GROUP BY
     ROLLUP(1, 2);
