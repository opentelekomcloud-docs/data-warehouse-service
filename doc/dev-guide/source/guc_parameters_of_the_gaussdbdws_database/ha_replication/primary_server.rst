:original_name: dws_04_0906.html

.. _dws_04_0906:

Primary Server
==============

enable_data_replicate
---------------------

**Parameter description**: Specifies the data synchronization mode between the primary and standby servers when data is imported to row-store tables in a database.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that data pages are used for the data synchronization between the primary and standby servers when data is imported to row-store tables in a database. This parameter cannot be set to **on** if **replication_type** is set to **1**.
-  **off** indicates that the primary and standby servers synchronize data using Xlogs while the data is imported to a row-store table.

**Default value**: **on**

ha_module_debug
---------------

**Parameter description**: This command is used to view the replication status log of a specific data block during data replication.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that the log records the status of each data block during data replication.
-  **off** indicates that the status of each data block is not recorded in logs during data replication.

**Default value:** **off**
