:original_name: dws_04_1237.html

.. _dws_04_1237:

Disk Caching
============

The following parameters are supported only by clusters of version 9.1.0 or later.

enable_disk_cache
-----------------

**Parameter description**: Specifies whether to enable file caching. Setting this parameter to **on** only takes effect when **enable_aio_scheduler** is set to **on** and **obs_worker_pool_size** is greater than or equal to 4.

**Type**: USERSET

**Value range**: Boolean

**Default value**: **off**

enable_disk_cache_recovery
--------------------------

**Parameter description**: Specifies whether file caching can be restored when the cluster is restarted.

**Type**: USERSET

**Value range**: Boolean

**Default value**: **off**
