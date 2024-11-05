:original_name: dws_04_0923.html

.. _dws_04_0923:

Automatic Cleanup
=================

The automatic cleanup process (**autovacuum**) in the system automatically runs the **VACUUM** and **ANALYZE** statements to reclaim the record space marked as deleted and update statistics about the table.

.. note::

   **autovacuum** does not block service statements initiated by users. **autovacuum** and **autoanalyze** statements can be executed concurrently without conflicts. This function is supported only in version later than 8.2.1.300.

.. _en-us_topic_0000001510283565__s8d6c38309e594a16a07f79ae412b63c6:

autovacuum
----------

**Parameter description**: Specifies whether to start the automatic cleanup process (**autovacuum**). Ensure that the :ref:`track_counts <en-us_topic_0000001510402293__s4682d08468f84845bfdc6ae9477126e8>` parameter is set to **on** before enabling the automatic cleanup process.

**Type**: SIGHUP

**Value range**: Boolean

-  **on** indicates the database automatic cleanup process is enabled.
-  **off** indicates that the database automatic cleanup process is disabled.

**Default value**: **on**

.. note::

   Set **autovacuum** to **on** if you want to enable the function of automatically cleaning up two-phase transactions after the system recovers from faults.

   -  If **autovacuum** is set to **on** and :ref:`autovacuum_max_workers <en-us_topic_0000001510283565__s502d4304994d4da5bd3cda661aab27ac>` to **0**, the **autovacuum** process will not be automatically performed and only abnormal two-phase transactions are cleaned up after the system recovers from faults.
   -  If **autovacuum** is set to **on** and the value of :ref:`autovacuum_max_workers <en-us_topic_0000001510283565__s502d4304994d4da5bd3cda661aab27ac>` is greater than **0**, the system will automatically clean up two-phase transactions and processes after recovering from faults.

.. important::

   Even if this parameter is set to **off**, the database initiates a cleanup process when transaction ID wraparound needs to be prevented. When a **CREATE DATABASE** or **DROP DATABASE** operation fails, the transaction may have been committed or rolled back on some nodes whereas some nodes are still in the prepared state. In this case, perform the following operations to manually restore the nodes:

   #. Use the gs_clean tool (setting the **option** parameter to **-N**) to query the xid of the abnormal two-phase transaction and nodes in the prepared status.
   #. Log in to the nodes whose transactions are in the prepared status. Administrators connect to an available database such as gaussdb to run the **SET xc_maintenance_mode = on** statement.
   #. Commit or roll back the two-phase transaction based on the global transaction status.

autovacuum_mode
---------------

**Parameter description**: Specifies whether the **autoanalyze** or **autovacuum** function is enabled. This parameter is valid only when **autovacuum** is set to **on**.

**Type**: SIGHUP

**Value range**: enumerated values

-  **analyze** indicates that only **autoanalyze** is performed.
-  **vacuum** indicates that only **autovacuum** is performed.
-  **mix** indicates that both **autoanalyze** and **autovacuum** are performed.
-  **none** indicates that neither of them is performed.

**Default value**: **mix**

autoanalyze_mode
----------------

**Parameter description**: Specifies the autoanalyze mode. This parameter is supported by version 8.2.0 or later clusters.

**Type**: USERSET

**Value range**: enumerated values

-  **normal** indicates common autoanalyze.
-  **light** indicates lightweight autoanalyze.

**Default value**:

-  If the current cluster is upgraded from an earlier version to 8.2.0, the default value is **normal** to ensure forward compatibility.
-  If the cluster version 8.2.0 is newly installed, the default value is **light**.

autoanalyze_timeout
-------------------

**Parameter description**: Specifies the timeout period of **autoanalyze**. If the duration of **analyze** on a table exceeds the value of **autoanalyze_timeout**, **analyze** is automatically canceled.

**Type**: SIGHUP

**Value range**: an integer ranging from 0 to 2147483. The unit is second.

**Default value**: **5min**

analyze_stats_mode
------------------

**Parameter description**: Specifies the mode for **ANALYZE** to calculate statistics.

**Type**: USERSET

**Value range**: enumerated values

-  **memory** indicates that the memory is forcibly used to calculate statistics. Multi-column statistics are not calculated.
-  **sample_table** indicates that temporary sampling tables are forcibly used to calculate statistics. Temporary tables do not support this mode.
-  **dynamic** indicates that the statistics calculation mode is determined based on the size of **maintenance_work_mem**. If :ref:`maintenance_work_mem <en-us_topic_0000001460563104__sfbfa78b6871442cb85a84a425335ce38>` can store samples, the memory mode is used. Otherwise, the temporary sampling table mode is used.

**Default value**:

-  If the current cluster is upgraded from an earlier version to 8.2.0.100, the default value is **memory** to ensure forward compatibility.
-  If the cluster version 8.2.0.100 is newly installed, the default value is **dynamic**.

analyze_sample_mode
-------------------

**Parameter description**: Specifies the sampling model used by **ANALYZE**.

**Type**: USERSET

**Value range**: an integer ranging from **0** to **2**

-  **0** indicates the default reservoir sampling.
-  **1** indicates the optimized reservoir sampling.
-  **2** indicates range sampling.

**Default value**: **0**

autovacuum_io_limits
--------------------

**Parameter description**: Specifies the upper limit of I/Os triggered by the **autovacuum** process per second. This parameter has been discarded in version 8.1.2 and is reserved for compatibility with earlier versions. This parameter is invalid in the current version.

**Type**: SIGHUP

**Value range**: an integer ranging from -1 to 1073741823. **-1** indicates that the default Cgroup is used.

**Default value**: **-1**

.. _en-us_topic_0000001510283565__s502d4304994d4da5bd3cda661aab27ac:

autovacuum_max_workers
----------------------

**Parameter description**: Specifies the maximum number of automatic cleanup threads running at the same time.

**Type**: SIGHUP

**Value range**: an integer ranging from 0 to 128. **0** indicates that **autovacuum** is disabled.

**Default value**: **4**

.. note::

   This parameter works with :ref:`autovacuum <en-us_topic_0000001510283565__s8d6c38309e594a16a07f79ae412b63c6>`. The rules for clearing system catalogs and user tables are as follows:

   -  When **autovacuum_max_workers** is set to **0**, **autovacuum** is disabled and no tables are cleared.
   -  If **autovacuum_max_workers > 0** and **autovacuum = off** are configured, the system only clears the system catalogs and column-store tables with delta tables enabled (such as **vacuum delta tables**, **vacuum cudesc tables**, and **delta merge**).
   -  When **autovacuum_max_workers** is set to a value greater than zero and **autovacuum** is enabled, all tables will be cleared.

autovacuum_naptime
------------------

**Parameter description**: Specifies the interval between two automatic cleanup operations.

**Type**: SIGHUP

**Value range**: an integer ranging from 1 to 2147483. The unit is second.

**Default value**: **60s**

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

**Default value**:

-  If the current cluster is upgraded from an earlier version to 8.1.3, the default value is **10000** to ensure forward compatibility.
-  If the current cluster version is 8.1.3, the default value is **50**.

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

**Default value**:

-  If the current cluster is upgraded from an earlier version to 8.1.3, the default value is **0.25** to ensure forward compatibility.
-  If the current cluster version is 8.1.3, the default value is **0.1**.

.. _en-us_topic_0000001510283565__s60e0fbc2967c44b3bb6c53c29e9c772e:

autovacuum_freeze_max_age
-------------------------

**Parameter description**: Specifies the maximum age (in transactions) that a table's **pg_class.relfrozenxid** column can attain before a VACUUM operation is forced to prevent transaction ID wraparound within the table.

The old files under the subdirectory of **pg_clog/** can also be deleted by the VACUUM operation. Even if the automatic cleanup process is forbidden, the system will invoke the automatic cleanup process to prevent the cyclic repetition.

**Type**: SIGHUP

**Value range**: an integer ranging from 100000 to 576460752303423487

**Default value**: **4000000000**

autovacuum_vacuum_cost_delay
----------------------------

**Parameter description**: Specifies the value of the cost delay used in the **autovacuum** operation.

**Type**: SIGHUP

**Value range**: an integer ranging from -1 to 100. The unit is ms. **-1** indicates that the normal vacuum cost delay is used.

**Default value**: **2ms**

autovacuum_vacuum_cost_limit
----------------------------

**Parameter description**: Specifies the value of the cost limit used in the **autovacuum** operation.

**Type**: SIGHUP

**Value range**: an integer ranging from -1 to 10000. **-1** indicates that the normal vacuum cost limit is used.

**Default value**: **-1**

.. _en-us_topic_0000001510283565__section4328534144311:

enable_pg_stat_object
---------------------

**Parameter description**: Specifies whether **AUTO VACUUM** updates the :ref:`PG_STAT_OBJECT <dws_04_1062>` system catalog. This parameter is supported by version 8.2.1 or later clusters.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that the **PG_STAT_OBJECT** system catalog is updated during **AUTO VACUUM**.
-  **off** indicates that the **PG_STAT_OBJECT** system catalog is not updated during **AUTO VACUUM**.

**Default value**: **on**
