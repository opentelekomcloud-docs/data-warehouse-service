:original_name: dws_03_2143.html

.. _dws_03_2143:

How Are Dirty Pages Generated in GaussDB(DWS)?
==============================================

Causes
------

GaussDB(DWS) employs the multi-version concurrency control (MVCC) mechanism to guarantee consistency and concurrency when multiple transactions access the database. Its advantages include unblocked read-write operations, while its disadvantages include disk bloating issues. The MVCC mechanism is the primary cause of dirty pages.

The scenarios are as follows:

-  When the DELETE operation is performed on a table, data is logically deleted but not physically deleted from the disk.
-  When an UPDATE operation is performed on a table, GaussDB(DWS) logically marks the original data to be updated for deletion while inserting new data.

For the DELETE and UPDATE operations in a table, the data marked as deleted is called discarded tuples. The proportion of discarded tuples in the entire table is the dirty page rate. Therefore, when the dirty page rate of a table is high, the proportion of data marked as deleted in the table is high.

Solution:
---------

With GaussDB(DWS), you can easily query the dirty page rate through a system view. For details, see "PGXC_STAT_TABLE_DIRTY" in *Data Warehouse Service (DWS) Development Guide*.

GaussDB(DWS) offers the **VACUUM** function to address disk space bloat resulting from high dirty page rates. This function clears data marked for deletion. For details, see "VACUUM" in the *Data Warehouse Service (DWS) SQL Syntax Reference*.

**VACUUM** does not release the allocated space. To completely reclaim the cleared space, run **VACUUM FULL**.

.. note::

   -  **VACUUM FULL** clears and releases the space of deleted data, improving database performance and efficiency. However, running **VACUUM FULL** consumes more time and resources, and may cause some tables to be locked. Therefore, run **VACUUM FULL** only when the database load is light.
   -  To reduce the impact of disk bloat on database performance, you are advised to do **VACUUM FULL** on non-system catalogs whose dirty page rate exceeds 80%. You can determine whether to do **VACUUM FULL** based on service scenarios.
