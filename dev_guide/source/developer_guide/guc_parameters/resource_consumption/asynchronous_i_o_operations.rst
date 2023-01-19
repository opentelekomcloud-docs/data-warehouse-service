:original_name: dws_04_0898.html

.. _dws_04_0898:

Asynchronous I/O Operations
===========================

enable_adio_debug
-----------------

**Parameter description**: Specifies whether O&M personnel are allowed to generate some ADIO logs to locate ADIO issues. This parameter is used only by developers. Common users are advised not to use it.

**Type**: SUSET

**Value range**: Boolean

-  **on** or **true** indicates the log switch is enabled.
-  **off** or **false** indicates the log switch is disabled.

**Default value**: **off**

enable_fast_allocate
--------------------

**Parameter description**: Specifies whether the quick allocation switch of the disk space is enabled. This switch can be enabled only in the XFS file system.

**Type**: SUSET

**Value range**: Boolean

-  **on** or **true** indicates that this function is enabled.
-  **off** or **false** indicates that the function is disabled.

**Default value**: **off**

prefetch_quantity
-----------------

**Parameter description**: Specifies the number of row-store prefetches using the ADIO.

**Type**: USERSET

**Value range**: an integer ranging from 1024 to 1048576. The unit is 8 KB.

**Default value**: **32 MB**

backwrite_quantity
------------------

**Parameter description**: Specifies the number of row-store writes using the ADIO.

**Type**: USERSET

**Value range**: an integer ranging from 1024 to 1048576. The unit is 8 KB.

**Default value**: **8MB**

cstore_prefetch_quantity
------------------------

**Parameter description**: Specifies the number of column-store prefetches using the ADIO.

**Type**: USERSET

**Value range**: an integer. The value range is from 1024 to 1048576 and the unit is KB.

**Default value**: **32 MB**

cstore_backwrite_quantity
-------------------------

**Parameter description**: Specifies the number of column-store writes using the ADIO.

**Type**: USERSET

**Value range**: an integer. The value range is from 1024 to 1048576 and the unit is KB.

**Default value**: **8MB**

cstore_backwrite_max_threshold
------------------------------

**Parameter description**: Specifies the maximum number of column-store writes buffered in the database using the ADIO.

**Type**: USERSET

**Value range**: An integer. The value range is from 4096 to INT_MAX/2 and the unit is KB.

**Default value**: **2 GB**

fast_extend_file_size
---------------------

**Parameter description**: Specifies the disk size that the row-store pre-scales using the ADIO.

**Type**: SUSET

**Value range**: an integer. The value range is from 1024 to 1048576 and the unit is KB.

**Default value**: **8MB**

effective_io_concurrency
------------------------

**Parameter description**: Specifies the number of requests that can be simultaneously processed by the disk subsystem. For the RAID array, the parameter value must be the number of disk drive spindles in the array.

**Type**: USERSET

**Value range**: an integer ranging from 0 to 1000

**Default value**: **1**
