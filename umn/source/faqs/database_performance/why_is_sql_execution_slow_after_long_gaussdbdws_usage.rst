:original_name: dws_03_0071.html

.. _dws_03_0071:

Why Is SQL Execution Slow After Long GaussDB(DWS) Usage?
========================================================

After a database is used for a period of time, the table data increases as services grow, or the table data is frequently added, deleted, or modified. As a result, bloating tables and inaccurate statistics are incurred, deteriorating database performance.

You are advised to periodically run **VACUUM FULL** and **ANALYZE** on tables that are frequently added, deleted, or modified. Perform the following operations:

#. By default, 100 out of 30,000 records of statistics are collected. When a large amount of data is involved, the SQL execution is unstable, which may be caused by a changed execution plan. In this case, the sampling rate needs to be adjusted for statistics. You can run **set default_statistics_target** to increase the sampling rate, which helps the optimizer generate the optimal plan.

   |image1|

#. Run **ANALYZE** again. For details, see "ANALYZE \| ANALYSE" in the *Data Warehouse Service (DWS) Developer Guide*.

   |image2|

.. note::

   To test whether disk fragments affect database performance, use the following function:

   .. code-block::

      SELECT * FROM pgxc_get_stat_dirty_tables(30,100000);

.. |image1| image:: /_static/images/en-us_image_0000001381728713.png
.. |image2| image:: /_static/images/en-us_image_0000001381889321.png
