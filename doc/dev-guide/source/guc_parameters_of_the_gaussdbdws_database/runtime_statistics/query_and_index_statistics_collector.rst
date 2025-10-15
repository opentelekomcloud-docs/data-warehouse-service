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

.. _en-us_topic_0000001764650352__s4682d08468f84845bfdc6ae9477126e8:

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

**Value range**: an integer ranging from 0 to 1440, in minutes.

**Default value**: **30min**

enable_save_datachanged_timestamp
---------------------------------

**Parameter description**: Specifies whether to record the time when **INSERT**, **UPDATE**, **DELETE**, or **EXCHANGE**/**TRUNCATE**/**DROP** **PARTITION** is performed on table data.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that the time when an operation is performed on table data will be recorded.
-  **off** indicates that the time when an operation is performed on table data will not be recorded.

**Default value**: on

enable_save_dataaccess_timestamp
--------------------------------

**Parameter description**: Specifies whether to record the last access time of a table. This parameter is supported only by 8.2.1.210 and later cluster versions.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that the last access time of the table is recorded.
-  **off** indicates that the last access time of the table is not recorded.

**Default value**: **off**

instr_unique_sql_count
----------------------

**Parameter description**: Specifies whether to collect Unique SQL statements and the maximum number allowed.

**Type**: SIGHUP

**Value range**: an integer ranging from 0 to INT_MAX

-  If it is set to **0**, Unique SQL statistics are not collected.
-  If the value is greater than **0**, the number of Unique SQL statements collected on the CN cannot exceed the value of this parameter. When the number of collected Unique SQL statements reaches the upper limit, the collection is stopped. In this case, you can increase the value of **reload** to continue the collection.

**Default value**: **0**

.. caution::

   If a new value is smaller than the original value, the Unique SQL statistics collected on the CN will be cleared.

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

enable_parallel_analyze
-----------------------

**Parameter description**: Specifies whether to use parallel sampling for internal and foreign table analysis. This parameter is supported only by clusters of version 9.1.0 or later.

**Type**: USERSET

**Value range**: Boolean

-  **true** indicates that parallel sampling is used for internal and foreign table analysis.
-  **false** indicates that parallel sampling is not used for internal and foreign table analysis.

**Default value**: **true**

.. caution::

   -  When **enable_parallel_analyze** is set to **true** and analyzing foreign tables, try to avoid adding NOT NULL constraints to the target foreign table columns to prevent constraint failure due to data source changes. Currently, parallel sampling does not support materialized views. If analyze fails due to such reasons, set this parameter to **false**.
   -  Currently, parallel sampling only supports analyzing ordinary column-store internal tables. This optimization does not take effect when the internal table uses hstore/hstore_opt or is declared as a replicated table.
   -  Currently, parallel sampling only supports analyzing foreign tables stored in parquet/orc format. This optimization does not take effect when the foreign table is in another format.

parallel_analyze_workers
------------------------

**Parameter description**: Specifies the number of concurrent threads for parallel analyze sampling. This parameter is supported only by clusters of version 9.1.0 or later.

**Type**: USERSET

**Value range**: an integer ranging from 0 to 64

**Default value**: **10**

.. note::

   The value of this parameter should correspond to the cluster load. When the cluster load is low, you can increase the parameter value appropriately based on the cluster configuration to further improve the efficiency of analyze execution.

analyze_sample_multiplier
-------------------------

**Parameter description**: Specifies the multiplier for the stripe sampling rate used in analyzing foreign tables. This parameter is supported only by clusters of version 9.1.0 or later.

**Type**: SUSET

**Value range**: an integer ranging from 0 to 100. **0** indicates that the stripe sampling rate is 100%.

**Default value**: **3**
