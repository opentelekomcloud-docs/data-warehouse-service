:original_name: dws_04_1233.html

.. _dws_04_1233:

PGXC_DISK_CACHE_PATH_INFO
=========================

**PGXC_DISK_CACHE_PATH_INFO** records information about the hard disk where the file cache is stored. This system view is supported only by clusters of version 9.1.0 or later.

.. table:: **Table 1** PGXC_DISK_CACHE_PATH_INFO columns

   +----------------+------------------+-------------------------------------------------------+
   | Column         | Type             | Description                                           |
   +================+==================+=======================================================+
   | path_name      | Text             | Path name.                                            |
   +----------------+------------------+-------------------------------------------------------+
   | node_name      | Text             | Name of the node the hard disk belongs to.            |
   +----------------+------------------+-------------------------------------------------------+
   | cache_size     | Bigint           | Total size of cache files in the hard disk, in bytes. |
   +----------------+------------------+-------------------------------------------------------+
   | disk_available | Bigint           | Available space in the hard disk, in bytes.           |
   +----------------+------------------+-------------------------------------------------------+
   | disk_size      | Bigint           | Total capacity of the hard drive, in bytes.           |
   +----------------+------------------+-------------------------------------------------------+
   | disk_use_ratio | Double precision | Disk space usage.                                     |
   +----------------+------------------+-------------------------------------------------------+

Example
-------

Query information about the hard disk used by the file cache.

::

   SELECT * FROM pgxc_disk_cache_path_info order by 1;
      path_name    |  node_name   | cache_size | disk_available |  disk_size   |  disk_use_ratio
   ----------------+--------------+------------+----------------+--------------+------------------
    dn_6001_6002_0 | dn_6001_6002 |      19619 |   137401716736 | 160982630400 | .146481105479564
    dn_6001_6002_1 | dn_6001_6002 |      35968 |   137401716736 | 160982630400 | .146481105479564
    dn_6003_6004_0 | dn_6003_6004 |      27794 |   121600655360 | 160982630400 | .244634933235629
    dn_6003_6004_1 | dn_6003_6004 |      26158 |   121600655360 | 160982630400 | .244634933235629
    dn_6005_6006_0 | dn_6005_6006 |      24533 |   134394839040 | 160982630400 | .165159379579873
    dn_6005_6006_1 | dn_6005_6006 |      31065 |   134394839040 | 160982630400 | .165159379579873
