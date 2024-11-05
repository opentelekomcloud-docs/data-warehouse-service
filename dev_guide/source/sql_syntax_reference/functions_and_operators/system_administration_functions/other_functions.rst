:original_name: dws_06_0062.html

.. _dws_06_0062:

Other Functions
===============

pgxc_pool_check()
-----------------

Description: Checks whether the connection data buffered in the pool is consistent with **pgxc_node**.

Return type: boolean

pgxc_pool_reload()
------------------

Description: Updates the connection information buffered in the pool.

Return type: boolean

pgxc_lock_for_backup()
----------------------

Description: Locks the cluster before backup. Backup is performed to restore data on new nodes.

Return type: boolean

.. note::

   **pgxc_lock_for_backup** locks a cluster before **gs_dump** or **gs_dumpall** is used to back up the cluster. After a cluster is locked, operations changing the system structure are not allowed. This function does not affect DML statements.

pg_pool_validate(clear boolean, co_node_name cstring)
-----------------------------------------------------

Description: Clears invalid backend threads on a CN. (These backend threads hold invalid pooler connections to standby DNs.)

Return type: record

pg_nodes_memory()
-----------------

Description: queries the memory usage of all nodes.

Return type: record

table_skewness(text)
--------------------

Description: queries the percentage of table data among all nodes.

Parameter: Indicates that the type of the name of the to-be-queried table is text.

Return type: record

table_skewness(table_name text, column_name text[, row_num text])
-----------------------------------------------------------------

Description: Queries the proportion of column data distributed on each node based on the hash distribution rule. The results are sorted based on the data volumes of the nodes.

Parameters: **table_name** indicates a table name, **column_name** indicates a column name, and **row_num** indicates that all data in the current column is returned. The default value is **0**. A value other than **0** indicates the number of data records whose statistics are sampled. (Records are randomly sampled.)

Return type: record

Example:

Distribute data by hash based on the **a** column in the **tx** table. Seven records are distributed on DN 1, two records on DN 2, and one record on DN 0.

::

   SELECT * FROM table_skewness('tx','a');
    seqnum | num |  ratio
   --------+-----+----------
    1      | 7   | 70.000%
    2      | 2   | 20.000%
    0      | 1   | 10.000%
   (3 row)

table_data_skewness(data_row record, locatorType "char")
--------------------------------------------------------

Description: Calculates the bucket distribution index for the records concatenated using the columns in a specified table.

Parameters: **data_row** indicates the record concatenated using columns in the specified table. **locatorType** indicates the distribution rule. You are advised to set **locatorType** to **H**, indicating hash distribution.

Return type: smallint

Example:

Calculates the bucket distribution index based on the hash distribution rule for the records combined concatenated using column a in the **tx** table.

::

   select a, table_data_skewness(row(a), 'H') from tx;
    a | table_data_skewness
   ---+---------------------
    3 |                   0
    6 |                   2
    7 |                   2
    4 |                   1
    5 |                   1
   (5 rows)

table_distribution(schemaname text, tablename text)
---------------------------------------------------

Description: queries the storage space occupied by a specified table on each node.

Parameter: Indicates that the types of the schema name and table name for the table to be queried are both text.

Return type: record

.. note::

   -  To query for the storage distribution of a specified table by using this function, you must have the **SELECT** permission for the table.
   -  The performance of **table_distribution** is better than that of **table_skewness**. Especially in a large cluster with a large amount of data, **table_distribution** is recommended.
   -  When you use **table_distribution** and want to view the space usage, you can use **dnsize** or **(sum(dnsize) over ())** to view the percentage.

table_distribution(regclass)
----------------------------

Description: queries the storage space occupied by a specified table on each node.

Parameter: indicates the name or OID of the table to be queried. The table name can be defined by the schema name. Parameter type: regclass

Return type: record

.. note::

   -  To query for the storage distribution of a specified table by using this function, you must have the **SELECT** permission for the table.
   -  The performance of **table_distribution** is better than that of **table_skewness**. Especially in a large cluster with a large amount of data, **table_distribution** is recommended.
   -  When you use **table_distribution** and want to view the space usage, you can use **dnsize** or **(sum(dnsize) over ())** to view the percentage.

table_distribution()
--------------------

Description: queries the storage distribution of all tables in the current database.

Return type: record

.. note::

   -  This function involves the query for information about all tables in the database. To execute this function, you must have the administrator rights or rights of the preset role **gs_role_read_all_stats**.
   -  Based on the table_distribution() function, GaussDB(DWS) provides the PGXC_GET_TABLE_SKEWNESS view as an alternative way to query for data skew. You are advised to use this view when the number of tables in the database is less than 10000.

gs_table_distribution(schemaname text, tablename text)
------------------------------------------------------

Description: queries the storage space occupied by a specified table on each node.

Return type: record

.. table:: **Table 1** Fields returned by the gs_table_distribution(schemaname text, tablename text) function

   +-----------------------+-----------------------+---------------------------------------------------+
   | Name                  | Type                  | Description                                       |
   +=======================+=======================+===================================================+
   | schemaname            | name                  | Schema name                                       |
   +-----------------------+-----------------------+---------------------------------------------------+
   | tablename             | name                  | Table name                                        |
   +-----------------------+-----------------------+---------------------------------------------------+
   | relkind               | character             | Type.                                             |
   |                       |                       |                                                   |
   |                       |                       | -  **i**: index                                   |
   |                       |                       | -  **r**: table                                   |
   +-----------------------+-----------------------+---------------------------------------------------+
   | nodename              | name                  | Node name                                         |
   +-----------------------+-----------------------+---------------------------------------------------+
   | dnsize                | bigint                | Storage space of the table on the node, in bytes. |
   +-----------------------+-----------------------+---------------------------------------------------+

.. note::

   -  To query for the storage distribution of a specified table by using this function, you must have the **SELECT** permission for the table.
   -  This function is based on the physical file storage space records in the **PG_RELFILENODE_SIZE** system catalog. Ensure that the GUC parameters **use_workload_manager** and **enable_perm_space** are enabled.
   -  The performance of the **gs_table_distribution** function is lower than that of the **table_distribution** function when a single table is queried. But when the entire database is queried, the performance of the **gs_table_distribution** function is much better. In a large cluster with a large amount of data, you are advised to use the **gs_table_distribution** function to query all tables in the database.

gs_table_distribution()
-----------------------

Description: quickly queries the storage distribution of all tables in the current database.

Return type: record

.. table:: **Table 2** Fields returned by the gs_table_distribution() function

   ========== ========= =================================================
   Name       Type      Description
   ========== ========= =================================================
   schemaname name      Schema name
   tablename  name      Table name
   relkind    character Type of the table. **i**: index; **r**: table.
   nodename   name      Node name
   dnsize     bigint    Storage space of the table on the node, in bytes.
   ========== ========= =================================================

.. note::

   -  To query for the storage distribution of a specified table by using this function, you must have the **SELECT** permission for the table.
   -  This function is based on the physical file storage space records in the **PG_RELFILENODE_SIZE** system catalog. Ensure that the GUC parameters **use_workload_manager** and **enable_perm_space** are enabled.
   -  The performance of the **gs_table_distribution** function is lower than that of the **table_distribution** function when a single table is queried. But when the entire database is queried, the performance of the **gs_table_distribution** function is much better. In a large cluster with a large amount of data, you are advised to use the **gs_table_distribution** function to query all tables in the database.
   -  Based on the **gs_table_distribution()** function, GaussDB(DWS) 8.2.1 and later versions provide the **PGXC_WLM_TABLE_DISTRIBUTION_SKEWNESS** view for data skew query. You are advised to use this view when the number of tables in the database is small (less than 10,000).

plan_seed()
-----------

Description: Obtains the seed value of the previous query statement (internal use).

Return type: integer

pg_stat_get_env()
-----------------

Description: Obtains the environment variable information about the current node.

Return type: record

pg_stat_get_thread()
--------------------

Description: Provides information about the status of all threads under the current node.

Return type: record

pgxc_get_os_threads()
---------------------

Description: Provides information about the status of threads under all normal nodes in a cluster.

Return type: record

pg_stat_get_sql_count()
-----------------------

Description: Provides statistics on the number of **SELECT**/**UPDATE**/**INSERT**/**DELETE**/**MERGE INTO** statements executed by all users on the current node, response time, and the number of DDL, DML, and DCL statements.

Return type: record

pgxc_get_sql_count()
--------------------

Description: Provides statistics on the number of **SELECT**/**UPDATE**/**INSERT**/**DELETE**/**MERGE INTO** statements executed by all users on all nodes of the current cluster, response time, and the number of DDL, DML, and DCL statements.

Return type: record

pgxc_get_workload_sql_count()
-----------------------------

Description: Provides statistics on the number of **SELECT**/**UPDATE**/**INSERT**/**DELETE** statements executed in all workload Cgroup on all CNs of the current cluster and the number of DDL, DML, and DCL statements.

Return type: record

pgxc_get_workload_sql_elapse_time()
-----------------------------------

Description: Provides statistics on response time of **SELECT**/**UPDATE**/**INSERT**/**DELETE** statements executed in all workload Cgroup on all CNs of the current cluster.

Return type: record

get_instr_unique_sql()
----------------------

Description: Provides information about Unique SQL statistics collected on the current node. If the node is a CN, the system returns the complete information about the Unique SQL statistics collected on the CN. That is, the system collects and summarizes the information about the Unique SQL statistics on other CNs and DNs. If the node is a DN, the Unique SQL statistics on the DN is returned. For details, see the **GS_INSTR_UNIQUE_SQL** view.

Return type: record

reset_instr_unique_sql(cstring, cstring, INT8)
----------------------------------------------

Description: Clears collected Unique SQL statistics. The input parameters are described as follows:

-  **GLOBAL**/**LOCAL**: Data is cleared from all nodes or the current node.
-  **ALL**/**BY_USERID**/**BY_CNID**/**BY_GUC**: **ALL** indicates that all data is cleared. **BY_USERID/BY_CNID** indicates that data is cleared by **USERID** or **CNID**. **BY_GUC** indicates that the clearance operation is caused by the decrease of the value of the GUC parameter **instr_unique_sql_count**.
-  The third parameter corresponds to the second parameter. The parameter is invalid for **ALL**/**BY_GUC**.

Return type: bool

pgxc_get_instr_unique_sql()
---------------------------

Description: Provides complete information about Unique SQL statistics collected on all CNs in a cluster. This function can be executed only on CNs.

Return type: record

get_instr_unique_sql_remote_cns()
---------------------------------

Description: Provides complete information about Unique SQL statements collected on all CNs in the cluster, except the CN on which the function is being executed. This function can be executed only on CNs.

Return type: record

pgxc_get_node_env()
-------------------

Description: Provides the environment variable information about all nodes in a cluster.

Return type: record

gs_switch_relfilenode()
-----------------------

Description: Exchanges meta information of two tables or partitions. (This is only used for the redistribution tool. An error message is displayed when the function is directly used by users).

Return type: integer

copy_error_log_create()
-----------------------

Description: Creates the error table (**public.pgxc_copy_error_log**) required for creating the **COPY FROM** error tolerance mechanism.

Return type: boolean

.. note::

   -  This function attempts to create the **public.pgxc_copy_error_log** table. For details about the table, see :ref:`Table 3 <en-us_topic_0000001460561332__table63361925092>`.
   -  Create the B-tree index on the **relname** column and execute **REVOKE ALL on public.pgxc_copy_error_log FROM public** to manage permissions for the error table (the permissions are the same as those of the **COPY** statement).
   -  **public.pgxc_copy_error_log** is a row-store table. Therefore, this function can be executed and **COPY FROM** error tolerance is available only when row-store tables can be created in the cluster. After the GUC parameter **enable_hadoop_env** is enabled, row-based tables cannot be created in the cluster. The default value is **off**.
   -  Same as the error table and the **COPY** statement, the function requires **sysadmin** or higher permissions.
   -  If the **public.pgxc_copy_error_log** table or the **copy_error_log_relname_idx** index already exists before the function creates it, the function will report an error and roll back.

.. _en-us_topic_0000001460561332__table63361925092:

.. table:: **Table 3** Error table public.pgxc_copy_error_log

   +------------+--------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Column     | Type                     | Description                                                                                                                                                 |
   +============+==========================+=============================================================================================================================================================+
   | relname    | varchar                  | Table name in the form of *Schema name*\ **.**\ *Table name*                                                                                                |
   +------------+--------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | begintime  | timestamp with time zone | Time when a data format error was reported                                                                                                                  |
   +------------+--------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | filename   | varchar                  | Name of the source data file where a data format error occurs                                                                                               |
   +------------+--------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | rownum     | bigint                   | Number of the row where a data format error occurs in a source data file                                                                                    |
   +------------+--------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | rawrecord  | text                     | Raw record of a data format error in the source data file To prevent a field from being too long, the length of the field cannot exceed 1024 bytes.         |
   +------------+--------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | detail     | text                     | Error details                                                                                                                                               |
   +------------+--------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | columnname | varchar                  | Name of the column whose data format is incorrect in the data source file. Only 8.2.1.100 and later versions support this function.                         |
   +------------+--------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | errcode    | varchar                  | Error code corresponding to the error information. The sqlstate error code is used. Only 8.2.1.100 and later versions support this function.                |
   +------------+--------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | queryid    | bigint                   | ID of the SQL statement for executing the Copy statement. It uniquely identifies an SQL statement. Only 8.2.1.100 and later versions support this function. |
   +------------+--------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+

pv_compute_pool_workload()
--------------------------

Description: Provides the current load information about computing Node Groups on cloud.

Return type: record

pg_stat_get_status(tid, num_node_display)
-----------------------------------------

Description: Queries for the blocking and waiting status of the backend threads and auxiliary threads in the current instance. For details about the returned results, see the PG_THREAD_WAIT_STATUS view. The input parameters are described as follows:

-  **tid**: thread ID, which is of the bigint type. If this parameter is null, the waiting statuses of all backend threads and auxiliary threads are returned. Otherwise, only the waiting statuses of threads with the specified IDs are returned.
-  **num_node_display**: integer type. Specifies the maximum number of waiting nodes displayed in the **wait_status** column for records whose waiting status is **wait node**.

   -  If this parameter is left empty or set to a value less than or equal to **0**, only one waiting node is displayed.
   -  If the value is greater than **20**, a maximum number of nodes can be displayed is **20**.
   -  If the value is greater than **0** and less than or equal to **20**, the smaller value between **num_node_display** and the actual number of waiting nodes is displayed. Use the **SELECT \* from pg_stat_get_status(NULL, 10)** query for example. If the number of waiting nodes is greater than **10**, the names of only 10 nodes are displayed randomly. If the number of waiting nodes is less than or equal to **10**, the names of all waiting nodes are displayed. If the number of waiting nodes is greater than the number of displayed nodes, the displayed node names are randomly selected.

Return type: record

pgxc_get_thread_wait_status(num_node_display)
---------------------------------------------

Description: Queries for the call hierarchy between threads generated by all SQL statements on each node in a cluster, as well as the block waiting status of each thread. For details about the returned results, see the PGXC_THREAD_WAIT_STATUS view. The type and meaning of the input parameter **num_node_display** are the same as those of the **pg_stat_get_status** function.

Return type: record

pgxc_os_run_info()
------------------

Description: Obtains the running status of the operating system on each node in a cluster. For details about the returned results, see "System Catalogs > System Views >PV_OS_RUN_INFO" in the *Developer Guide*.

Return type: record

get_instr_wait_event()
----------------------

Description: obtains the waiting status and events of the current instance. For details about the returned results, see "System Catalogs > System Views > GS_WAIT_EVENTS" in the *Developer Guide*. If the GUC parameter **enable_track_wait_event** is **off**, this function returns **0**.

Return type: record

pgxc_wait_events()
------------------

Description: queries statistics about waiting status and events on each node in a cluster. For details about the returned results, see "System Catalogs > System Views > PGXC_WAIT_EVENTS" in the *Developer Guide*. If the GUC parameter **enable_track_wait_event** is **off**, this function returns **0**.

Return type: record

pgxc_stat_bgwriter()
--------------------

Description: queries statistics about backend write processes on each node in a cluster. For details about the returned results, see "System Catalogs > System Views > PG_STAT_BGWRITER" in the *Developer Guide*.

Return type: record

pgxc_stat_replication()
-----------------------

Description: queries information about the log synchronization status on each node in a cluster, such as the location where the logs are sent and received. For details about the returned results, see "System Catalogs > System Views > PG_STAT_REPLICATION" in the *Developer Guide*.

Return type: record

pgxc_replication_slots()
------------------------

Description: queries the replication status on each DN in a cluster. For details about the returned results, see "System Catalogs > System Views > PG_REPLICATION_SLOTS" in the *Developer Guide*.

Return type: record

pgxc_settings()
---------------

Description: queries information about runtime parameters on each node in a cluster. For details about the returned results, see "System Catalogs > System Views > PG_SETTINGS" in the *Developer Guide*.

Return type: record

pgxc_instance_time()
--------------------

Description: queries the running time statistics of each node in a cluster and the time consumed in each execution phase. For details about the returned results, see "System Catalogs > System Views > PV_INSTANCE_TIME" in the *Developer Guide*.

Return type: record

pg_stat_get_redo_stat()
-----------------------

Description: queries Xlog redo statistics on the current node. For details about the returned results, see "System Catalogs > System Views > PV_REDO_STAT" in the *Developer Guide*.

Return type: record

pgxc_redo_stat()
----------------

Description: queries the Xlog redo statistics of each node in a cluster. For details about the returned results, see "System Catalogs > System Views > PV_REDO_STAT" in the *Developer Guide*.

Return type: record

get_local_rel_iostat()
----------------------

Description: Obtains the disk I/O statistics of the current instance. For details about the returned results, see "System Catalogs > System Views > GS_REL_IOSTAT" in the *Developer Guide*.

Return type: record

pgxc_rel_iostat()
-----------------

Description: queries the disk I/O statistics on each node in a cluster. For details about the returned result, see "System Catalogs > System Views > GS_REL_IOSTAT" in the *Developer Guide*.

Return type: record

get_node_stat_reset_time()
--------------------------

Description: Obtains the time when statistics of the current instance were reset.

Return type: timestamptz

.. _en-us_topic_0000001460561332__section158811598710:

pgxc_node_stat_reset_time()
---------------------------

Description: queries the time when the statistics of each node in a cluster are reset. For details about the returned result, see "System Catalogs > System Views > GS_NODE_STAT_RESET_TIME" in the *Developer Guide*.

Return type: record

.. note::

   When an instance is running, its statistics keep rising. In the following cases, the statistical values in the memory will be reset to **0**:

   -  The instance is restarted or a cluster switchover occurs.
   -  The database is dropped.
   -  A reset operation is performed. For example, the statistics counter in the database is reset using the **pgstat_recv_resetcounter** function or the Unique SQL statements are cleared using the **reset_instr_unique_sql** function.

   If any of the preceding events occurs, GaussDB(DWS) will record the time when the statistics are reset. You can query the time using the **get_node_stat_reset_time** function.

pgxc_parallel_query(text, text)
-------------------------------

Description: Runs a specified SQL query statement on a data instance of a specified type and returns the query result to the current CN. This function is supported in 8.1.2 or later.

The function has two parameters:

The first parameter specifies the instances on which the SQL statement is executed. Currently, the valid input parameters are **dn**, **datanode**, **cn**, **coordinator**, and **all**. Other values will cause function execution errors. **dn** and **datanode** indicate that the statement is executed on all DNs. **cn** and **coordinator** indicate that the statement is executed on all CNs. **all** indicates that the statement is executed on all CNs and DNs.

The second parameter specifies the verification of the objects queried by the SQL statement that is to be sent to a remote node for execution. User tables, distributed tables, and user-defined functions with multiple result sets are not supported.

Return type: record

.. note::

   -  This function is only used by developers to efficiently collect the execution information or status of instances in a cluster. You are not advised to use it directly.

   -  This function contains multiple result sets, and the return data type is record. Therefore, you need to add the output column name and data type specified by the **AS** statement after the function call, as shown in the following:

      .. code-block::

         SELECT * FROM pgxc_parallel_query('all', 'select node_name, db_name, thread_name, query_id, tid, lwtid, ptid, tlevel, smpid, wait_status, wait_event from pg_thread_wait_status') AS (node_name text, db_name text, thread_name text, query_id bigint, tid bigint, lwtid integer , ptid integer, tlevel integer , smpid integer, wait_status text, wait_event text);

   -  The data type of the output result of the SQL statement specified by the second parameter of the function must be the same as the data type specified by the **AS** statement. Otherwise, an error may be reported during execution due to type mismatch.

   -  The SQL statement specified by the second parameter of the function cannot trigger cross-node query. Otherwise, an error is reported.

   -  The SQL statement specified by the second parameter of the function can only be a **SELECT**, **UPDATE**, **DELETE**, or **INSERT** statement.

      -  The **returning** statement is not supported.

      -  The user who invokes the function must have the operation permission on the SQL objects.

      -  For **INSERT** statements, **INSERT OVERWRITE**, **UPSERT**, and **INSERT INTO** are not supported.

      -  The **UPDATE**, **DELETE**, and **INSERT** statements can be executed only by the initial user in in-place upgrade mode or by the administrator in redistribution mode. The number of records modified by the statements on each instance must be the same. Otherwise, an error will be reported during statement execution. The function outputs a column of values of the bigint type. These values indicate the number of records operated by the statement on each instance.

         .. code-block::

            SELECT * FROM pgxc_parallel_query('cn', 'UPDATE pg_partition SET relpages = 0') AS (updated bigint);

create_wdr_snapshot()
---------------------

Description: Creates a performance data snapshot.

Return type: text

.. note::

   -  Only the database administrator **SYSADMIN** can execute this function.
   -  This function can be executed only on CNs. If it is executed on DNs, the following message will be returned: "WDR snapshot can only be created on coordinator."
   -  Before executing this function, ensure that the value of **enable_wdr_snapshot** is **on**. If its value is **off**, the following message will be returned for this function: "WDR snapshot request cannot be executed, because GUC parameter 'enable_wdr_snapshot' is off."
   -  If the snapshot thread is not started for some reason, for example, the node is restarted, the following message will be returned for this function: "WDR snapshot request cannot be accepted, please retry later."
   -  If this function fails to be executed, the following message will be returned: "Cannot respond to WDR snapshot request."
   -  If this function is successfully executed, the following message will be returned: "WDR snapshot request has been submitted." This message indicates that the snapshot creation request has been sent to the background snapshot thread, but does not mean that the snapshot has been successfully created.

kill_snapshot(scope cstring)
----------------------------

Description: kills the background snapshot thread. This function sends a command to the background snapshot thread and waits for the thread to stop.

The input parameter **scope** indicates the operation scope. Its value can be **local** or **global**.

-  Value **local** indicates killing the snapshot thread on the current CN.
-  Value **global** indicates killing the snapshot thread on the current CN as well as those on all the other CNs in the cluster.
-  If any other value is passed, error message "Scope is invalid, use "local" or "global"." is displayed.
-  The input parameter can be left empty, in which case the default value **local** will be used.

Return type: none

.. note::

   -  Only the database administrator **SYSADMIN** can execute this function.
   -  This function can be executed only on CNs. If it is executed on DNs, the following message will be returned: "kill_snapshot can only be executed on coordinator."
   -  Executing this function sends a kill signal to the background snapshot thread and waits for it to finish. If the snapshot thread is not killed within 100s, the error message "Kill snapshot thread failed" is displayed.

generate_wdr_report(begin_snap_id bigint, end_snap_id bigint, report_type cstring, report_scope cstring, node_name cstring)
---------------------------------------------------------------------------------------------------------------------------

Description: Creates a load analysis report.

The input parameters are described as follows:

-  **begin_snap_id** and **end_snap_id**: IDs of the start and end snapshots, respectively. The IDs are of the bigint type. The value of **begin_snap_id** must be less than that of **end_snap_id**, and the time for the start and end snapshots cannot overlap. You can check whether the snapshot time overlaps by querying **select s1.end_ts < s2.start_ts from (select \* from dbms_om.snapshot where snapshot_id=$begin_snap_id) as s1, (select \* from dbms_om.snapshot where snapshot_id=$end_snap_id) as s2** in the **dbms_om.snapshot** table. If **true** is returned, the snapshot time does not overlap. Otherwise, the snapshot time overlaps.
-  **report_type**: report type. The value is a cstring and can be **summary**, **detail**, or **all**.
-  **report_scope**: report scope. The value is a cstring and can be **cluster** or **node**.
-  **node_name**: node name. The value is a cstring. If **report_scope** is **node**, the value of this parameter must be **pg_catalog**, which indicates the CN or DN name in the **node_name** column of the **pgxc_node** table.

Return type: text

.. note::

   -  Only the database administrator **SYSADMIN** can execute this function.
   -  This function can be executed only on CNs. If it is executed on DNs, the following message will be returned: "WDR report can only be created on coordinator."
   -  If the report is created successfully, message "Report %s has been generated" will be returned.
   -  The statistics cannot be reset between the time the start snapshot is taken and the time the end snapshot is taken. Otherwise, error message "Instance reset time is different" will be displayed. For details about the events that cause a statistics reset, see the :ref:`pgxc_node_stat_reset_time <en-us_topic_0000001460561332__section158811598710>` function.

wdr_xdb_query(db_name text, snapshot_id bigint, view_name text)
---------------------------------------------------------------

Description: Queries a specified view in a specified database. The query results of some views vary depending on databases. For example, the **global_table_stat** view is used to query the statistics of a table. The results of querying this view vary because tables in different databases are different. The **wdr_xdb_query** function can access the database specified by **db_name** in the current connection and query the view specified by **view_name** in the database. The input parameters are described as follows:

-  **db_name**: specifies the name of a database. The value is of the text type.
-  **snapshot_id**: specifies the snapshot ID. The value is of the bigint type. For details, see "Performance View Snapshot".
-  **view_name**: specifies the name of a view. The value is of the text type. The view name must be in the following whitelist:

   -  global_table_stat

   -  global_table_change_stat

   -  global_column_table_io_stat

   -  global_row_table_io_stat

      The return value type is record. The first column is **snapshot_id bigint**, and the second column is **db_name text**. The names, types, and sequences of other columns are the same as those of the views specified by **view_name**.

      Example:

      ::

         select snapshot_id, db_name, schemaname, relname, distribute_mode, seq_scan ,seq_tuple_read ,index_scan ,index_tuple_read ,tuple_inserted
         ,tuple_updated ,tuple_deleted ,tuple_hot_updated ,live_tuples ,dead_tuples from wdr_xdb_query('postgres'::text, 1, 'global_table_stat'::text) as i(snapshot_id bigint, db_name text, schemaname name, relname name, distribute_mode char, seq_scan bigint, seq_tuple_read bigint, index_scan bigint, index_tuple_read bigint, tuple_inserted bigint, tuple_updated bigint, tuplee_deleted bigint, tuple_hot_updated bigint, live_tuples bigint, dead_tuples bigint);

      .. note::

         -  This function is supported only in 8.1.2 or later.
         -  Only the database administrator **SYSADMIN** can execute this function.
         -  This function can be used to query only the views in the whitelist. If you use this function to query other views, the error message **Input view name is invalid.** will be displayed.

vac_fileclear_relation(oid)
---------------------------

Description: Forcibly clears VACUUM rewritten files in a specified column-store table to reclaim space.

Parameter: OID of a column-store table.

Return type: integer

.. note::

   -  Before using this function, set **colvacuum_threshold_scale_factor** and ensure that the files are cleared and space reclaimed only after the VACUUM process has rewritten the files of the specified column-store table.
   -  This function exclusively locks a specified column-store table.

vac_fileclear_all_relation()
----------------------------

Description: Forcibly clears VACUUM rewritten files in all specified column-store tables to reclaim space.

Return type: record

get_col_file_info(table_name)
-----------------------------

Description: Queries the number of empty CU files and the total number of CU files in a specified column-store table. This function is supported only in cluster 8.2.0 and later versions.

Parameter: Name of a column-store table.

Return type: int

Fields in the returned value:

-  **total_file_num int**: total number of CU files. The value ranges from **-1** to **INT_MAX**. **-1** indicates a failure, which could be caused by unsupported table types. The values in the range **0** to **INT_MAX** indicates the total number of files.
-  **empty_file_num int**: number of empty CU files. The value ranges from **-1** to **INT_MAX**. **-1** indicates a failure, which could be caused by unsupported table types. The values in the range **0** to **INT_MAX** indicates the total number of empty files.

Example:

::

   call get_col_file_info('t4');
    total_file_num | empty_file_num
   ----------------+----------------
                10 |              7
   (1 row)

get_all_col_file_info()
-----------------------

Description: Queries the number of empty CU files and the total number of CU files in all column-store tables. This function is supported only in cluster 8.2.0 and later versions.

Return type: record

Fields in the returned value:

-  **space_name text**: schema to which the column-store tables belong
-  **table_name text**: name of a column-store table
-  **total_file_num int**: total number of CU files. The value ranges from **-1** to **INT_MAX**. **-1** indicates a failure, which could be caused by unsupported table types. The values in the range **0** to **INT_MAX** indicates the total number of files.
-  **empty_file_num int**: number of empty CU files. The value ranges from **-1** to **INT_MAX**. **-1** indicates a failure, which could be caused by unsupported table types. The values in the range **0** to **INT_MAX** indicates the total number of empty files.

Example:

::

   call get_all_col_file_info();
    space_name | table_name | total_file_num | empty_file_num
   ------------+------------+----------------+----------------
    public     | t4         |             10 |              7
    public     | t2         |              1 |              1
    public     | t1         |              3 |              0
   (3 rows)

get_volatile_pg_class()
-----------------------

Description: Obtains the **pg_class** metadata related to all volatile temporary tables in the current session. This parameter is supported by version 8.2.0 or later clusters.

Return type: record

Fields in the returned value:

-  **oid**: OID of the volatile temporary table.
-  Other fields: same as the fields (excluding hidden fields) in the **pg_class system** catalog.

get_volatile_pg_class(relname text)
-----------------------------------

Description: Obtains the **pg_class** metadata related to a specified volatile temporary table in the current session. This parameter is supported by version 8.2.0 or later clusters.

Parameter: name of the volatile temporary table in the current session.

Return type: record

Fields in the returned value:

-  **oid**: OID of the volatile temporary table.
-  Other fields: same as the fields (excluding hidden fields) in the **pg_class system** catalog.

Example:

::

   SELECT * FROM get_volatile_pg_class('tx1');
     oid  | relname | relnamespace | reltype | reloftype | relowner | relam | relfilenode | reltablespace | relpages | reltuples | relallvisible | reltoastrelid | reltoastidxid | reldeltarelid |
    reldeltaidx | relcudescrelid | relcudescidx | relhasindex | relisshared | relpersistence | relkind | relnatts | relchecks | relhasoids | relhaspkey | relhasrules | relhastriggers | relhassub
   class | relcmprs | relhasclusterkey | relrowmovement | parttype | relfrozenxid | relacl |            reloptions            | relreplident | relfrozenxid64
   -------+---------+--------------+---------+-----------+----------+-------+-------------+---------------+----------+-----------+---------------+---------------+---------------+---------------+
   -------------+----------------+--------------+-------------+-------------+----------------+---------+----------+-----------+------------+------------+-------------+----------------+----------
   ------+----------+------------------+----------------+----------+--------------+--------+----------------------------------+--------------+----------------
    16772 | tx1     |        16770 |   16774 |         0 |       10 |     0 |       16772 |          1665 |        0 |         0 |             0 |         16775 |             0 |             0 |
              0 |              0 |            0 | f           | f           | v              | r       |        2 |         0 | f          | f          | f           | f              | f
         | 1        | f                | f              | n        | 11815        |        | {orientation=row,compression=no} | d            |          11815
   (1 row)

get_volatile_pg_attribute()
---------------------------

Description: Obtains the **pg_attribute** metadata related to all volatile temporary tables in the current session. This parameter is supported by version 8.2.0 or later clusters.

Return type: record

Fields in the returned value:

-  **oid**: OID of the column.
-  Other fields: same as the fields (excluding hidden fields) in the **pg_attribute** catalog.

get_volatile_pg_attribute(relname text, attrname text)
------------------------------------------------------

Description: Obtains the **pg_attribute** metadata related to a specified volatile temporary table in the current session. This parameter is supported by version 8.2.0 or later clusters.

Parameter:

-  **relname**: table name (must be in the current session).
-  **attrname**: column name.

Return type: record

Fields in the returned value:

-  **oid**: OID of the column.
-  Other fields: same as the fields (excluding hidden fields) in the **pg_attribute** catalog.

Example:

::

   SELECT * FROM get_volatile_pg_attribute('tx1', 'b');
    attrelid | attname | atttypid | attstattarget | attlen | attnum | attndims | attcacheoff | atttypmod | attbyval | attstorage | attalign | attnotnull | atthasdef | attisdropped | attislocal |
    attcmprmode | attinhcount | attcollation | attacl | attoptions | attfdwoptions | attinitdefval | attkvtype
   ----------+---------+----------+---------------+--------+--------+----------+-------------+-----------+----------+------------+----------+------------+-----------+--------------+------------+
   -------------+-------------+--------------+--------+------------+---------------+---------------+-----------
       16772 | b       |       25 |            -1 |     -1 |      2 |        0 |          -1 |        -1 | f        | x          | i        | f          | f         | f            | t          |
    127         |           0 |          100 |        |            |               |               | 0
   (1 row)

pg_get_publication_tables(pubname text)
---------------------------------------

Description: Returns the relid list of tables to be published based on the publication name. This function is supported by clusters of version 8.2.0.100 or later.

Parameter: **pubname**

Return type: set of OID

Example:

::

   SELECT * FROM pg_get_publication_tables('mypub');
    relid
   -------
    16757
    16776
   (2 rows)

pg_relation_is_publishable(relname regclass)
--------------------------------------------

Description: Checks whether a table can be published. This function is supported by clusters of version 8.2.0.100 or later.

Parameter: **relname**

Return type: Boolean

Example:

::

   SELECT * FROM pg_relation_is_publishable('t1');
    pg_relation_is_publishable
   ----------------------------
    t
   (1 row)

get_col_cu_info(schema_name text, table_name text, row_count int8, dirty_percent int8)
--------------------------------------------------------------------------------------

Description: Queries the CU information of a column-store table. The CU information of each partition is collected separately. This function is supported by clusters of version 8.2.0.100 or later.

Parameters: schema name (mandatory), table name (mandatory), threshold for the number of rows in a small CU (optional, 200 by default, ranging from 1 to 60000, and percentage threshold for deleting dirty CUs (optional, 70 by default, ranging from 1 to 100)

Return type: record

Fields in the returned value:

**node_name**: DN name.

**part_name**: partition name. This column is empty for a common table.

**zero_size_cu_count**: number of CUs whose **cuSize** is **0** and number of rows is less than or equal to **row_count**.

**small_cu_count**: number of CUs whose **cuSize** is **ALIGNOF_CUSIZE(8192)** and number of rows is less than or equal to **row_count**.

**dirty_cu_count**: number of CUs whose deadtupe percentage exceeds **dirty_percent** due to deletion.

**total_cu_count**: total number of CUs.

**small_cu_size**: total size of 8 KB CUs.

**total_cu_size**: total CU size.

Example:

::

    SELECT * FROM get_col_cu_info('public','hs_part');
    node_name | part_name | zero_size_cu_count | small_cu_count | dirty_cu_count | total_cu_count | small_cu_size | total_cu_size
   -----------+-----------+--------------------+----------------+----------------+----------------+---------------+---------------
    dn_1      | p1        |                  3 |              0 |              0 |              3 | 0 bytes       | 0 bytes
    dn_1      | p2        |                  3 |              0 |              0 |              3 | 0 bytes       | 0 bytes
    dn_1      | p3        |                  3 |              0 |              0 |              3 | 0 bytes       | 0 bytes
   (3 rows)

    SELECT * FROM get_col_cu_info('public','hs_part', 200, 90);
    node_name | part_name | zero_size_cu_count | small_cu_count | dirty_cu_count | total_cu_count | small_cu_size | total_cu_size
   -----------+-----------+--------------------+----------------+----------------+----------------+---------------+---------------
    dn_1      | p1        |                  3 |              0 |              0 |              3 | 0 bytes       | 0 bytes
    dn_1      | p2        |                  3 |              0 |              0 |              3 | 0 bytes       | 0 bytes
    dn_1      | p3        |                  3 |              0 |              0 |              3 | 0 bytes       | 0 bytes
   (3 rows)

get_col_file_vacuum_info(schema_name text, table_name text, force_get_rewritten_file_num bool)
----------------------------------------------------------------------------------------------

Description: Queries the vacuum information of a column-store table. The vacuum information of each partition is collected separately. This function is supported by clusters of version 8.2.0.100 or later.

Parameters: schema name (mandatory), table name (mandatory), and whether to forcibly obtain the precise number of files that can be cleared (mandatory, **false** by default)

Return type: record

Fields in the returned value:

**node_name**: DN name.

**part_name**: partition name. This column is empty for a common table.

**total_file_num**: total number of CU files.

**rewritable_file_num**: number of files that can be rewritten but have not been rewritten.

**rewritten_file_num**: number of files that have been rewritten but have not been cleared. The value is obtained from data in the memory. If the memory data is lost due to reasons such as restart, you can set **force_get_rewritten_file_num=true** to forcibly obtain the accurate number of files that can be cleared.

**empty_file_num**: number of cleared files.

Example:

::

   SELECT * FROM get_col_file_vacuum_info('public','pa',false);
    node_name | part_name | total_file_num | rewritable_file_num | rewritten_file_num | empty_file_num
   -----------+-----------+----------------+---------------------+--------------------+----------------
    datanode1 | pa1       |              1 |                   0 |                  0 |              0
    datanode1 | pa2       |              1 |                   0 |                  0 |              0
    datanode2 | pa1       |              1 |                   0 |                  0 |              0
    datanode2 | pa2       |              1 |                   0 |                  0 |              0
    datanode3 | pa1       |              1 |                   0 |                  0 |              0
    datanode3 | pa2       |              1 |                   0 |                  0 |              0
   (6 rows)

get_col_file_vacuum_info(schema_name text, table_name text, colvacuum_threshold_scale_factor int)
-------------------------------------------------------------------------------------------------

Description: Queries the vacuum information of a column-store table. The vacuum information of each partition is collected separately. This function is supported by clusters of version 8.2.0.100 or later.

Parameters: schema name (mandatory), table name (mandatory), and **colvacuum_threshold_scale_factor** (mandatory. The value range is 0 to 100, indicating the ratio of dead tuples.)

Return type: record

Return value:

**node_name**: DN name.

**part_name**: partition name. This column is empty for a common table.

**total_file_num**: total number of CU files.

**rewritable_file_num**: number of files that can be rewritten but have not been rewritten.

**rewritten_file_num**: number of files that have been rewritten but have not been cleared. The value is obtained from data in the memory. If the memory data is lost due to reasons such as restart, you can set **force_get_rewritten_file_num=true** to forcibly obtain the accurate number of files that can be cleared.

**empty_file_num**: number of cleared files.

Example:

::

   SELECT * FROM get_col_file_vacuum_info('public','pa',10);
    node_name | part_name | total_file_num | rewritable_file_num | rewritten_file_num | empty_file_num
   -----------+-----------+----------------+---------------------+--------------------+----------------
    datanode1 | pa1       |              1 |                   0 |                  0 |              0
    datanode1 | pa2       |              1 |                   0 |                  0 |              0
    datanode2 | pa1       |              1 |                   0 |                  0 |              0
    datanode2 | pa2       |              1 |                   0 |                  0 |              0
    datanode3 | pa1       |              1 |                   0 |                  0 |              0
    datanode3 | pa2       |              1 |                   0 |                  0 |              0
   (6 rows)

get_all_col_cu_info(row_count int8)
-----------------------------------

Description: Queries the CU information of all column-store tables in the database. This function is supported by clusters of version 8.2.0.100 or later.

Parameter: threshold for the number of rows in a small CU (optional, **200** by default, and ranging from **1** to **60000**)

Return type: record

Fields in the returned value:

**node_name**: DN name.

**schema_name**: schema name.

**table_name**: table name.

**zero_size_cu_count**: number of CUs whose **cuSize** is **0** and number of rows is less than or equal to **row_count**.

**small_cu_count**: number of CUs whose **cuSize** is **ALIGNOF_CUSIZE(8192)** and number of rows is less than or equal to **row_count**.

**total_cu_count**: total number of CUs.

**small_cu_size**: total size of 8 KB CUs.

**total_cu_size**: total CU size.

Example:

::

   SELECT * FROM get_all_col_cu_info(200);
    node_name | schema_name |      table_name      | zero_size_cu_count | small_cu_count | total_cu_count | small_cu_size | total_cu_size
   -----------+-------------+----------------------+--------------------+----------------+----------------+---------------+---------------
    datanode1 | public      | udi_48076            |                  5 |              1 |              6 | 8192 bytes    | 8192 bytes
    datanode1 | public      | udi_48077            |                  5 |              1 |              6 | 8192 bytes    | 8192 bytes
    datanode2 | public      | udi_48076            |                  5 |              1 |              6 | 8192 bytes    | 8192 bytes
    datanode2 | public      | udi_48077            |                  5 |              1 |              6 | 8192 bytes    | 8192 bytes
    datanode3 | public      | udi_48076            |                  5 |              1 |              6 | 8192 bytes    | 8192 bytes
    datanode3 | public      | udi_48077            |                  5 |              1 |              6 | 8192 bytes    | 8192 bytes
   (6 rows)

get_all_col_file_vacuum_info(force_get_rewritten_file_num bool)
---------------------------------------------------------------

Description: Queries the vacuum information of all column-store tables in the database. This function is supported by clusters of version 8.2.0.100 or later.

Parameter: whether to forcibly obtain the accurate number of files that can be cleared (mandatory. It can be **true** or **false** .)

Return type: record

Fields in the returned value:

**node_name**: DN name.

**schema_name**: schema name.

**table_name**: table name.

**total_file_num**: total number of CU files.

**rewritable_file_num**: number of files that can be rewritten but have not been rewritten.

**rewritten_file_num**: number of files that have been rewritten but have not been cleared. The value is obtained from data in the memory. If the memory data is lost due to reasons such as restart, you can set **force_get_rewritten_file_num=true** to forcibly obtain the accurate number of files that can be cleared.

**empty_file_num**: number of cleared files.

Example:

::

   SELECT * FROM get_all_col_file_vacuum_info(false);
    node_name | schema_name |      table_name      | total_file_num | rewritable_file_num | rewritten_file_num | empty_file_num
   -----------+-------------+----------------------+----------------+---------------------+--------------------+----------------
    datanode1 | public      | udi_57373            |              2 |                   0 |                  0 |              1
    datanode1 | public      | udi_57374            |              2 |                   0 |                  0 |              1
    datanode2 | public      | udi_57373            |              2 |                   0 |                  0 |              1
    datanode2 | public      | udi_57374            |              2 |                   0 |                  0 |              1
    datanode3 | public      | udi_57373            |              2 |                   0 |                  0 |              1
    datanode3 | public      | udi_57374            |              2 |                   0 |                  0 |              1

show_tsc_info()
---------------

Description: Queries the TimeStamp-Counter (TSC) information obtained from the current database node. This function is supported by version 8.2.1 or later clusters.

Return type: record

.. table:: **Table 4** Parameter

   +-----------------------+---------+---------------------------------------------------------------------+
   | Name                  | Type    | Description                                                         |
   +=======================+=========+=====================================================================+
   | node_name             | text    | Node name                                                           |
   +-----------------------+---------+---------------------------------------------------------------------+
   | tsc_mult              | bigint  | TSC conversion multiplier                                           |
   +-----------------------+---------+---------------------------------------------------------------------+
   | tsc_shift             | bigint  | TSC conversion shifts                                               |
   +-----------------------+---------+---------------------------------------------------------------------+
   | tsc_frequency         | float8  | TSC frequency.                                                      |
   +-----------------------+---------+---------------------------------------------------------------------+
   | tsc_use_freqency      | boolean | Indicates whether to use the TSC frequency for time conversion.     |
   +-----------------------+---------+---------------------------------------------------------------------+
   | tsc_ready             | boolean | Indicates whether the TSC frequency can be used for time conversion |
   +-----------------------+---------+---------------------------------------------------------------------+
   | tsc_scalar_error_info | text    | Error information about obtaining TSC conversion information        |
   +-----------------------+---------+---------------------------------------------------------------------+
   | tsc_freq_error_info   | text    | Error information about obtaining TSC frequency information         |
   +-----------------------+---------+---------------------------------------------------------------------+

Example:

::

   SELECT * FROM show_tsc_info();
     node_name   | tsc_mult | tsc_shift | tsc_frequency | tsc_use_frequency | tsc_ready |     tsc_scalar_error_info     | tsc_freq_error_info
   --------------+----------+-----------+---------------+-------------------+-----------+-------------------------------+---------------------
    coordinator1 |          |           |          2400 | t                 | t         | TSC scalar is not initialized |

get_tsc_info()
--------------

Description: Re-obtains the TimeStamp-Counter (TSC) information of the current database node. This function is supported by version 8.2.1 or later clusters.

Return type: record

.. table:: **Table 5** show_tsc_info() return columns

   +-----------------------+---------+---------------------------------------------------------------------+
   | Column                | Type    | Description                                                         |
   +=======================+=========+=====================================================================+
   | node_name             | text    | Node name                                                           |
   +-----------------------+---------+---------------------------------------------------------------------+
   | tsc_mult              | bigint  | TSC conversion multiplier                                           |
   +-----------------------+---------+---------------------------------------------------------------------+
   | tsc_shift             | bigint  | TSC conversion shifts                                               |
   +-----------------------+---------+---------------------------------------------------------------------+
   | tsc_frequency         | float8  | TSC frequency                                                       |
   +-----------------------+---------+---------------------------------------------------------------------+
   | tsc_use_freqency      | boolean | Indicates whether to use the TSC frequency for time conversion.     |
   +-----------------------+---------+---------------------------------------------------------------------+
   | tsc_ready             | boolean | Indicates whether the TSC frequency can be used for time conversion |
   +-----------------------+---------+---------------------------------------------------------------------+
   | tsc_scalar_error_info | text    | Error information about obtaining TSC conversion information        |
   +-----------------------+---------+---------------------------------------------------------------------+
   | tsc_freq_error_info   | text    | Error information about obtaining TSC frequency information         |
   +-----------------------+---------+---------------------------------------------------------------------+

Example:

::

   SELECT * FROM get_tsc_info();
     node_name   | tsc_mult | tsc_shift | tsc_frequency | tsc_use_frequency | tsc_ready |     tsc_scalar_error_info     | tsc_freq_error_info
   --------------+----------+-----------+---------------+-------------------+-----------+-------------------------------+---------------------
    coordinator1 |          |           |          2400 | t                 | t         | TSC scalar is not initialized |

test_tsc_info(time float8, loops int)
-------------------------------------

Description: Tests the accuracy of the time converted using the TimeStamp-Counter (TSC) on the current node. This function is supported by version 8.2.1 or later clusters.

The input parameters are described as follows:

-  **time**: indicates the test time difference (unit: s). The test duration must be less than or equal to 60s.
-  **loops**: indicates the number of tests. The value ranges from 1 to 10.

Return type: record

Fields in the returned value:

-  **id**: number of cycles.
-  **real_time_diff**: time difference obtained using **gettimeofday** (unit: us).
-  **est_time_scalar**: time difference (unit: μs) converted using TSC conversion information.
-  **est_time_frequency**: time difference (unit: μs) converted using the TSC frequency.

Example:

::

   SELECT * FROM test_tsc_info(0.01,10);
    id | real_time_diff | est_time_scalar | est_time_frequency
   ----+----------------+-----------------+--------------------
     1 |          10057 |                 |            10056.9
     2 |          10057 |                 |   10057.4816666667
     3 |          10056 |                 |   10055.2841666667
     4 |          10054 |                 |   10054.4908333333
     5 |          10055 |                 |         10054.2875
     6 |          10055 |                 |   10054.7483333333
     7 |          10055 |                 |         10054.4725
     8 |          10054 |                 |   10054.0766666667
     9 |          10058 |                 |   10058.1016666667
    10 |          10057 |                 |   10056.3733333333
   (10 rows)
