:original_name: dws_04_1232.html

.. _dws_04_1232:

PGXC_DISK_CACHE_STATS
=====================

**PGXC_DISK_CACHE_STATS** records the usage of file cache. This system view is supported only by clusters of version 9.1.0 or later.

.. table:: **Table 1** PGXC_DISK_CACHE_STATS columns

   +-------------+--------------+-------------------------------------------------------------+
   | Column      | Type         | Description                                                 |
   +=============+==============+=============================================================+
   | node_name   | Text         | Node name.                                                  |
   +-------------+--------------+-------------------------------------------------------------+
   | total_read  | Bigint       | Total number of accesses to disk cache.                     |
   +-------------+--------------+-------------------------------------------------------------+
   | local_read  | Bigint       | Total number of times disk cache reads from local disk.     |
   +-------------+--------------+-------------------------------------------------------------+
   | remote_read | Bigint       | Total number of times disk cache reads from remote storage. |
   +-------------+--------------+-------------------------------------------------------------+
   | hit_rate    | numeric(5,2) | Hit rate of disk cache.                                     |
   +-------------+--------------+-------------------------------------------------------------+
   | cache_size  | Bigint       | Total size of data saved in disk cache, in KB.              |
   +-------------+--------------+-------------------------------------------------------------+
   | fill_rate   | numeric(5,2) | Fill rate of disk cache.                                    |
   +-------------+--------------+-------------------------------------------------------------+

Example
-------

Query the hit rate of disk cache on each node.

::

   SELECT hit_rate FROM pgxc_disk_cache_stats;
    hit_rate
   ----------
       56.91
       56.85
         NaN
         NaN
         NaN
         NaN
   (6 rows)
