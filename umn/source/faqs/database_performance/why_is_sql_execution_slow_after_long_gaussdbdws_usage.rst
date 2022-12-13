:original_name: dws_03_0071.html

.. _dws_03_0071:

Why Is SQL Execution Slow After Long GaussDB(DWS) Usage?
========================================================

When a database is used for a period of time, the table data increases as services grow, or the table data is frequently added, deleted, or modified. This results in bloating tables and inaccurate statistics, deteriorating database performance.

You are advised to periodically perform **VACUUM FULL** and **ANALYZE** on tables that are frequently added, deleted, or modified.

#. By default, 100 out of 30,000 records of statistics are collected. When a large amount of data is involved, the SQL execution is unstable, which may be caused by a changed execution plan. In this case, the sampling rate needs to be adjusted for statistics. You can run **set default_statistics_target** to increase the sampling rate, which helps the optimizer generate the optimal plan.

   |image1|

#. Perform **ANALYZE** again. For details, see "ANALYZE \| ANALYSE" in the *Developer Guide*.

   |image2|

.. note::

   To test whether disk fragments affect database performance, use the following function:

   .. code-block::

      select * from pgxc_get_stat_dirty_tables(30,100000);

.. |image1| image:: /_static/images/en-us_image_0000001192746637.png
.. |image2| image:: /_static/images/en-us_image_0000001146626882.png
