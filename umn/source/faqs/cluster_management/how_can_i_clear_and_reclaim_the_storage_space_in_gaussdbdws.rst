:original_name: dws_03_0033.html

.. _dws_03_0033:

How Can I Clear and Reclaim the Storage Space in GaussDB(DWS)?
==============================================================

After you delete data stored in GaussDB(DWS) data warehouses, dirty data may be generated possibly because the disk space is not released. This results in disk space waste and deteriorates snapshot creation and restoration performance. The following describes the impact on the system and subsequent operation to clear the disk space:

Points worth mentioning during clearing and reclaiming storage space:

-  Unnecessary data needs to be deleted to release the storage space.
-  Frequent read and write operations may affect proper database use. Therefore, it is good practice to clear and reclaim the storage space when not in peak hours.
-  The data clearing time depends on the data stored in the database.

Perform the following steps to clear and reclaim the storage space:

#. Connect to the database. For details, see section "Connecting to a GaussDB(DWS) Cluster" in the *Data Warehouse Service (DWS) User Guide*.

#. Run the following command to clear and reclaim the storage space:

   **VACUUM FULL;**

   By default, tables the current user has the permission on are deleted. Other tables are skipped.

   The following information is displayed once the space is cleared:

   ::

      VACUUM

   .. note::

      -  **VACUUM FULL** reclaims all expired row space, however it requires an exclusive lock on each table being processed, and might take a long time to complete on large, distributed database tables. You are advised to do **VACUUM FULL** to specified tables. If you want to do **VACUUM FULL** to the entire database, you are advised to do it during database maintenance.
      -  The statistical information will be lost if you use the **FULL** parameter. To collect the statistics, add keyword **ANALYZE**, for example, **VACUUM FULL ANALYZE;**.
