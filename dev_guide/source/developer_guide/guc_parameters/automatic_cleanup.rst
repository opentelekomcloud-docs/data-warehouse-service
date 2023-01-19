:original_name: dws_04_0923.html

.. _dws_04_0923:

Automatic Cleanup
=================

The automatic cleanup process (autovacuum) in the system automatically runs the **VACUUM** and **ANALYZE** commands to recycle the record space marked by the deleted status and update statistics in the table.

autovacuum
----------

**Parameter description**: Enables the automatic cleanup process (autovacuum) in the database. Ensure that the :ref:`track_counts <en-us_topic_0000001098974554__s4682d08468f84845bfdc6ae9477126e8>` parameter is set to **on** before enabling the automatic cleanup process.

**Type**: SIGHUP

.. note::

   -  Set the **autovacuum** parameter to **on** if you want to enable the function to automatically clean up two-phase transactions after the system recovers from faults.
   -  If **autovacuum** is set to **on** and the value of :ref:`autovacuum_max_workers <en-us_topic_0000001145814611__s502d4304994d4da5bd3cda661aab27ac>` is **0**, the system will not automatically clean up two-phase transactions. The system will clean up them after recovering from faults.
   -  If **autovacuum** is set to **on** and the value of :ref:`autovacuum_max_workers <en-us_topic_0000001145814611__s502d4304994d4da5bd3cda661aab27ac>` is greater than **0**, the system will automatically clean up the two-phase transactions and processes after recovering from faults.

.. important::

   Even if the **autovacuum** parameter is set to **off**, the automatic cleanup process will be enabled automatically by the database when a transaction ID wrap is about to occur. When the create database or drop database operation fails, some nodes may be submitted or rolled back while others in the prepared status may not be submitted. In this case, the system cannot automatically restore these nodes and the manual restoration is required. The restoration steps are as follows:

   #. Use the **gs_clean** tool (setting the **option** parameter to **-N**) to query the xid of the abnormal two-phase transactions and nodes in the prepared state.
   #. Log in to the nodes whose transactions are in the prepared status. Administrators connect to an available database such as gaussdb to run the **set xc_maintenance_mode = on** statement.
   #. Submit or roll back the two-phase transactions (for example, submit or roll back a statement) based on global transaction status.

**Value range**: Boolean

-  **on** indicates the database automatic cleanup process is enabled.
-  **off** indicates the database automatic cleanup process is disabled.

**Default value**: **off**

autovacuum_mode
---------------

**Parameter description**: Specifies whether the autoanalyze or autovacuum function is enabled. This parameter is valid only when **autovacuum** is set to **on**.

**Type**: SIGHUP

**Value range**: enumerated values

-  **analyze** indicates that only autoanalyze is performed.
-  **vacuum** indicates that only autovacuum is performed.
-  **mix** indicates that both autoanalyze and autovacuum are performed.
-  **none** indicates that neither of them is performed.

**Default value**: **mix**

autoanalyze_timeout
-------------------

**Parameter description**: Specifies the timeout period of autoanalyze. If the duration of autoanalyze on a table exceeds the value of **autoanalyze_timeout**, the autoanalyze is automatically canceled.

**Type**: SIGHUP

**Value range**: an integer ranging from 0 to 2147483. The unit is second.

**Default value**: **5min**

autovacuum_io_limits
--------------------

**Parameter description**: Specifies the upper limit of I/Os triggered by the autovacuum process per second.

**Type**: SIGHUP

**Value range**: an integer ranging from -1 to 1073741823. **-1** indicates that the default Cgroup is used.

**Default value**: **-1**

log_autovacuum_min_duration
---------------------------

**Parameter description**: Records each step performed by the automatic cleanup process to the server log when the execution time of the automatic cleanup process is greater than or equal to a certain value. This parameter helps track the automatic cleanup behaviors.

**Type**: SIGHUP

For example, set the **log_autovacuum_min_duration** parameter to 250 ms to record the information related to the automatic cleanup commands running the parameters whose values are greater than or equal to 250 ms.

**Value range**: an integer ranging from -1 to INT_MAX. The unit is ms.

-  If this parameter is set to **0**, all the automatic cleanup operations are recorded in the log.
-  If this parameter is set to **-1**, all the automatic cleanup operations are not recorded in the log.
-  If this parameter is not set to **-1**, an automatic cleanup operation is skipped and a message is recorded due to lock conflicts.

**Default value**: **-1**

.. _en-us_topic_0000001145814611__s502d4304994d4da5bd3cda661aab27ac:

autovacuum_max_workers
----------------------

**Parameter description**: Specifies the maximum number of automatic cleanup threads running at the same time.

**Type**: POSTMASTER

**Value range**: an integer ranging from 0 to 262143. **0** indicates that autovacuum is disabled.

**Default value**: **3**

autovacuum_naptime
------------------

**Parameter description**: Specifies the interval between two automatic cleanup operations.

**Type**: SIGHUP

**Value range**: an integer ranging from 1 to 2147483. The unit is second.

**Default value**: **10min**

**autovacuum_vacuum_threshold**
-------------------------------

**Parameter description**: Specifies the threshold for triggering the **VACUUM** operation. When the number of deleted or updated records in a table exceeds the specified threshold, the **VACUUM** operation is executed on this table.

**Type**: SIGHUP

**Value range**: an integer ranging from **0** to **INT_MAX**

**Default value**: **50**

autovacuum_analyze_threshold
----------------------------

**Parameter description**: Specifies the threshold for triggering the **ANALYZE** operation. When the number of deleted, inserted, or updated records in a table exceeds the specified threshold, the **ANALYZE** operation is executed on this table.

**Type**: SIGHUP

**Value range**: an integer ranging from **0** to **INT_MAX**

**Default value**: **50**

autovacuum_vacuum_scale_factor
------------------------------

**Parameter description**: Specifies the size scaling factor of a table added to the **autovacuum_vacuum_threshold** parameter when a **VACUUM** event is triggered.

**Type**: SIGHUP

**Value range**: a floating point number ranging from 0.0 to 100.0

**Default value**: **0.2**

autovacuum_analyze_scale_factor
-------------------------------

**Parameter description**: Specifies the size scaling factor of a table added to the **autovacuum_analyze_threshold** parameter when an **ANALYZE** event is triggered.

**Type**: SIGHUP

**Value range**: a floating point number ranging from 0.0 to 100.0

**Default value**: **0.1**

autovacuum_freeze_max_age
-------------------------

**Parameter description**: Specifies the maximum age (in transactions) that a table's **pg_class.relfrozenxid** column can attain before a VACUUM operation is forced to prevent transaction ID wraparound within the table.

The old files under the subdirectory of **pg_clog/** can also be deleted by the VACUUM operation. Even if the automatic cleanup process is forbidden, the system will invoke the automatic cleanup process to prevent the cyclic repetition.

**Type**: POSTMASTER

**Value range**: an integer ranging from 100000 to 576460752303423487

**Default value**: **20000000000**

autovacuum_vacuum_cost_delay
----------------------------

**Parameter description**: Specifies the value of the cost delay used in the autovacuum operation.

**Type**: SIGHUP

**Value range**: an integer ranging from -1 to 100. The unit is ms. **-1** indicates that the normal vacuum cost delay is used.

**Default value**: **20ms**

autovacuum_vacuum_cost_limit
----------------------------

**Parameter description**: Specifies the value of the cost limit used in the autovacuum operation.

**Type**: SIGHUP

**Value range**: an integer ranging from -1 to 10000. **-1** indicates that the normal vacuum cost limit is used.

**Default value**: **-1**
