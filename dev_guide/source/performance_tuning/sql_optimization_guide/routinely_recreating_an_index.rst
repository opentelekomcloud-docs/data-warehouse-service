:original_name: dws_04_0467.html

.. _dws_04_0467:

Routinely Recreating an Index
=============================

Context
-------

When data deletion is repeatedly performed in the database, index keys will be deleted from the index page, resulting in index distention. Recreating an index routinely improves query efficiency.

The database supports B-tree, GIN, and psort indexes.

-  Recreating a B-tree index helps improve query efficiency.

   -  If massive data is deleted, index keys on the index page will be deleted. As a result, the number of index pages reduces and index bloat occurs. Recreating an index helps reclaim wasted space.
   -  In the created index, pages adjacent in its logical structure are adjacent in its physical structure. Therefore, a created index achieves higher access speed than an index that has been updated for multiple times.

-  You are advised not to recreate a non-B-tree index.

Rebuilding an Index
-------------------

Use either of the following two methods to recreate an index:

-  Run the **DROP INDEX** statement to delete an index and run the **CREATE INDEX** statement to create an index.

   When you delete an index, a temporary exclusive lock is added in the parent table to block related read/write operations. When you create an index, the write operation is locked but the read operation is not. The data is read and scanned by order.

-  Run the **REINDEX** statement to recreate an index:

   -  When you run the **REINDEX TABLE** statement to recreate an index, an exclusive lock is added to block related read/write operations.
   -  When you run the **REINDEX INTERNAL TABLE** statement to recreate an index for a **desc** table (), an exclusive lock is added to block read/write operations on the table.

Procedure
---------

Assume the ordinary index areaS_idx exists in the **area_id** column of the imported table **areaS**. Use either of the following two methods to recreate an index:

-  Run the **DROP INDEX** statement to delete the index and run the **CREATE INDEX** statement to create an index.

   #. Delete an index.

      .. code-block::

         DROP INDEX areaS_idx;
         DROP INDEX

   #. Create an index.

      .. code-block::

         CREATE INDEX areaS_idx ON areaS (area_id);
         CREATE INDEX

-  Run the **REINDEX** statement to recreate an index.

   -  Run the **REINDEX TABLE** statement to recreate an index.

      .. code-block::

         REINDEX TABLE areaS;
         REINDEX

   -  Run the **REINDEX INTERNAL TABLE** statement to recreate an index for a **desc** table ().

      .. code-block::

         REINDEX INTERNAL TABLE areaS;
         REINDEX
