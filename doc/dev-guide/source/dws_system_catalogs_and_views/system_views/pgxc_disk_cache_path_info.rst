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

|image1|

.. |image1| image:: /_static/images/en-us_image_0000002621971765.png
