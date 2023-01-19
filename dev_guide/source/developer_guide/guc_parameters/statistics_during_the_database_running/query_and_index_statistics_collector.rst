:original_name: dws_04_0920.html

.. _dws_04_0920:

Query and Index Statistics Collector
====================================

The query and index statistics collector is used to collect statistics during database running. The statistics include the times of inserting and updating a table and an index, the number of disk blocks and tuples, and the time required for the last cleanup and analysis on each table. The statistics can be viewed by querying system view families pg_stats and pg_statistic. The following parameters are used to set the statistics collection feature in the server scope.

track_activities
----------------

**Parameter description**: Collects statistics about the commands that are being executed in session.

**Type**: SUSET

**Value range**: Boolean

-  **on** indicates that the statistics collection function is enabled.
-  **off** indicates that the statistics collection function is disabled.

**Default value**: **on**

.. _en-us_topic_0000001098974554__s4682d08468f84845bfdc6ae9477126e8:

track_counts
------------

**Parameter description**: Collects statistics about data activities.

**Type**: SUSET

**Value range**: Boolean

-  **on** indicates that the statistics collection function is enabled.
-  **off** indicates that the statistics collection function is disabled.

.. note::

   When the database to be cleaned up is selected from the AutoVacuum automatic cleanup process, the database statistics are required. In this case, the default value is set to **on**.

**Default value**: **on**

track_io_timing
---------------

**Parameter description**: Collects statistics about I/O invoking timing in the database. The I/O timing statistics can be queried by using the **pg_stat_database** parameter.

**Type**: SUSET

**Value range**: Boolean

-  If this parameter is set to **on**, the collection function is enabled. In this case, the collector repeatedly queries the OS at the current time. As a result, large numbers of costs may occur on some platforms. Therefore, the default value is set to **off**.
-  **off** indicates that the statistics collection function is disabled.

**Default value**: **off**

track_functions
---------------

**Parameter description**: Collects statistics about invoking times and duration in a function.

**Type**: SUSET

.. important::

   When the SQL functions are set to inline functions queried by the invoking, these SQL functions cannot be traced no matter these functions are set or not.

**Value range**: enumerated values

-  **pl** indicates that only procedural language functions are traced.
-  **all** indicates that SQL and C language functions are traced.
-  **none** indicates that the function tracing function is disabled.

**Default value**: none

track_activity_query_size
-------------------------

**Parameter description**: Specifies byte counts of the current running commands used to trace each active session.

**Type**: POSTMASTER

**Value range**: an integer ranging from 100 to 102400

**Default value**: **1024**

update_process_title
--------------------

**Parameter description:** Collects statistics updated with a process name each time the server receives a new SQL statement.

The process name can be viewed on Windows task manager by running the **ps** command.

**Type**: SUSET

**Value range**: Boolean

-  **on** indicates that the statistics collection function is enabled.
-  **off** indicates that the statistics collection function is disabled.

**Default value**: **off**

track_thread_wait_status_interval
---------------------------------

**Parameter description**: Specifies the interval of collecting the thread status information periodically.

**Type**: SUSET

**Value range**: an integer ranging from 0 to 1440. The unit is minute (min).

**Default value**: **30min**

enable_save_datachanged_timestamp
---------------------------------

**Parameter description**: Specifies whether to record the time when **INSERT**, **UPDATE**, **DELETE**, or **EXCHANGE**/**TRUNCATE**/**DROP** **PARTITION** is performed on table data.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that the time when an operation is performed on table data will be recorded.
-  **off** indicates that the time when an operation is performed on table data will not be recorded.

**Default value**: on

instr_unique_sql_count
----------------------

**Parameter description**: Specifies whether to collect Unique SQL statements and the maximum number of unique SQL statements that can be collected.

**Type**: SIGHUP

**Value range**: an integer ranging from 0 to INT_MAX

-  If it is set to **0**, Unique SQL statistics are not collected.
-  If the value is greater than **0**, the number of Unique SQL statements collected on the CN cannot exceed the value of this parameter. When the number of collected unique SQL statements reaches the upper limit, new unique SQL statements are not collected. In this case, you can increase the value of **reload** to continue collecting new unique SQL statements.

**Default value**: **0**

.. caution::

   If a new value is loaded using **reload** and the new value is less than the original value, the Unique SQL statistics collected by the corresponding CN will be cleared. Note that the clearing operation is performed by the background thread of the resource management module. If the GUC parameter :ref:`use_workload_manager <en-us_topic_0000001145694507__sc1692143c357427cbeadd6160010fd40>` is set to **off**, the clearing operation may fail. In this case, you can use the **reset_instr_unique_sql** function for clearing.

track_sql_count
---------------

**Parameter description**: Specifies whether to collect statistics on the number of the **SELECT**, **INSERT**, **UPDATE**, **DELETE**, and **MERGE INTO** statements that are being executed in each session, the response time of the **SELECT**, **INSERT**, **UPDATE**, and **DELETE** statements, and the number of DDL, DML, and DCL statements.

**Type**: SUSET

**Value range**: Boolean

-  **on** indicates that the statistics collection function is enabled.
-  **off** indicates that the statistics collection function is disabled.

**Default value**: **on**

.. note::

   -  The **track_sql_count** parameter is restricted by the **track_activities** parameter.

      -  If **track_activities** is set to **on** and **track_sql_count** is set to **off**, a warning message indicating that **track_sql_count** is disabled will be displayed when the view **gs_sql_count**, **pgxc_sql_count**, **gs_workload_sql_count**, **pgxc_workload_sql_count**, **global_workload_sql_count**, **gs_workload_sql_elapse_time**, **pgxc_workload_sql_elapse_time**, or **global_workload_sql_elapse_time** are queried.
      -  If both **track_activities** and **track_sql_count** are set to **off**, two logs indicating that **track_activities** is disabled and **track_sql_count** is disabled will be displayed when the views are queried.
      -  If **track_activities** is set to **off** and **track_sql_count** is set to **on**, a log indicating that **track_activities** is disabled will be displayed when the views are queried.

   -  If this parameter is disabled, querying the view returns **0**.

enable_track_wait_event
-----------------------

**Parameter description**: Specifies whether to collect statistics on waiting events, including the number of occurrence times, number of failures, duration, maximum waiting time, minimum waiting time, and average waiting time.

**Type**: SIGHUP

**Value range**: Boolean

-  **on** indicates that the statistics collection function is enabled.
-  **off** indicates that the statistics collection function is disabled.

**Default value**: **off**

.. note::

   -  The **enable_track_wait_event** parameter is restricted by **track_activities**. Its functions cannot take effect no matter whether it is enabled if **track_activities** is disabled.
   -  When **track_activities** or **enable_track_wait_event** is disabled, if you query the **get_instr_wait_event** function, **gs_wait_events** view, or **pgxc_wait_events** view, a message is displayed indicating that the GUC parameter is disabled and the query result is 0.
   -  If **track_activities** or **enable_track_wait_event** is disabled during cluster running, GaussDB(DWS) will not collect statistics on waiting events. However, statistics that have been collected are not affected.

enable_wdr_snapshot
-------------------

**Parameter description**: Specifies whether to enable the performance view snapshot function. After this function is enabled, GaussDB(DWS) will periodically create snapshots for some system performance views and save them permanently. In addition, it will accept manual snapshot creation requests.

**Type**: SIGHUP

**Value range**: Boolean

-  **on** indicates that the snapshot function is enabled.
-  **off** indicates that the snapshot function is disabled.

**Default value**: **off**

.. note::

   -  If the **create_wdr_snapshot** function is executed to manually create a view when the **enable_wdr_snapshot** parameter is disabled, a message is displayed indicating that the GUC parameter is not enabled.
   -  If the **enable_wdr_snapshot** parameter is modified during the snapshot creation process, the snapshot that is being created is not affected. The modification takes effect when the snapshot is manually or periodically created next time.

wdr_snapshot_interval
---------------------

**Parameter description**: Specifies the interval for automatically creating performance view snapshots.

**Type**: SIGHUP

**Value range**: an integer ranging from 10 to 180, in minutes

**Default value**: **60**

.. note::

   -  The value of this parameter must be set in accordance with the cluster load. You are advised to set this parameter to a value greater than the time required for creating a snapshot.
   -  If the value of **wdr_snapshot_interval** is less than the time required for creating a snapshot, the system will skip this snapshot creation because it finds that the previous snapshot creation is not complete when the time for this automatic snapshot creation arrives.

wdr_snapshot_retention_days
---------------------------

**Parameter description**: Specifies the maximum number of days for storing performance snapshot data.

**Type**: SIGHUP

**Value range**: an integer ranging from 1 to 15 days

**Default value**: **8**

.. note::

   -  If **enable_wdr_snapshot** is enabled, snapshot data that has been stored for **wdr_snapshot_retention_days** days will be automatically deleted.
   -  The value of this parameter must be set in accordance with the available disk space. A larger value requires more disk space.
   -  The modification of this parameter does not take effect immediately. The expired snapshot data will be cleared only when a snapshot is automatically created next time.
