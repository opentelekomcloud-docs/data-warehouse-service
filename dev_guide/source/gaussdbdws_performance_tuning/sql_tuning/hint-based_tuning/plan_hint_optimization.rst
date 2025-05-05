:original_name: dws_04_0455.html

.. _dws_04_0455:

Plan Hint Optimization
======================

In plan hints, you can specify a join order, join, stream, and scan operations, the number of rows in a result, and redistribution skew information to tune an execution plan, improving query performance.

Function
--------

Plan hints can be specified using the keywords such as **SELECT**, **INSERT**, **UPDATE**, **MERGE**, and **DELETE**, in the following format:

::

   /*+  */

You can specify multiple hints for a query plan and separate them by spaces. A hint specified for a query plan does not apply to its subquery plans. To specify a hint for a subquery, add the hint following the keyword of this subquery.

For example:

::

   select /*+   */ * from t1, (select /*+  */ from t2) where 1=1;

In the preceding command, <*plan_hint1*> and <*plan_hint2*> are the hints of a query, and <*plan_hint3*> is the hint of its subquery.

.. important::

   If a hint is specified in the **CREATE VIEW** statement, the hint will be applied each time this view is used.

   If the random plan function is enabled (**plan_mode_seed** is set to a value other than 0), the specified hint will not be used.

Supported Hints
---------------

Currently, the following hints are supported:

-  Join order hints (**leading**)
-  Join operation hints, excluding the **semi join**, **anti join**, and **unique plan** hints
-  Rows hints
-  Stream operation hints
-  Scan operation hints, supporting only **tablescan**, **indexscan**, and **indexonlyscan**
-  Sublink name hints
-  Skew hints, supporting only the skew in the redistribution involving Join or HashAgg
-  Hint used for **Agg** distribution columns Only clusters of 8.1.3.100 and later versions support this function.
-  Hint that disables subquery pull-up. Only clusters of 8.2.0 and later versions support this function.
-  Configuration parameter hints. For details about supported parameters, see :ref:`Configuration Parameter Hints <dws_04_0463>`.

Precautions
-----------

-  **Sort**, **Setop**, and **Subplan** hints are not supported.
-  Hints do not support SMP or Node Groups.
-  Hints cannot be used for the target table of the **INSERT** statement.

.. _en-us_topic_0000002053159594__en-us_topic_0000001658028034_section671421102912:

Examples
--------

The following is the original plan and is used for comparing with the optimized ones:

::

   explain
   select i_product_name product_name
   ,i_item_sk item_sk
   ,s_store_name store_name
   ,s_zip store_zip
   ,ad2.ca_street_number c_street_number
   ,ad2.ca_street_name c_street_name
   ,ad2.ca_city c_city
   ,ad2.ca_zip c_zip
   ,count(*) cnt
   ,sum(ss_wholesale_cost) s1
   ,sum(ss_list_price) s2
   ,sum(ss_coupon_amt) s3
   FROM   store_sales
   ,store_returns
   ,store
   ,customer
   ,promotion
   ,customer_address ad2
   ,item
   WHERE  ss_store_sk = s_store_sk AND
   ss_customer_sk = c_customer_sk AND
   ss_item_sk = i_item_sk and
   ss_item_sk = sr_item_sk and
   ss_ticket_number = sr_ticket_number and
   c_current_addr_sk = ad2.ca_address_sk and
   ss_promo_sk = p_promo_sk and
   i_color in ('maroon','burnished','dim','steel','navajo','chocolate') and
   i_current_price between 35 and 35 + 10 and
   i_current_price between 35 + 1 and 35 + 15
   group by i_product_name
   ,i_item_sk
   ,s_store_name
   ,s_zip
   ,ad2.ca_street_number
   ,ad2.ca_street_name
   ,ad2.ca_city
   ,ad2.ca_zip
   ;

|image1|

.. |image1| image:: /_static/images/en-us_image_0000001657868950.png
