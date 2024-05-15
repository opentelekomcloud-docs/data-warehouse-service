:original_name: dws_06_0065.html

.. _dws_06_0065:

Statistics Information Functions
================================

Statistics functions are classified into the following types based on the objects:

-  Functions used to access a database. Table OIDs and indexes in the database can be used to identify the database.
-  Functions used to accesses a server. The server process ID is used as a parameter. The value ranges from 1 to the number of active servers.

pg_stat_get_db_numbackends(oid)
-------------------------------

Description: Obtains the number of active server threads of a specified database on the current instance.

Return type: integer

pg_stat_get_db_total_numbackends(oid)
-------------------------------------

Description: Obtains the total number of active server threads of a specified database on all CNs in a cluster (if this function is executed on a CN), or obtains the number of active server threads of a specified database on the current instance (if this function is executed on a DN).

Return type: integer

pg_stat_get_db_xact_commit(oid)
-------------------------------

Description: Obtains the number of committed transactions in a specified database on the current instance.

Return type: bigint

pg_stat_get_db_total_xact_commit(oid)
-------------------------------------

Description: Obtains the total number of committed transactions in a specified database on all CNs in a cluster (if this function is executed on a CN), or obtains the number of committed transactions in a specified database on the current instance (if this function is executed on a DN).

Return type: bigint

pg_stat_get_db_xact_rollback(oid)
---------------------------------

Description: Obtains the number of rollback transactions in a specified database on the current instance.

Return type: bigint

pg_stat_get_db_total_xact_rollback(oid)
---------------------------------------

Description: Obtains the total number of rollback transactions in a specified database on all CNs in a cluster (if this function is executed on a CN), or obtains the number of rollback transactions in a specified database on the current instance (if this function is executed on a DN).

Return type: bigint

pg_stat_get_db_blocks_fetched(oid)
----------------------------------

Description: Obtains the number of disk block fetch requests in a specified database on the current instance.

Return type: bigint

pg_stat_get_db_total_blocks_fetched(oid)
----------------------------------------

Description: Obtains the total number of disk block fetch requests in a specified database on all DNs in a cluster (if this function is executed on a CN), or obtains the number of disk block fetch requests in a specified database on the current instance (if this function is executed on a DN).

Return type: bigint

pg_stat_get_db_blocks_hit(oid)
------------------------------

Description: Obtains the number of requested disk blocks found in the cache in a specified database on the current instance.

Return type: bigint

pg_stat_get_db_total_blocks_hit(oid)
------------------------------------

Description: Obtains the total number of requested disk blocks found in the cache in a specified database on all DNs in a cluster (if this function is executed on a CN), or obtains the number of requested disk blocks found in the cache in a specified database on the current instance (if this function is executed on a DN).

Return type: bigint

pg_stat_get_db_tuples_returned(oid)
-----------------------------------

Description: Obtains the number of tuples returned for a specified database on the current instance.

Return type: bigint

pg_stat_get_db_total_tuples_returned(oid)
-----------------------------------------

Description: Obtains the total number of tuples returned for a specified database on all DNs in a cluster (if this function is executed on a CN), or obtains the number of tuples returned for a specified database on the current instance (if this function is executed on a DN).

Return type: bigint

pg_stat_get_db_tuples_fetched(oid)
----------------------------------

Description: Obtains the number of tuples read from a specified database on the current instance.

Return type: bigint

pg_stat_get_db_total_tuples_fetched(oid)
----------------------------------------

Description: Obtains the total number of tuples read from a specified database on all DNs in a cluster (if this function is executed on a CN), or obtains the number of tuples read from a specified database on the current instance (if this function is executed on a DN).

Return type: bigint

pg_stat_get_db_tuples_inserted(oid)
-----------------------------------

Description: Obtains the number of tuples inserted into a specified database on the current instance.

Return type: bigint

pg_stat_get_db_total_tuples_inserted(oid)
-----------------------------------------

Description: Obtains the total number of tuples inserted into a specified database on all DNs in a cluster (if this function is executed on a CN), or obtains the number of tuples inserted into a specified database on the current instance (if this function is executed on a DN).

Return type: bigint

pg_stat_get_db_tuples_updated(oid)
----------------------------------

Description: Obtains the number of updated tuples in a specified database on the current instance.

Return type: bigint

pg_stat_get_db_total_tuples_updated(oid)
----------------------------------------

Description: Obtains the total number of updated tuples in a specified database on all DNs in a cluster (if this function is executed on a CN), or obtains the number of updated tuples in a specified database on the current instance (if this function is executed on a DN).

Return type: bigint

pg_stat_get_db_tuples_deleted(oid)
----------------------------------

Description: Obtains the number of tuples deleted from a specified database on the current instance.

Return type: bigint

pg_stat_get_db_total_tuples_deleted(oid)
----------------------------------------

Description: Obtains the total number of tuples deleted from a specified database on all DNs in a cluster (if this function is executed on a CN), or obtains the number of tuples deleted from a specified database on the current instance (if this function is executed on a DN).

Return type: bigint

pg_stat_get_db_conflict_lock(oid)
---------------------------------

Description: Obtains the total number of conflicting locks in a specified database on all CNs and DNs in a cluster (if this function is executed on a CN), or obtains the number of conflicting locks in a specified database on the current instance (if this function is executed on a DN).

Return type: bigint

pg_stat_get_db_deadlocks(oid)
-----------------------------

Description: Obtains the number of deadlocks in a specified database on the current instance.

Return type: bigint

pg_stat_get_db_total_deadlocks(oid)
-----------------------------------

Description: Obtains the total number of deadlocks in a specified database on all CNs and DNs in a cluster (if this function is executed on a CN), or obtains the number of deadlocks in a specified database on the current instance (if this function is executed on a DN).

Return type: bigint

pg_stat_get_db_conflict_all(oid)
--------------------------------

Description: Obtains the number of conflict recoveries in a specified database on the current instance.

Return type: bigint

pg_stat_get_db_total_conflict_all(oid)
--------------------------------------

Description: Obtains the total number of conflict recoveries in a specified database on all CNs and DNs in a cluster (if this function is executed on a CN), or obtains the number of conflict recoveries in a specified database on the current instance (if this function is executed on a DN).

Return type: bigint

pg_stat_get_db_temp_files(oid)
------------------------------

Description: Obtains the number of temporary files created in a specified database on the current instance.

Return type: bigint

pg_stat_get_db_total_temp_files(oid)
------------------------------------

Description: Obtains the total number of temporary files created in a specified database on all DNs in a cluster (if this function is executed on a CN), or obtains the number of temporary files created in a specified database on the current instance (if this function is executed on a DN).

Return type: bigint

pg_stat_get_db_temp_bytes(oid)
------------------------------

Description: Obtains the number of bytes of the temporary files created in a specified database on the current instance.

Return type: bigint

pg_stat_get_db_total_temp_bytes(oid)
------------------------------------

Description: Obtains the total number of bytes of the temporary files created in a specified database on all DNs in a cluster (if this function is executed on a CN), or obtains the number of bytes of the temporary files created in a specified database on the current instance (if this function is executed on a DN).

Return type: bigint

pg_stat_get_db_blk_read_time(oid)
---------------------------------

Description: Obtains the time required for reading data blocks from a specified database on the current instance.

Return type: double

pg_stat_get_db_total_blk_read_time(oid)
---------------------------------------

Description: Obtains the total time required for reading data blocks from a specified database on all DNs in a cluster (if this function is executed on a CN), or obtains the time required for reading data blocks from a specified database on the current instance (if this function is executed on a DN).

Return type: double

pg_stat_get_db_blk_write_time(oid)
----------------------------------

Description: Obtains the time required for writing data blocks to a specified database on the current instance.

Return type: double

pg_stat_get_db_total_blk_write_time(oid)
----------------------------------------

Description: Obtains the total time required for writing data blocks to a specified database on all DNs in a cluster (if this function is executed on a CN), or obtains the time required for writing data blocks to a specified database on the current instance (if this function is executed on a DN).

Return type: double

pg_stat_get_numscans(oid)
-------------------------

Description: Number of sequential row scans done if parameters are in a table

or number of index scans done if parameters are in an index

Return type: bigint

.. _en-us_topic_0000001188270492__section4444448123716:

pg_stat_get_tuple()
-------------------

Description: This function can be executed on both CNs and DNs. This function is supported only by version 8.1.3 or later clusters.

If no parameters are specified, this function queries the statistics of all system catalogs on CNs, the dirty page rate of the tables on each CN, the statistics of all system catalogs and user catalogs on DNs, and the dirty page rate of the tables on each DN.

If the schema name and table name are specified, this function queries the statistics and dirty page rate of the specified table.

.. note::

   The statistics of this function depend on the **ANALYZE** operation. To obtain the most accurate information, perform the **ANALYZE** operation on the table first.

Return type: record

The following table describes return columns.

.. table:: **Table 1** pg_stat_get_tuple() return fields

   +-------------------+--------------------------+--------------------------------------------+
   | Name              | Type                     | Description                                |
   +===================+==========================+============================================+
   | nodename          | text                     | Node name                                  |
   +-------------------+--------------------------+--------------------------------------------+
   | tableid           | oid                      | Table OID                                  |
   +-------------------+--------------------------+--------------------------------------------+
   | partid            | oid                      | Partition OID of the partitioned table     |
   +-------------------+--------------------------+--------------------------------------------+
   | last_vacuum       | timestamp with time zone | Time of the last manual **VACUUM**         |
   +-------------------+--------------------------+--------------------------------------------+
   | last_autovacuum   | timestamp with time zone | Time of the last **AUTOVACUUM**            |
   +-------------------+--------------------------+--------------------------------------------+
   | last_analyze      | timestamp with time zone | Time of the last manual **ANALYZE**        |
   +-------------------+--------------------------+--------------------------------------------+
   | last_autoanalyze  | timestamp with time zone | Time of the last **AUTOANALYZE**           |
   +-------------------+--------------------------+--------------------------------------------+
   | vacuum_count      | bigint                   | Number of times **VACUUM** operations      |
   +-------------------+--------------------------+--------------------------------------------+
   | autovacuum_count  | bigint                   | Number of **AUTOVACUUM** operations        |
   +-------------------+--------------------------+--------------------------------------------+
   | analyze_count     | bigint                   | Number of **ANALYZE** operations           |
   +-------------------+--------------------------+--------------------------------------------+
   | autoanalyze_count | bigint                   | Number of **AUTOANALYZE_COUNT** operations |
   +-------------------+--------------------------+--------------------------------------------+
   | n_tup_ins         | bigint                   | Number of rows inserted                    |
   +-------------------+--------------------------+--------------------------------------------+
   | n_tup_upd         | bigint                   | Number of rows updated                     |
   +-------------------+--------------------------+--------------------------------------------+
   | n_tup_del         | bigint                   | Number of rows deleted                     |
   +-------------------+--------------------------+--------------------------------------------+
   | n_tup_hot_upd     | bigint                   | Number of rows with HOT updates            |
   +-------------------+--------------------------+--------------------------------------------+
   | n_tup_change      | bigint                   | Number of changed rows after **ANALYZE**   |
   +-------------------+--------------------------+--------------------------------------------+
   | n_live_tup        | bigint                   | Estimated number of live rows              |
   +-------------------+--------------------------+--------------------------------------------+
   | n_dead_tup        | bigint                   | Estimated number of dead rows              |
   +-------------------+--------------------------+--------------------------------------------+
   | dirty_rate        | bigint                   | Dirty page rate of a single CN or DN       |
   +-------------------+--------------------------+--------------------------------------------+
   | last_data_changed | timestamp with time zone | Time when a table was last modified        |
   +-------------------+--------------------------+--------------------------------------------+

pg_stat_get_tuples_returned(oid)
--------------------------------

Description: Number of sequential row scans done if parameters are in a table

or number of index entries returned if parameters are in an index

Return type: bigint

pg_stat_get_tuples_fetched(oid)
-------------------------------

Description: Number of table rows fetched by bitmap scans if parameters are in a table,

or table rows fetched by simple index scans using the index if parameters are in an index

Return type: bigint

pg_stat_get_tuples_inserted(oid)
--------------------------------

Description: Number of rows inserted into table

Return type: bigint

pg_stat_get_local_tuples_inserted(oid)
--------------------------------------

Description: Number of rows inserted into the table on the current node. This function is supported only in 8.1.2 or later.

Return type: bigint

pg_stat_get_tuples_updated(oid)
-------------------------------

Description: Number of rows updated in table

Return type: bigint

pg_stat_get_local_tuples_updated(oid)
-------------------------------------

Description: Number of rows updated in the table on the current node. This function is supported only in 8.1.2 or later.

Return type: bigint

pg_stat_get_tuples_deleted(oid)
-------------------------------

Description: Number of rows deleted from table

Return type: bigint

pg_stat_get_local_tuples_deleted(oid)
-------------------------------------

Description: Number of rows deleted from the table on the current node. This function is supported only in 8.1.2 or later.

Return type: bigint

pg_stat_get_tuples_changed(oid)
-------------------------------

Description: Queries the function on a CN and returns the total number of inserted, updated, and deleted rows in the table since the last **ANALYZE** or **AUTOANALYZE** operation. Queries the function on a DN and returns the total number of inserted, updated, and deleted rows in the table since the last **ANALYZE** or **AUTOANALYZE** operation on the current node.

Return type: bigint

pg_stat_get_local_tuples_changed(oid)
-------------------------------------

Description: Number of inserted, updated, and deleted rows in the table since the last **ANALYZE** or **AUTOANALYZE** operation on the current node.

Return type: bigint

pg_stat_get_tuples_hot_updated(oid)
-----------------------------------

Description: Number of rows HOT-updated in table

Return type: bigint

pg_stat_get_local_tuples_hot_updated(oid)
-----------------------------------------

Description: Number of rows with HOT updates in the table on the current node. This function is supported only in 8.1.2 or later.

Return type: bigint

pg_stat_get_live_tuples(oid)
----------------------------

Description: Number of live tuples in the table.

Return type: bigint

pg_stat_get_local_live_tuples(oid)
----------------------------------

Description: Number of live tuples in the table on the current node. This function is supported only in 8.1.2 or later.

Return type: bigint

pg_stat_get_dead_tuples(oid)
----------------------------

Description: Number of dead tuples in the table.

Return type: bigint

pg_stat_get_local_dead_tuples(oid)
----------------------------------

Description: Number of dead tuples in the table on the current node. This function is supported only in 8.1.2 or later.

Return type: bigint

pg_stat_get_blocks_fetched(oid)
-------------------------------

Description: Number of disk block fetch requests for table or index

Return type: bigint

pg_stat_get_blocks_hit(oid)
---------------------------

Description: Number of disk block requests found in cache for table or index

Return type: bigint

pg_stat_get_partition_tuples_inserted(oid)
------------------------------------------

Description: Number of rows in the corresponding table partition

Return type: bigint

pg_stat_get_partition_tuples_updated(oid)
-----------------------------------------

Description: Number of rows that have been updated in the corresponding table partition

Return type: bigint

pg_stat_get_partition_tuples_deleted(oid)
-----------------------------------------

Description: Number of rows deleted from the corresponding table partition

Return type: bigint

pg_stat_get_partition_tuples_changed(oid)
-----------------------------------------

Description: Total number of inserted, updated, and deleted rows after the table partition was last analyzed or autoanalyzed

Return type: bigint

pg_stat_get_partition_live_tuples(oid)
--------------------------------------

Description: Number of live rows in a table partition

Return type: bigint

pg_stat_get_partition_dead_tuples(oid)
--------------------------------------

Description: Number of dead rows in a table partition

Return type: bigint

pg_stat_get_xact_tuples_inserted(oid)
-------------------------------------

Description: Number of tuple inserted into the active subtransactions related to the table.

Return type: bigint

pg_stat_get_xact_tuples_deleted(oid)
------------------------------------

Description: Number of deleted tuples in the active subtransactions related to a table

Return type: bigint

pg_stat_get_xact_tuples_hot_updated(oid)
----------------------------------------

Description: Number of hot updated tuples in the active subtransactions related to a table

Return type: bigint

pg_stat_get_xact_tuples_updated(oid)
------------------------------------

Description: Number of updated tuples in the active subtransactions related to a table

Return type: bigint

pg_stat_get_xact_partition_tuples_inserted(oid)
-----------------------------------------------

Description: Number of inserted tuples in the active subtransactions related to a table partition

Return type: bigint

pg_stat_get_xact_partition_tuples_deleted(oid)
----------------------------------------------

Description: Number of deleted tuples in the active subtransactions related to a table partition

Return type: bigint

pg_stat_get_xact_partition_tuples_hot_updated(oid)
--------------------------------------------------

Description: Number of hot updated tuples in the active subtransactions related to a table partition

Return type: bigint

pg_stat_get_xact_partition_tuples_updated(oid)
----------------------------------------------

Description: Number of updated tuples in the active subtransactions related to a table partition

Return type: bigint

pg_stat_get_last_vacuum_time(oid)
---------------------------------

Description: Last time when the autovacuum thread is manually started to clear a table

Return type: timestamptz

pg_stat_get_last_autovacuum_time(oid)
-------------------------------------

Description: Time of the last vacuum initiated by the autovacuum thread on this table

Return type: timestamptz

pg_stat_get_local_last_autovacuum_time(oid)
-------------------------------------------

Description: Time of the last vacuum initiated by the autovacuum thread of the current node on this table. This function is supported only in 8.1.2 or later.

Return type: timestamptz

pg_stat_get_vacuum_count(oid)
-----------------------------

Description: Number of times a table is manually cleared

Return type: bigint

pg_stat_get_autovacuum_count(oid)
---------------------------------

Description: Number of times of vacuum initiated by the autovacuum thread on this table

Return type: bigint

pg_stat_get_local_autovacuum_count(oid)
---------------------------------------

Description: Number of times of vacuum initiated by the autovacuum thread of the current node on this table. This function is supported only in 8.1.2 or later.

Return type: bigint

pg_stat_get_last_analyze_time(oid)
----------------------------------

Description: Last time when a table starts to be analyzed manually or by the autovacuum thread

Return type: timestamptz

pg_stat_get_last_autoanalyze_time(oid)
--------------------------------------

Description: Time of the last analysis initiated by the autovacuum thread on this table

Return type: timestamptz

pg_stat_get_local_last_autoanalyze_time(oid)
--------------------------------------------

Description: Time of the last analysis initiated by the autovacuum thread of the current node on this table. This function is supported only in 8.1.2 or later.

Return type: timestamptz

pg_stat_get_analyze_count(oid)
------------------------------

Description: Number of times a table is manually analyzed

Return type: bigint

pg_stat_get_autoanalyze_count(oid)
----------------------------------

Description: Number of times the autovacuum daemon analyzes a table

Return type: bigint

pg_stat_get_local_autoanalyze_count(oid)
----------------------------------------

Description: Number of times that the autovacuum daemon of the current node starts analysis on this table. This function is supported only in 8.1.2 or later.

Return type: bigint

pg_stat_get_local_analyze_status(oid)
-------------------------------------

Description: Specifies whether to analyze the status of a table on the current node. This parameter is valid only for CNs. This function is supported only in 8.1.2 or later.

-  If the number of modified rows in the table exceeds the ANALYZE threshold (calculated based on autovacuum_analyze_threshold + autovacuum_analyze_scale_factor x reltuples, where **reltuples** is the estimated number of rows in the table recorded in **pg_class**), **Analyze needed** is returned.
-  If the number of modified rows in the table does not exceed the threshold of **analyze**, the message **Analyze not needed** is returned.
-  If the table is being analyzed, the message **Analyze in progress** is returned.
-  If whether to analyze the table is unknown, the message **Unknown analyze status** is returned.

Return type: text

pg_total_autovac_tuples(bool)
-----------------------------

Description: Gets the tuple records related to **total autovac**, such as **nodename**, **nspname**, **relname**, and the IUD information of tuples.

Return type: SETOF record

pg_autovac_status(oid)

Description: Returns autovac information, such as **nodename**, **nspname**, **relname**, **analyze**, **vacuum**, thresholds of **analyze** and **vacuum**, and the number of analyzed or vacuumed tuples.

Return type: SETOF record

pg_autovac_timeout(oid)
-----------------------

Description: Returns the number of consecutive timeouts during the autovac operation on a table. If the table information is invalid or the node information is abnormal, **NULL** will be returned.

Return type: bigint

pg_autovac_coordinator(oid)
---------------------------

Description: Returns the name of the CN performing the autovac operation on a table. If the table information is invalid or the node information is abnormal, **NULL** will be returned.

Return type: text

pgxc_get_wlm_session_info_bytime(text, timestamp without time zone, timestamp without time zone, int)
-----------------------------------------------------------------------------------------------------

Description: The query performance of the PGXC_WLM_SESSION_INFO view is poor if the view contains a large number of records. In this case, you are advised to use this function to filter the query. The input parameters are *time column* (**start_time** or **finish_time**), *start time*, *end time*, and *maximum number of records returned for each CN*. The return result is a subset of records in the **GS_WLM_SESSION_HISTORY** view.

Return type: SETOF record

pgxc_get_wlm_current_instance_info(text, int default null)
----------------------------------------------------------

Description: Queries the current resource usage of each node in the cluster on the CN and reads the data that is not stored in the GS_WLM_INSTANCE_HISTORY system catalog in the memory. The input parameters are the node name (**ALL**, **C**, **D**, or *instance name*) and the maximum number of records returned by each node. The returned value is **GS_WLM_INSTANCE_HISTORY**.

Return type: SETOF record

pgxc_get_wlm_history_instance_info(text, TIMESTAMP, TIMESTAMP, int default null)
--------------------------------------------------------------------------------

Description: Queries the historical resource usage of each cluster node on the CN node and reads data from the **GS_WLM_INSTANCE_HISTORY** system catalog. The input parameters are as follows: node name (**ALL**, **C**, **D**, or *instance name*), start time, end time, and maximum number of records returned for each instance. The returned value is **GS_WLM_INSTANCE_HISTORY**.

Return type: SETOF record

pg_stat_get_last_data_changed_time(oid)
---------------------------------------

Description: Returns the time when **INSERT**, **UPDATE**, **DELETE**, or **EXCHANGE**/**DROP** **PARTITION** was performed last time on a table. The data in the **last_data_changed** column of the PG_STAT_ALL_TABLES view is calculated by using this function. The performance of obtaining the last modification time by using the view is poor when the table has a large amount of data. In this case, you are advised to use the function.

Return type: timestamptz

pg_stat_set_last_data_changed_time(oid)
---------------------------------------

Description: Manually changes the time when **INSERT**, **UPDATE**, **DELETE**, or **EXCHANGE**/**TRUNCATE**/**DROP** **PARTITION** was performed last time.

Return type: void

pv_session_time()
-----------------

Description: Collects statistics on the running time of each session thread on the current node and the time consumed in each execution phase.

Return type: record

pv_instance_time()
------------------

Description: Collects statistics on the running time of the current node and the time consumed in each execution phase.

Return type: record

pg_stat_get_activity(integer)
-----------------------------

Description: Returns a record about the backend with the specified PID. A record for each active backend in the system is returned if **NULL** is specified. The return result is a subset of records (excluding the **connection_info** column) in the PG_STAT_ACTIVITY view.

Return type: SETOF record

pg_stat_get_activity_with_conninfo(integer)
-------------------------------------------

Description: Returns a record about the backend with the specified PID. A record for each active backend in the system is returned if **NULL** is specified. The return result is a subset of records in the **PG_STAT_ACTIVITY** view.

Return type: SETOF record

pg_user_iostat(text)
--------------------

Description: This function has been discarded in version 8.1.2 and is reserved for compatibility with earlier versions. This function is invalid in the current version.

Return type: record

.. table:: **Table 2** pg_user_iostat(text) return fields

   +---------------+------+----------------------------------------------------------------+
   | Name          | Type | Description                                                    |
   +===============+======+================================================================+
   | userid        | oid  | User ID.                                                       |
   +---------------+------+----------------------------------------------------------------+
   | min_curr_iops | int4 | Minimum I/O of the current user across DNs.                    |
   +---------------+------+----------------------------------------------------------------+
   | max_curr_iops | int4 | Maximum I/O of the current user across DNs.                    |
   +---------------+------+----------------------------------------------------------------+
   | min_peak_iops | int4 | Minimum peak I/O of the current user across DNs.               |
   +---------------+------+----------------------------------------------------------------+
   | max_peak_iops | int4 | Maximum peak I/O of the current user across DNs.               |
   +---------------+------+----------------------------------------------------------------+
   | io_limits     | int4 | **io_limits** set for the resource pool specified by the user. |
   +---------------+------+----------------------------------------------------------------+
   | io_priority   | text | **io_priority** set for the user.                              |
   +---------------+------+----------------------------------------------------------------+

pg_stat_get_function_calls(oid)
-------------------------------

Description: Number of times the function has been called

Return type: bigint

pg_stat_get_function_total_time(oid)
------------------------------------

Description: Gets the total wall-clock time spent on a function, in microseconds. The time spent on calling this function is included.

Return type: double precision

pg_stat_get_function_self_time(oid)
-----------------------------------

Description: Gets the time spent only on this function in the current transaction. The time spent on calling this function is not included.

Return type: double precision

pg_stat_get_backend_idset()
---------------------------

Description: Set of currently active server process numbers (from 1 to the number of active server processes)

Return type: setofinteger

pg_stat_get_backend_pid(integer)
--------------------------------

Description: Thread ID of the given server thread

Return type: bigint

::

   SELECT pg_stat_get_backend_pid(1);
    pg_stat_get_backend_pid
   -------------------------
            139706243217168
   (1 row)

pg_stat_get_backend_dbid(integer)
---------------------------------

Description: ID of the database connected to the given server process

Return type: OID

pg_stat_get_backend_userid(integer)
-----------------------------------

Description: User ID of the given server process

Return type: OID

pg_stat_get_backend_activity(integer)
-------------------------------------

Description: Active command of the given server process, but only if the current user is a system administrator or the same user as that of the session being queried and **track_activities** is on

Return type: text

pg_stat_get_backend_waiting(integer)
------------------------------------

Description: True if the given server process is waiting for a lock, but only if the current user is a system administrator or the same user as that of the session being queried and **track_activities** is on

Return type: boolean

pg_stat_get_backend_activity_start(integer)
-------------------------------------------

Description: The time at which the given server process's currently executing query was started, but only if the current user is a system administrator or the same user as that of the session being queried and **track_activities** is on

Return type: timestamp with time zone

pg_stat_get_backend_xact_start(integer)
---------------------------------------

Description: The time at which the given server process's currently executing transaction was started, but only if the current user is a system administrator or the same user as that of the session being queried and **track_activities** is on

Return type: timestamp with time zone

pg_stat_get_backend_start(integer)
----------------------------------

Description: The time at which the given server process was started, or **NULL** if the current user is neither a system administrator nor the same user as that of the session being queried

Return type: timestamp with time zone

pg_stat_get_backend_client_addr(integer)
----------------------------------------

Description: IP address of the client connected to the given server process.

If the connection is over a Unix domain socket, or if the current user is neither a system administrator nor the same user as that of the session being queried, **NULL** will be returned.

Return type: inet

Note: An IP address used as an input parameter of this function cannot contain periods (.). For example, **192.168.100.128** should be written as **192168100128**.

pg_stat_get_backend_client_port(integer)
----------------------------------------

Description: TCP port number of the client connected to the given server process

If the connection is over a Unix domain socket, **-1** will be returned. If the current user is neither a system administrator nor the same user as that of the session being queried, **NULL** will be returned.

Return type: integer

pg_stat_get_bgwriter_timed_checkpoints()
----------------------------------------

Description: The number of times the background writer has started timed checkpoints (because the **checkpoint_timeout** time has expired)

Return type: bigint

pg_stat_get_bgwriter_requested_checkpoints()

Description: The number of times the background writer has started checkpoints based on requests from the backend because **checkpoint_segments** has been exceeded or the **CHECKPOINT** command has been executed

Return type: bigint

pg_stat_get_bgwriter_buf_written_checkpoints()
----------------------------------------------

Description: The number of buffers written by the background writer during checkpoints

Return type: bigint

pg_stat_get_bgwriter_buf_written_clean()
----------------------------------------

Description: The number of buffers written by the background writer for routine cleaning of dirty pages

Return type: bigint

pg_stat_get_bgwriter_maxwritten_clean()
---------------------------------------

Description: The number of times the background writer has stopped its cleaning scan because it has written more buffers than specified in the **bgwriter_lru_maxpages** parameter

Return type: bigint

pg_stat_get_buf_written_backend()
---------------------------------

Description: The number of buffers written by the backend because they needed to allocate a new buffer

Return type: bigint

pg_stat_get_buf_alloc()
-----------------------

Description: The total number of buffer allocations

Return type: bigint

pg_stat_clear_snapshot()
------------------------

Description: Discards the current statistics snapshot.

Return type: void

pg_stat_reset()
---------------

Description: Resets all statistics counters for the current database to zero (requires system administrator permissions).

Return type: void

pg_stat_reset_shared(text)
--------------------------

Description: Resets all statistics counters for the current database in each node in a shared cluster to zero (requires system administrator permissions).

Return type: void

pg_stat_reset_single_table_counters(oid)
----------------------------------------

Description: Resets statistics for a single table or index in the current database to zero (requires system administrator permissions).

Return type: void

pg_stat_reset_single_function_counters(oid)
-------------------------------------------

Description: Resets statistics for a single function in the current database to zero (requires system administrator permissions).

Return type: void

pg_stat_session_cu(int, int, int)
---------------------------------

Description: Obtains the compression unit (CU) hit statistics of sessions running on the current node.

Return type: record

gs_get_stat_session_cu(text, int, int, int)
-------------------------------------------

Description: Obtains the CU hit statistics of all sessions running in a cluster.

Return type: record

gs_get_stat_db_cu(text, text, bigint, bigint, bigint)
-----------------------------------------------------

Description: Obtains the CU hit statistics of a database in a cluster.

Return type: record

pg_stat_get_cu_mem_hit(oid)
---------------------------

Description: Obtains the number of CU memory hits of a column storage table in the current database of the current node.

Return type: bigint

pg_stat_get_cu_hdd_sync(oid)
----------------------------

Description: Obtains the number of times CU is synchronously read from a disk by a column-store table in the current database of the current node.

Return type: bigint

pg_stat_get_cu_hdd_asyn(oid)
----------------------------

Description: Obtains the number of times CU is asynchronously read from a disk by a column-store table in the current database of the current node.

Return type: bigint

pg_stat_get_db_cu_mem_hit(oid)
------------------------------

Description: Obtains the CU memory hit in a database of the current node.

Return type: bigint

pg_stat_get_db_cu_hdd_sync(oid)
-------------------------------

Description: Obtains the times CU is synchronously read from a disk by a database of the current node.

Return type: bigint

pg_stat_get_db_cu_hdd_asyn(oid)
-------------------------------

Description: Obtains the times CU is asynchronously read from a disk by a database of the current node.

Return type: bigint

pgxc_fenced_udf_process()
-------------------------

Description: Shows the number of UDF Master and Work processes.

Return type: record

pgxc_terminate_all_fenced_udf_process()
---------------------------------------

Description: Kills all UDF Work processes.

Return type: bool

GS_ALL_NODEGROUP_CONTROL_GROUP_INFO(text)
-----------------------------------------

Description: Provides Cgroup information for all logical clusters. Before invoking this function, you need to specify the name of a logical cluster to be queried. For example, to query the Cgroup information for the **installation** logical cluster, run the following command:

::

   SELECT * FROM GS_ALL_NODEGROUP_CONTROL_GROUP_INFO('installation')

Return type: record

The following table describes return columns.

.. table:: **Table 3** GS_ALL_NODEGROUP_CONTROL_GROUP_INFO(text)

   +----------+--------+----------------------------------------------------------------+
   | Name     | Type   | Description                                                    |
   +==========+========+================================================================+
   | name     | text   | Name of a Cgroup                                               |
   +----------+--------+----------------------------------------------------------------+
   | type     | text   | Type of the Cgroup                                             |
   +----------+--------+----------------------------------------------------------------+
   | gid      | bigint | Cgroup ID                                                      |
   +----------+--------+----------------------------------------------------------------+
   | classgid | bigint | ID of the **Class** Cgroup where a **Workload** Cgroup belongs |
   +----------+--------+----------------------------------------------------------------+
   | class    | text   | **Class** Cgroup                                               |
   +----------+--------+----------------------------------------------------------------+
   | workload | text   | **Workload** Cgroup                                            |
   +----------+--------+----------------------------------------------------------------+
   | shares   | bigint | CPU quota allocated to a Cgroup                                |
   +----------+--------+----------------------------------------------------------------+
   | limits   | bigint | Limit of CPUs allocated to a Cgroup                            |
   +----------+--------+----------------------------------------------------------------+
   | wdlevel  | bigint | **Workload** Cgroup level                                      |
   +----------+--------+----------------------------------------------------------------+
   | cpucores | text   | Usage of CPU cores in a Cgroup                                 |
   +----------+--------+----------------------------------------------------------------+

gs_get_nodegroup_tablecount(name)
---------------------------------

Description: Total number of user tables in all the databases in a logical cluster

Return type: integer

pgxc_max_datanode_size(name)
----------------------------

Description: Maximum disk space occupied by database files in all the DNs of a logical cluster. The unit is byte.

Return type: bigint

gs_check_logic_cluster_consistency()
------------------------------------

Description: Checks whether the system information of all logical clusters in the system is consistent. If no record is returned, the information is consistent. Otherwise, the Node Group information on CNs and DNs in the logical cluster is inconsistent. This function cannot be invoked during redistribution in a scale-in or scale-out.

Return type: record

gs_check_tables_distribution()
------------------------------

Description: Checks whether the user table distribution in the system is consistent. If no record is returned, table distribution is consistent. This function cannot be invoked during redistribution in a scale-in or scale-out.

Return type: record

pg_stat_bad_block(text, int, int, int, int, int, timestamp with time zone, timestamp with time zone)
----------------------------------------------------------------------------------------------------

Description: Obtains damage information about pages or CUs after the current node is started.

Return type: record

pgxc_stat_bad_block(text, int, int, int, int, int, timestamp with time zone, timestamp with time zone)
------------------------------------------------------------------------------------------------------

Description: Obtains damage information about pages or CUs after all the nodes in the cluster are started.

Return type: record

pg_stat_bad_block_clear()
-------------------------

Description: Deletes the page and CU damage information that is read and recorded on the node. (System administrator rights are required.)

Return type: void

pgxc_stat_bad_block_clear()
---------------------------

Description: Deletes the page and CU damage information that is read and recorded on all the nodes in the cluster. (System administrator rights are required.)

Return type: void

gs_respool_exception_info(pool text)
------------------------------------

Description: Queries for the query rule of a specified resource pool.

Return type: record

gs_control_group_info(pool text)
--------------------------------

Description: Queries for information about Cgroups associated with a resource pool.

Return type: record

The following information is displayed:

.. table:: **Table 4** gs_control_group_info(pool text) return fields

   +-----------+---------------------+---------------------------------------------------------+
   | Attribute | Value               | Description                                             |
   +===========+=====================+=========================================================+
   | name      | class_a:workload_a1 | Class name and workload name                            |
   +-----------+---------------------+---------------------------------------------------------+
   | class     | class_a             | **Class** Cgroup name                                   |
   +-----------+---------------------+---------------------------------------------------------+
   | workload  | workload_a1         | **Workload** Cgroup name                                |
   +-----------+---------------------+---------------------------------------------------------+
   | type      | DEFWD               | Cgroup type (Top, CLASS, BAKWD, DEFWD, and TSWD)        |
   +-----------+---------------------+---------------------------------------------------------+
   | gid       | 87                  | Cgroup ID                                               |
   +-----------+---------------------+---------------------------------------------------------+
   | shares    | 30                  | Percentage of CPU resources to those on the parent node |
   +-----------+---------------------+---------------------------------------------------------+
   | limits    | 0                   | Percentage of CPU cores to those on the parent node     |
   +-----------+---------------------+---------------------------------------------------------+
   | rate      | 0                   | Allocation ratio in Timeshare                           |
   +-----------+---------------------+---------------------------------------------------------+
   | cpucores  | 0-3                 | Number of CPU cores                                     |
   +-----------+---------------------+---------------------------------------------------------+

gs_wlm_user_resource_info(name text)
------------------------------------

Description: Queries for a user's resource quota and resource usage.

Return type: record

pgxc_stat_single_table(schema, talename)
----------------------------------------

Description: Executed on CNs, with the schema name and table name passed. This function queries the statistics of a single table in the entire database and the dirty page rate of the table on each DN.

This function is supported by version 8.1.3 or later clusters.

.. note::

   The statistics of this function depend on the **ANALYZE** operation. To obtain the most accurate information, perform the **ANALYZE** operation on the table first.

Return type: record

The return value fields are the same as those of the :ref:`pg_stat_get_tuple() <en-us_topic_0000001188270492__section4444448123716>` function.

::

   SELECT * FROM pgxc_stat_single_table('public','t1');
    nodename  | tableid | partid |      last_vacuum       |    last_autovacuum     |         last_analyze          |    last_autoanalyze    | vacuum_count | autovacuum_count | analyze_count | autoanalyze_count | n_tup_ins | n_
   tup_upd | n_tup_del | n_tup_hot_upd | n_tup_change | n_live_tup | n_dead_tup | dirty_rate | last_data_changed
   -----------+---------+--------+------------------------+------------------------+-------------------------------+------------------------+--------------+------------------+---------------+-------------------+-----------+---
   --------+-----------+---------------+--------------+------------+------------+------------+-------------------
    datanode1 | 1270075 |        | 2000-01-01 08:00:00+08 | 2000-01-01 08:00:00+08 | 2023-01-09 09:38:43.220876+08 | 2000-01-01 08:00:00+08 |            0 |                0 |             1 |                 0 |         0 |
         0 |         0 |             0 |            0 |          0 |          0 |          0 |
   (1 row)
