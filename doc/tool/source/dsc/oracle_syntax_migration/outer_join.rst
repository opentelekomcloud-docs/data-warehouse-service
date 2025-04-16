:original_name: dws_mt_0132.html

.. _dws_mt_0132:

OUTER JOIN
==========

This section describes the migration syntax of Oracle **OUTER JOIN**. The migration syntax determines how the keywords/features are migrated.

An **OUTER JOIN** returns all rows that meet the join condition. If rows of a table cannot join any rows in the other table, the statement returns these rows. In Oracle:

-  Left outer join of tables A and B returns all rows from A and rows that satisfy the join condition by applying the outer join operator (+) to all columns of B in the **WHERE** conditions.
-  Right outer join of tables A and B returns all rows from B and rows that satisfy the join condition by applying the outer join operator (+) to all columns of A in the **WHERE** condition.

GaussDB(DWS) does not support the **+** operator. The function of this operator is enabled using **LEFT OUTER JOIN** and **RIGHT OUTER JOIN** keywords.


.. figure:: /_static/images/en-us_image_0000001706105257.png
   :alt: **Figure 1** Input: OUTER JOIN

   **Figure 1** Input: OUTER JOIN


.. figure:: /_static/images/en-us_image_0000001706224505.png
   :alt: **Figure 2** Output: OUTER JOIN

   **Figure 2** Output: OUTER JOIN
