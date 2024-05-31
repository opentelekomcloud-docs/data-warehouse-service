:original_name: dws_04_0928.html

.. _dws_04_0928:

Lock Management
===============

In GaussDB(DWS), a deadlock may occur when concurrently executed transactions compete for resources. This section describes parameters used for managing transaction lock mechanisms.

.. _en-us_topic_0000001233563133__s34083b462e2947b5a232d8b3a1465d3b:

deadlock_timeout
----------------

**Parameter description**: Specifies the time, in milliseconds, to wait on a lock before checking whether there is a deadlock condition. When the applied lock exceeds the preset value, the system will check whether a deadlock occurs.

-  The check for deadlock is relatively expensive. Therefore, the server does not check it when waiting for a lock every time. Deadlocks do not frequently occur when the system is running. Therefore, the system just needs to wait on the lock for a while before checking for a deadlock. Increasing this value reduces the time wasted in needless deadlock checks, but slows down reporting of real deadlock errors. On a heavily loaded server, you may need to raise it. The value you have set needs to exceed the transaction time. By doing this, the possibility that a lock will be released before the waiter decides to check for deadlocks will be reduced.
-  When :ref:`log_lock_waits <en-us_topic_0000001233761693__s80fbcd77ad5a4cdc879fe344d17b2c13>` is set, this parameter also determines the duration you need to wait before a log message about the lock wait is issued. If you are trying to investigate locking delays, you need to set this parameter to a value smaller than normal **deadlock_timeout**.

**Type**: SUSET

**Value range**: an integer ranging from 1 to 2147483647. The unit is millisecond (ms).

**Default value**: **1s**

ddl_lock_timeout
----------------

**Parameter description**: Indicates the number of seconds a DDL command should wait for the locks to become available. If the time spent in waiting for a lock exceeds the specified time, an error is reported. (This parameter is supported only in 8.1.3.200 and later cluster versions.)

**Type**: SUSET

**Value range**: an integer ranging from 0 to INT_MAX. The unit is millisecond (ms).

-  If the value of this parameter is 0, this parameter does not take effect.
-  If the value of this parameter is greater than 0, the lock wait time of DDL statements is the value of this parameter, and the lock wait time of other locks is the value of **lockwait_timeout**.

**Default value**: **0**

.. note::

   This parameter has a higher priority than **lockwait_timeout** and takes effect only for **AccessExclusiveLock**.

.. _en-us_topic_0000001233563133__s4c1383de18ec4928a1f9d7a7a4c0498b:

lockwait_timeout
----------------

**Parameter description**: Specifies the longest time to wait before a single lock times out. If the time you wait before acquiring a lock exceeds the specified time, an error is reported.

**Type**: SUSET

**Value range**: an integer ranging from 0 to INT_MAX. The unit is millisecond (ms).

**Default value**: **20 min**

update_lockwait_timeout
-----------------------

**Parameter description**: sets the maximum duration that a lock waits for concurrent updates on a row to complete when the concurrent update feature is enabled. If the time you wait before acquiring a lock exceeds the specified time, an error is reported.

**Type**: SUSET

**Value range**: an integer ranging from 0 to INT_MAX. The unit is millisecond (ms).

**Default value**: **2 min**

max_locks_per_transaction
-------------------------

**Parameter description**: Controls the average number of object locks allocated for each transaction.

-  The size of the shared lock table is calculated under the condition that a maximum of *N* independent objects need to be locked at any time. *N* = max_locks_per_transaction x (max_connections + max_prepared_transactions). Objects that do not exceed the preset number can be locked simultaneously at any time. You may need to increase this value when you modify many different tables in a single transaction. This parameter can only be set at database start.
-  If this parameter is set to a large value, GaussDB(DWS) may require more System V shared memory than the default setting.
-  When running a standby server, you must set this parameter to a value that is no less than that on the primary server. Otherwise, queries will not be allowed on the standby server.

**Type**: POSTMASTER

**Value range**: an integer ranging from 10 to INT_MAX

**Default value**: **256**

ddl_select_concurrent_mode
--------------------------

**Parameter description**: Specifies the concurrency mode of DDL and **SELECT** statements. This parameter is supported only by clusters of 8.1.3.320, 8.2.1, and later versions.

**Type**: SUSET

**Value range**: enumerated values

-  **none**: DDL and **SELECT** statements cannot be executed concurrently. Waiting statements are in the lock wait state.
-  **truncate**: When a **TRUNCATE** statement is blocked by a **SELECT** statement, the **TRUNCATE** statement interrupts the **SELECT** statement and is executed first. Other DDL statements and **SELECT** statements remain in the lock wait state.
-  **exchange**: When an **EXCHANGE** statement is blocked by a **SELECT** statement, the **EXCHANGE** statement interrupts the **SELECT** statement and is executed first. Other DDL statements and **SELECT** statements remain in the lock wait state.
-  **truncate**, **exchange**: When a **TRUNCATE** and an **EXCHANGE** statement are blocked by the **SELECT** statement, the **SELECT** statement is interrupted and the **TRUNCATE** and **EXCHANGE** statement are executed first.

**Default value**: **none**

.. note::

   -  To reserve time for the **SELECT** statement to respond to signals, if the value of **ddl_lock_timeout** is less than 1 second in the current version, 1 second is used.
   -  Concurrency is not supported when there are conflicts with locks of higher levels (more than one level). For example, **autoanalyze** is triggered by **select** when **autoanalyze_mode** is set to **normal**.
   -  Concurrency is not supported when there are conflicts with locks in transaction blocks.

max_pred_locks_per_transaction
------------------------------

**Parameter description**: Controls the average number of predicated locks allocated for each transaction.

-  The size of the shared and predicated lock table is calculated under the condition that a maximum of *N* independent objects need to be locked at any time. *N* = max_pred_locks_per_transaction x (max_connections + max_prepared_transactions). Objects that do not exceed the preset number can be locked simultaneously at any time. You may need to increase this value when you modify many different tables in a single transaction. This parameter can only be set at server start.
-  If this parameter is set to a large value, GaussDB(DWS) may require more System V shared memory than the default setting.

**Type**: POSTMASTER

**Value range**: an integer ranging from 10 to INT_MAX

**Default value**: **64**

partition_lock_upgrade_timeout
------------------------------

**Parameter description**: Specifies the time to wait before the attempt of a lock upgrade from ExclusiveLock to AccessExclusiveLock times out on partitions.

-  When you do MERGE PARTITION and CLUSTER PARTITION on a partitioned table, temporary tables are used for data rearrangement and file exchange. To concurrently perform as many operations as possible on the partitions, ExclusiveLock is acquired for the partitions during data rearrangement and AccessExclusiveLock is acquired during file exchange.
-  Generally, a partition waits until it acquires a lock, or a timeout occurs if the partition waits for a period of time longer than specified by the :ref:`lockwait_timeout <en-us_topic_0000001233563133__s4c1383de18ec4928a1f9d7a7a4c0498b>` parameter.
-  When doing MERGE PARTITION or CLUSTER PARTITION on a partitioned table, you need to acquire AccessExclusiveLock during file exchange. If the lock fails to be acquired, the acquisition is retried in 50 ms. This parameter specifies the time to wait before the lock acquisition attempt times out.
-  If this parameter is set to **-1**, the lock upgrade never times out. The lock upgrade is continuously retried until it succeeds.

**Type**: USERSET

**Value range**: an integer ranging from -1 to 3000. The unit is second (s).

**Default value**: **1800**
