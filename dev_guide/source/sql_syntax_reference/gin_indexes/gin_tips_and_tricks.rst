:original_name: dws_06_0274.html

.. _dws_06_0274:

GIN Tips and Tricks
===================

Create vs. Insert

Insertion into a GIN index can be slow due to the likelihood of many keys being inserted for each item. So, for bulk insertions into a table, it is advisable to drop the GIN index and recreate it after finishing the bulk insertions. GUC parameters related to GIN index creation and query performance as follows:

-  maintenance_work_mem

   Build time for a GIN index is very sensitive to the **maintenance_work_mem** setting.

-  work_mem

   During a series of insertions into an existing GIN index that has **fastupdate** enabled, the system will clean up the pending-entry list whenever the list grows larger than **work_mem**. To avoid fluctuations in observed response time, it is desirable to have pending-list cleanup occur in the background (that is, via autovacuum). Foreground cleanup operations can be avoided by increasing **work_mem** or making **autovacuum** more aggressive. However, if **work_mem** is increased, a foreground cleanup (if any) will take a longer time.

-  gin_fuzzy_search_limit

   The primary goal of developing GIN indexes is to create support for highly scalable full-text search in GaussDB(DWS). However, a very large set of results may be returned by a full-text query for words that frequently occur. In addition, reading many tuples from the disk and sorting them will consume large numbers of resources, which is unacceptable for production.

   To facilitate controlled execution of such queries, GIN has a configurable soft upper limit on the number of rows returned: the **gin_fuzzy_search_limit** configuration parameter. It is set to 0 (meaning no limit) by default. If a non-zero limit is set, then the returned set is a subset of the whole result set, chosen at random.
