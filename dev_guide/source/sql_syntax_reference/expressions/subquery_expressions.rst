:original_name: dws_06_0072.html

.. _dws_06_0072:

Subquery Expressions
====================

Subquery expressions include the following types:

-  EXISTS/NOT EXISTS

   :ref:`Figure 1 <en-us_topic_0000001145910889__f575631d43042448bb4015781d352f07b>` shows the syntax of an **EXISTS/NOT EXISTS** expression.

   .. _en-us_topic_0000001145910889__f575631d43042448bb4015781d352f07b:

   .. figure:: /_static/images/en-us_image_0000001145710995.png
      :alt: **Figure 1** EXISTS/NOT EXISTS::=

      **Figure 1** EXISTS/NOT EXISTS::=

   The parameter of an **EXISTS** expression is an arbitrary **SELECT** statement, or subquery. The subquery is evaluated to determine whether it returns any rows. If it returns at least one row, the result of **EXISTS** is "true". If the subquery returns no rows, the result of **EXISTS** is "false".

   The subquery will generally only be executed long enough to determine whether at least one row is returned, not all the way to completion.

   For example:

   ::

      SELECT sr_reason_sk,sr_customer_sk FROM tpcds.store_returns WHERE EXISTS (SELECT d_dom FROM tpcds.date_dim WHERE d_dom = store_returns.sr_reason_sk and sr_customer_sk <10);
      sr_reason_sk | sr_customer_sk
      --------------+----------------
                 13 |              2
                 22 |              5
                 17 |              7
                 25 |              7
                  3 |              7
                 31 |              5
                  7 |              7
                 14 |              6
                 20 |              4
                  5 |              6
                 10 |              3
                  1 |              5
                 15 |              2
                  4 |              1
                 26 |              3
      (15 rows)

-  IN/NOT IN

   :ref:`Figure 2 <en-us_topic_0000001145910889__f87790713e7a743a5b623e1f09c2da29b>` shows the syntax of an **IN/NOT IN** expression.

   .. _en-us_topic_0000001145910889__f87790713e7a743a5b623e1f09c2da29b:

   .. figure:: /_static/images/en-us_image_0000001145911043.png
      :alt: **Figure 2** IN/NOT IN::=

      **Figure 2** IN/NOT IN::=

   The right-hand side is a parenthesized subquery, which must return exactly one column. The left-hand expression is evaluated and compared to each row of the subquery result. The result of **IN** is "true" if any equal subquery row is found. The result is "false" if no equal row is found (including the case where the subquery returns no rows).

   This is in accordance with SQL's normal rules for Boolean combinations of null values. If the columns corresponding to two rows equal and are not empty, the two rows are equal to each other. If any columns corresponding to the two rows do not equal and are not empty, the two rows are not equal to each other. Otherwise, the result is **NULL**. If there are no equal right-hand values and at least one right-hand row yields null, the result of **IN** will be null, not false.

   For example:

   ::

       SELECT sr_reason_sk,sr_customer_sk  FROM tpcds.store_returns WHERE sr_customer_sk IN (SELECT d_dom FROM tpcds.date_dim WHERE d_dom < 10);
      sr_reason_sk | sr_customer_sk
      --------------+----------------
                 10 |              3
                 26 |              3
                 22 |              5
                 31 |              5
                  1 |              5
                 32 |              5
                 32 |              5
                  4 |              1
                 15 |              2
                 13 |              2
                 33 |              4
                 20 |              4
                 33 |              8
                  5 |              6
                 14 |              6
                 17 |              7
                  3 |              7
                 25 |              7
                  7 |              7
      (19 rows)

-  ANY/SOME

   :ref:`Figure 3 <en-us_topic_0000001145910889__fdceb3a5e35db44408dfa28a129fae88e>` shows the syntax of an **ANY/SOME** expression.

   .. _en-us_topic_0000001145910889__fdceb3a5e35db44408dfa28a129fae88e:

   .. figure:: /_static/images/en-us_image_0000001098671238.png
      :alt: **Figure 3** any/some::=

      **Figure 3** any/some::=

   The right-hand side is a parenthesized subquery, which must return exactly one column. The left-hand expression is evaluated and compared to each row of the subquery result using the given operator, which must yield a Boolean result. The result of **ANY** is "true" if any true result is obtained. The result is "false" if no true result is found (including the case where the subquery returns no rows). **SOME** is a synonym of **ANY**. **IN** can be equivalently replaced with **ANY**.

   For example:

   ::

      SELECT sr_reason_sk,sr_customer_sk  FROM tpcds.store_returns WHERE sr_customer_sk < ANY (SELECT d_dom FROM tpcds.date_dim WHERE d_dom < 10);
      sr_reason_sk | sr_customer_sk
      --------------+----------------
                 26 |              3
                 17 |              7
                 32 |              5
                 32 |              5
                 13 |              2
                 31 |              5
                 25 |              7
                  5 |              6
                  7 |              7
                 10 |              3
                  1 |              5
                 14 |              6
                  4 |              1
                  3 |              7
                 22 |              5
                 33 |              4
                 20 |              4
                 33 |              8
                 15 |              2
      (19 rows)

-  ALL

   :ref:`Figure 4 <en-us_topic_0000001145910889__f785d31056d7f4f3390b9f1bf6370fb59>` shows the syntax of an **ALL** expression.

   .. _en-us_topic_0000001145910889__f785d31056d7f4f3390b9f1bf6370fb59:

   .. figure:: /_static/images/en-us_image_0000001098831064.png
      :alt: **Figure 4** all::=

      **Figure 4** all::=

   The right-hand side is a parenthesized subquery, which must return exactly one column. The left-hand expression is evaluated and compared to each row of the subquery result using the given operator, which must yield a Boolean result. The result of **ALL** is "true" if all rows yield true (including the case where the subquery returns no rows). The result is "false" if any false result is found.

   Example:

   ::

      SELECT sr_reason_sk,sr_customer_sk  FROM tpcds.store_returns WHERE sr_customer_sk < all(SELECT d_dom FROM tpcds.date_dim WHERE d_dom < 10);
       sr_reason_sk | sr_customer_sk
      --------------+----------------
      (0 rows)
