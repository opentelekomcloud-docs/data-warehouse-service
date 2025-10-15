:original_name: dws_04_0928.html

.. _dws_04_0928:

Lock Management
===============

In GaussDB(DWS), concurrent transactions may cause single-node deadlocks or distributed deadlocks due to resource competition. This section describes parameters used for managing transaction lock mechanisms.

.. _en-us_topic_0000001811490993__s34083b462e2947b5a232d8b3a1465d3b:

deadlock_timeout
----------------

**Parameter description**: Specifies the time, in milliseconds, to wait on a lock before checking whether there is a deadlock condition. When the applied lock exceeds the preset value, the system will check whether a deadlock occurs.

-  The check for deadlock is relatively expensive. Therefore, the server does not check it when waiting for a lock every time. Deadlocks do not frequently occur when the system is running. Therefore, the system just needs to wait on the lock for a while before checking for a deadlock. Increasing this value reduces the time wasted in needless deadlock checks, but slows down reporting of real deadlock errors. On a heavily loaded server, you may need to raise it. The value you have set needs to exceed the transaction time. By doing this, the possibility that a lock will be released before the waiter decides to check for deadlocks will be reduced.
-  When :ref:`log_lock_waits <en-us_topic_0000001811491197__s80fbcd77ad5a4cdc879fe344d17b2c13>` is set, this parameter also determines the duration you need to wait before a log message about the lock wait is issued. If you are trying to investigate locking delays, you need to set this parameter to a value smaller than normal **deadlock_timeout**.

**Type**: SUSET

**Value range**: an integer ranging from 1 to 2147483647. The unit is millisecond (ms).

**Default value**: **1s**

ddl_lock_timeout
----------------

**Parameter description**: Indicates the number of seconds a DDL command should wait for the locks to become available. If the time spent in waiting for a lock exceeds the specified time, an error is reported. This parameter is supported only by clusters of version 8.1.3.200 or later.

**Type**: SUSET

**Value range**: an integer ranging from 0 to INT_MAX. The unit is millisecond (ms).

-  If the value of this parameter is 0, this parameter does not take effect.
-  If the value of this parameter is greater than 0, the lock wait time of DDL statements is the value of this parameter, and the lock wait time of other locks is the value of **lockwait_timeout**.

**Default value**: **0**

.. note::

   This parameter has a higher priority than **lockwait_timeout** and takes effect only for **AccessExclusiveLock**.

ddl_select_concurrent_mode
--------------------------

**Parameter description**: Specifies the concurrency mode of DDL and **SELECT** statements. This parameter is supported only by clusters of version 8.1.3.320, 8.2.1, or later.

**Type**: SUSET

**Value range**: a string

-  **none**: DDL and select statements cannot be executed concurrently. Waiting statements are in the lock wait state.
-  **truncate**: When the **TRUNCATE** statement is blocked by the **SELECT** statement, the **SELECT** statement is interrupted and the **TRUNCATE** statement is executed first.
-  **exchange**: When the **EXCHANGE** statement is blocked by the **SELECT** statement, the **SELECT** statement is interrupted and the **EXCHANGE** statement is executed first.
-  **vacuum_full**: When the **vacuum_full** statement is blocked by the **SELECT** statement, the **SELECT** statement is interrupted and the **vacuum_full** statement is executed first. This is supported only by clusters of version 9.1.0.200 or later.
-  **insert_overwrite**: When the **insert_overwrite** statement is blocked by the **SELECT** statement, the **SELECT** statement is interrupted and the **insert_overwrite** statement is executed first. This is supported only by clusters of version 9.1.0.200 or later.

**Default value**: **none**

.. note::

   -  To reserve time for the **SELECT** statement to respond to signals, if the value of **ddl_lock_timeout** is less than 1 second in the current version, 1 second is used.
   -  Concurrency is not supported when there are conflicts with locks of higher levels (more than one level). For example, **autoanalyze** is triggered by **SELECT** when **autoanalyze_mode** is set to **normal**.
   -  This parameter allows for **SELECT** statements in either a single statement or a transaction block. However, in other versions, it only supports **SELECT** statements in a single statement. For concurrent **SELECT** operations in a single statement or transaction block, learn more information in the description of parameter :ref:`enable_cancel_select_in_txnblock <en-us_topic_0000001811490993__section148041451182219>`.
   -  Values other than **none** can be used together. For example, if this parameter is set to **truncate, exchange**, the **TRUNCATE** and **EXCHANGE** statements are blocked by the **SELECT** statement. The **SELECT** statement is interrupted and executed first.

.. _en-us_topic_0000001811490993__section148041451182219:

enable_cancel_select_in_txnblock
--------------------------------

**Parameter description**: Specifies whether the **SELECT** statement in a transaction block can be interrupted. This parameter is supported only by clusters of version 8.2.1, 9.1.0.200, or later.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that the select operation in the transaction block can be interrupted.
-  **off** indicates that the select operation in the transaction block cannot be interrupted.

**Default value**: **on**

.. note::

   -  This parameter controls whether the **SELECT** statement in a transaction block can be interrupted by the DDL operation specified in **ddl_select_concurrent_mode**.
   -  The **ddl_select_concurrent_mode** parameter controls DDL statements such as **TRUNCATE** and **EXCHANGE**, and the **enable_cancel_select_in_txnblock** parameter controls **SELECT** statements.

.. _en-us_topic_0000001811490993__s4c1383de18ec4928a1f9d7a7a4c0498b:

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

**Default value**: **2min**

partition_lock_upgrade_timeout
------------------------------

**Parameter description**: Specifies the time to wait before the attempt of a lock upgrade from ExclusiveLock to AccessExclusiveLock times out on partitions.

-  When you do MERGE PARTITION and CLUSTER PARTITION on a partitioned table, temporary tables are used for data rearrangement and file exchange. To concurrently perform as many operations as possible on the partitions, ExclusiveLock is acquired for the partitions during data rearrangement and AccessExclusiveLock is acquired during file exchange.
-  Generally, a partition waits until it acquires a lock, or a timeout occurs if the partition waits for a period of time longer than specified by the :ref:`lockwait_timeout <en-us_topic_0000001811490993__s4c1383de18ec4928a1f9d7a7a4c0498b>` parameter.
-  When doing MERGE PARTITION or CLUSTER PARTITION on a partitioned table, you need to acquire AccessExclusiveLock during file exchange. If the lock fails to be acquired, the acquisition is retried in 50 ms. This parameter specifies the time to wait before the lock acquisition attempt times out.
-  If this parameter is set to **-1**, the lock upgrade never times out. The lock upgrade is continuously retried until it succeeds.

**Type**: USERSET

**Value range**: an integer ranging from -1 to 3000, in seconds

**Default value**: **1800**

enable_release_scan_lock
------------------------

**Parameter description**: Specifies whether a SELECT statement releases a level-1 lock after the statement execution is complete. This parameter reduces DDL conflicts with SELECT locks within transaction blocks. This parameter is supported only by clusters of version 8.3.0 or later.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that DDL operations will be blocked to wait for the release of cluster locks. The SELECT statement releases the level-1 lock after it finishes, not when the transaction commits.
-  **off** indicates that DDL operations will not be blocked.

**Default value**: **off**

vacuum_full_interruptible
-------------------------

**Parameter description**: Controls the behavior that the **VACUUM FULL** statement gives a lock to other statements. This is supported only by clusters of version 9.1.0.200 or later.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that DDL operations will be blocked to wait for the release of cluster locks. When **VACUUM FULL** blocks other statements, it interrupts the execution and gives the lock to other statements.
-  **off** indicates that DDL operations will not be blocked. When **VACUUM FULL** blocks other statements, it does not interrupt the execution. Other statements can be executed only after **VACUUM FULL** has completed and released the lock.

**Default value**: **off**
