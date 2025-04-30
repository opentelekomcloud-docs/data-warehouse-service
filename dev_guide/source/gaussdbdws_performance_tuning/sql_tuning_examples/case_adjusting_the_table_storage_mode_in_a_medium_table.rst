:original_name: dws_04_0482.html

.. _dws_04_0482:

Case: Adjusting the Table Storage Mode in a Medium Table
========================================================

In GaussDB(DWS), row-store tables use the row execution engine, and column-store tables use the column execution engine. If both row-store table and column-store tables exist in a SQL statement, the system will automatically select the row execution engine. The performance of a column execution engine (except for the indexscan related operators) is much better than that of a row execution engine. Therefore, a column-store table is recommended. This is important for some medium result set dumping tables, and you need to select a proper table storage type.

Before Optimization
-------------------

During the test at a site, if the following execution plan is performed, the customer expects that the performance can be improved and the result can be returned within 3s.

|image1|

After Optimization
------------------

It is found that the row engine is used after analysis, because both the temporary plan table input_acct_id_tbl and the medium result dumping table row_unlogged_table use a row-store table.

After the two tables are changed into column-store tables, the system performance is improved and the result is returned by 1.6s.

|image2|

.. |image1| image:: /_static/images/en-us_image_0000001188323768.jpg
.. |image2| image:: /_static/images/en-us_image_0000001233883401.png
