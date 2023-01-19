:original_name: dws_04_0906.html

.. _dws_04_0906:

Primary Server
==============

vacuum_defer_cleanup_age
------------------------

**Parameter description**: Specifies the number of transactions by which **VACUUM** will defer the cleanup of invalid row-store table records, so that **VACUUM** and **VACUUM FULL** do not clean up deleted tuples immediately.

**Type**: SIGHUP

**Value range**: an integer ranging from 0 to 1000000. **0** means no delay.

**Default value**: **0**

data_replicate_buffer_size
--------------------------

**Parameter description**: Specifies the size of memory used by queues when the sender sends data pages to the receiver. The value of this parameter affects the buffer size copied for the replication between the primary and standby servers.

**Type**: POSTMASTER

**Value range**: an integer ranging from 4 to 1023. The unit is MB.

**Default value**: **128 MB**

enable_data_replicate
---------------------

**Parameter description**: Specifies the data synchronization mode between the primary and standby servers when data is imported to row-store tables in a database.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that data pages are used for the data synchronization between the primary and standby servers when data is imported to row-store tables in a database. This parameter cannot be set to **on** if **replication_type** is set to **1**.
-  **off** indicates that the primary and standby servers synchronize data using Xlogs while the data is imported to a row-store table.

**Default value**: **on**

enable_incremental_catchup
--------------------------

**Parameter description**: Specifies the data catchup mode between the primary and standby nodes.

**Type**: SIGHUP

**Value range:** Boolean

-  **on** indicates that the standby node uses the incremental catchup mode. That is, the standby server scans local data files on the standby server to obtain the list of differential data files between the primary and standby nodes and then performs catchup between the primary and standby nodes.
-  **off** indicates that the standby node uses the full catchup mode. That is, the standby node scans all local data files on the primary node to obtain the list of differential data files between the primary and standby nodes and performs catchup between the primary and standby nodes.

**Default value**: **on**

wait_dummy_time
---------------

**Parameter description**: Specifies the maximum duration for the primary, standby, and secondary clusters to wait for the secondary cluster to start in sequence and the maximum duration for the secondary cluster to send the scanning list when incremental data catchup is enabled.

**Type**: SIGHUP

**Value range**: Integer, from 1 to **INT_MAX**, in seconds.

**Default value**: **300s**

.. caution::

   The unit can only be second.
