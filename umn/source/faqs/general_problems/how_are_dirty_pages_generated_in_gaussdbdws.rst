:original_name: dws_03_2143.html

.. _dws_03_2143:

How Are Dirty Pages Generated in GaussDB(DWS)?
==============================================

Causes
------

By using the versioning concurrency control (MVCC) mechanism, GaussDB(DWS) can achieve consistency and concurrency for multiple transactions that access the database simultaneously. This mechanism has the benefit of avoiding read-write conflicts, but the drawback of causing disk bloat and dirty pages.

The scenarios are as follows:

-  When the DELETE operation is performed on a table, data is logically deleted but not physically deleted from the disk.
-  When the UPDATE operation is performed on a table, GaussDB(DWS) logically marks the data to be updated as delete and inserts new data.

For the DELETE and UPDATE operations in a table, the data marked as deleted is called discarded tuples. The proportion of discarded tuples in the entire table is the dirty page rate. Therefore, when the dirty page rate of a table is high, the proportion of data marked as deleted in the table is high.

Solution:
---------

GaussDB(DWS) provides a system view for querying the dirty page rate. For details, see section "PGXC_STAT_TABLE_DIRTY" in *Data Warehouse Service (DWS) Development Guide* .

To solve the problem of disk space bloat caused by high dirty page rate, GaussDB(DWS) provides the VACUUM function to clear the data logically marked as deleted. For details, see "VACUUM" in *Data Warehouse Service (DWS) SQL Syntax Reference* .

**VACUUM** does not release the allocated space. To completely reclaim the cleared space, run **VACUUM FULL**.

.. note::

   -  **VACUUM FULL** clears and releases the space of deleted data, improving database performance and efficiency. However, running **VACUUM FULL** consumes more time and resources, and may cause some tables to be locked. Therefore, run **VACUUM FULL** only when the database load is light.
   -  To reduce the impact of disk bloat on database performance, you are advised to do **VACUUM FULL** on non-system catalogs whose dirty page rate exceeds 80%. You can determine whether to do **VACUUM FULL** based on service scenarios.
