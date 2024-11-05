:original_name: dws_04_0901.html

.. _dws_04_0901:

Settings
========

.. _en-us_topic_0000001510402533__s9be202a993664326975a0e79a16d60c0:

wal_level
---------

**Parameter description:** Specifies the level of the information that is written to WALs.

**Type**: POSTMASTER

**Value range**: enumerated values

-  minimal

   Advantages: Certain bulk operations (including creating tables and indexes, executing cluster operations, and copying tables) are safely skipped in logging, which can make those operations much faster.

   Disadvantages: WALs only contain basic information required for the recovery from a database server crash or an emergency shutdown. Archived WALs cannot be used to restore data.

-  archive

   Adds logging required for WAL archiving, supporting the database restoration from archives.

-  hot_standby

   -  Further adds information required to run SQL queries on a standby server and takes effect after a server restart.
   -  To enable read-only queries on a standby server, the **wal_level** parameter must be set to **hot_standby** on the primary server and the same value must be set on the standby server. There is little measurable difference in performance between using **hot_standby** and **archive** levels, so feedback is welcome if any production performance impacts are noticeable.

**Default value**: **hot_standby**

.. important::

   -  To enable WAL archiving and data streaming replication between primary and standby servers, set this parameter to **archive** or **hot_standby**.
   -  If this parameter is set to **archive**, **hot_standby** must be set to **off**. Otherwise, the database startup fails.

synchronous_commit
------------------

**Parameter description**: Specifies the synchronization mode of the current transaction.

**Type**: USERSET

**Value range**: enumerated values

-  **on** indicates synchronization logs of a standby server are updated to disks.
-  **off** indicates asynchronous commit.
-  **local** indicates local commit.
-  **remote_write** indicates synchronization logs of a standby server are written to disks.
-  **remote_receive** indicates synchronization logs of a standby server are required to receive data.

**Default value**: **on**

wal_buffers
-----------

**Parameter description**: Specifies the number of XLOG_BLCKSZs used for storing WAL data. The size of each XLOG_BLCKSZ is 8 KB.

**Type**: POSTMASTER

**Value range**: -1 to 2\ :sup:`18`. The unit is 8 KB.

-  If this parameter is set to **-1**, the value of **wal_buffers** is automatically changed to 1/32 of **shared_buffers**. The minimum value is 8 x **XLOG_BLCKSZ**, and the maximum value is 2048 x **XLOG_BLCKSZ**.
-  If it is set to a value smaller than **8**, the value **8** is used. If it is set to a value greater than 2048, the value **2048** is used.

**Default value**: **256 MB**

**Setting suggestions**: The content of WAL buffers is written to disks at each transaction commit, and setting this parameter to a large value does not significantly improve system performance. Setting this parameter to hundreds of megabytes can improve the disk writing performance on the server, to which a large number of transactions are committed. Based on experiences, the default value meets user requirements in most cases.

enable_wal_decelerate
---------------------

**Parameter description**: Specifies whether to enable WAL log rate limiting. This parameter is supported only by cluster versions 8.2.0 and later.

**Type**: SIGHUP

**Value range**: Boolean

-  **on** indicates that this feature is enabled.
-  **off** indicates that this feature is disabled.

**Default value**: **on**

wal_decelerate_policy
---------------------

**Parameter description**: Specifies the behavior policy after rate limiting is triggered. This parameter is supported only by clusters of 8.2.0 and later versions.

**Type**: USERSET

**Value range**: enumerated values

-  **warning** indicates that an alarm is generated but the rate is not limited.
-  **decelerate** indicates that the rate will be limited based on policy settings.

**Default value**: **warning**

.. note::

   Setting the parameter to **warning** does not affect performance. Setting it to **decelerate** will limit the rate based on policy settings if the rate exceeds the threshold.

wal_write_speed
---------------

**Parameter description**: Specifies the maximum WAL write speed (byte/s) allowed by each query on a single DN. This parameter is supported only by clusters of 8.2.0 or later.

**Type**: USERSET

**Value range**: an integer ranging from 1024 to 10240000, in KB.

**Default value**: **30MB**

.. note::

   The rate of a large number of jobs with index copy and deletion operations will be limited.

wal_decelerate_trigger_threshold
--------------------------------

**Parameter description**: Specifies the threshold of WAL write rate limiting for each query on a single DN. This parameter is supported only by cluster versions 8.2.0 and later.

**Type**: USERSET

**Value range**: an integer ranging from 1024 to 100000000000, in KB.

**Default value**: **128MB**

.. note::

   This function is triggered only if the number of Xlogs generated by a single query is greater than the value of this parameter. DDL operations or a small number of DML operations are not affected.

.. _en-us_topic_0000001510402533__seb6fde0eb5bf4b5488d9f6069aeeaa5c:

commit_delay
------------

**Parameter description**: Specifies the duration of committed data be stored in the WAL buffer.

**Type**: USERSET

**Value range**: an integer, ranging from 0 to 100000 (unit: μs). **0** indicates no delay.

**Default value**: **0**

.. important::

   -  When this parameter is set to a value other than 0, the committed transaction is stored in the WAL buffer instead of being written to the WAL immediately. Then, the WalWriter process flushes the buffer out to disks periodically.
   -  If system load is high, other transactions are probably ready to be committed within the delay. If no transactions are waiting to be submitted, the delay is a waste of time.

commit_siblings
---------------

**Parameter description**: Specifies a limit on the number of ongoing transactions. If the number of ongoing transactions is greater than the limit, a new transaction will wait for the period of time specified by :ref:`commit_delay <en-us_topic_0000001510402533__seb6fde0eb5bf4b5488d9f6069aeeaa5c>` before it is submitted. If the number of ongoing transactions is less than the limit, the new transaction is immediately written into a WAL.

**Type**: USERSET

**Value range**: an integer ranging from 0 to 1000

**Default value**: **5**

wal_compression
---------------

**Parameter description**: Specifies whether to compress FPI pages.

**Type**: USERSET

**Value range**: Boolean

-  **on**: enable the compression
-  **off**: disable the compression

**Default value**: **on**

.. important::

   -  Only zlib compression algorithm is supported.
   -  For clusters that are upgraded to the current version from an earlier version, this parameter is set to **off** by default. You can run the **gs_guc** command to enable the FPI compression function if needed.
   -  If the current version is a newly installed version, this parameter is set to **on** by default.
   -  If this parameter is manually enabled for a cluster upgraded from an earlier version, the cluster cannot be rolled back.

wal_compression_level
---------------------

**Parameter description**: Specifies the compression level of zlib compression algorithm when the **wal_compression** parameter is enabled.

**Type**: USERSET

**Value range**: an integer ranging from 0 to 9.

-  **0** indicates no compression.
-  **1** indicates the lowest compression ratio.
-  **9** indicates the highest compression ratio.

**Default value**: **9**
