:original_name: dws_06_0062.html

.. _dws_06_0062:

Other Functions
===============

-  pgxc_pool_check()

   Description: Checks whether the connection data buffered in the pool is consistent with **pgxc_node**.

   Return type: boolean

-  pgxc_pool_reload()

   Description: Updates the connection information buffered in the pool.

   Return type: boolean

-  pgxc_lock_for_backup()

   Description: Locks the cluster before backup. Backup is performed to restore data on new nodes.

   Return type: boolean

   .. note::

      **pgxc_lock_for_backup** locks a cluster before **gs_dump** or **gs_dumpall** is used to back up the cluster. After a cluster is locked, operations changing the system structure are not allowed. This function does not affect DML statements.

-  pg_pool_validate(clear boolean, co_node_name cstring)

   Description: Clears invalid backend threads on a CN. (These backend threads hold invalid pooler connections to standby DNs.)

   Return type: record

-  pg_nodes_memory()

   Description: queries the memory usage of all nodes.

   Return type: record

-  table_skewness(text)

   Description: queries the percentage of table data among all nodes.

   Parameter: Indicates that the type of the name of the to-be-queried table is text.

   Return type: record

-  table_skewness(table_name text, column_name text[, row_num text])

   Description: Queries the proportion of column data distributed on each node based on the hash distribution rule. The results are sorted based on the data volumes of the nodes.

   Parameters: **table_name** indicates a table name, **column_name** indicates a column name, and **row_num** indicates that all data in the current column is returned. The default value is **0**. A value other than **0** indicates the number of data records whose statistics are sampled. (Records are randomly sampled.)

   Return type: record

   Example:

   Distribute data by hash based on the **a** column in the **tx** table. Seven records are distributed on DN 1, two records on DN 2, and one record on DN 0.

   ::

      select table_skewness('tx','a');
       table_skewness
      ----------------
       (1,7,70.000%)
       (2,2,20.000%)
       (0,1,10.000%)
      (3 rows)

-  table_data_skewness(data_row record, locatorType "char")

   Description: Calculates the bucket distribution index for the records concatenated using the columns in a specified table.

   Parameters: **data_row** indicates the record concatenated using columns in the specified table. **locatorType** indicates the distribution rule. You are advised to set **locatorType** to **H**, indicating hash distribution.

   Return type: smallint

   Example:

   Calculates the bucket distribution index based on the hash distribution rule for the records combined concatenated using the columns in the **tx** table.

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

-  table_distribution(schemaname text, tablename text)

   Description: queries the storage space occupied by a specified table on each node.

   Parameter: Indicates that the types of the schema name and table name for the table to be queried are both text.

   Return type: record

   .. note::

      -  To query for the storage distribution of a specified table by using this function, you must have the **SELECT** permission for the table.
      -  The performance of **table_distribution** is better than that of **table_skewness**. Especially in a large cluster with a large amount of data, **table_distribution** is recommended.
      -  When you use **table_distribution** and want to view the space usage, you can use **dnsize** or **(sum(dnsize) over ())** to view the percentage.

-  table_distribution(regclass)

   Description: queries the storage space occupied by a specified table on each node.

   Parameter: indicates the name or OID of the table to be queried. The table name can be defined by the schema name. Parameter type: regclass

   Return type: record

   .. note::

      -  To query for the storage distribution of a specified table by using this function, you must have the **SELECT** permission for the table.
      -  The performance of **table_distribution** is better than that of **table_skewness**. Especially in a large cluster with a large amount of data, **table_distribution** is recommended.
      -  When you use **table_distribution** and want to view the space usage, you can use **dnsize** or **(sum(dnsize) over ())** to view the percentage.

-  table_distribution()

   Description: queries the storage distribution of all tables in the current database.

   Return type: record

   .. note::

      -  This function involves the query for information about all tables in the database. To execute this function, you must have the administrator rights.
      -  Based on the table_distribution() function, GaussDB(DWS) provides the PGXC_GET_TABLE_SKEWNESS view as an alternative way to query for data skew. You are advised to use this view when the number of tables in the database is less than 10000.

-  pgxc_get_stat_dirty_tables(int dirty_percent, int n_tuples)

   Description: Obtains information about insertion, update, and deletion operations on tables and the dirty page rate of tables. This function optimizes the performance of the **PGXC_GET_STAT_ALL_TABLES** view. It can quickly filter out tables whose dirty page rate is greater than **dirty_percent** and number of dead tuples is greater than **n_tuples**.

   Return type: SETOF record

   The following table describes return columns.

   =============== ============ ==============================
   Name            Type         Description
   =============== ============ ==============================
   relid           oid          Table OID
   relname         name         Table name
   schemaname      name         Schema name of the table
   n_tup_ins       bigint       Number of inserted tuples
   n_tup_upd       bigint       Number of updated tuples
   n_tup_del       bigint       Number of deleted tuples
   n_live_tup      bigint       Number of live tuples
   n_dead_tup      bigint       Number of dead tuples
   dirty_page_rate numeric(5,2) Dirty page rate (%) of a table
   =============== ============ ==============================

-  pgxc_get_stat_dirty_tables(int dirty_percent, int n_tuples, text schema)

   Description: Obtains information about insertion, update, and deletion operations on tables and the dirty page rate of tables. This function can quickly filter out tables whose dirty page rate is greater than **page_dirty_rate**, number of dead tuples is greater than **n_tuples**, and schema name is **schema**.

   Return type: SETOF record

   The return columns of the function are the same as those of the **pgxc_get_stat_dirty_tables(int dirty_percent, int n_tuples)** function.

-  plan_seed()

   Description: Obtains the seed value of the previous query statement (internal use).

   Return type: int

-  pg_stat_get_env()

   Description: Obtains the environment variable information about the current node.

   Return type: record

-  pg_stat_get_thread()

   Description: Provides information about the status of all threads under the current node.

   Return type: record

-  pgxc_get_os_threads()

   Description: Provides information about the status of threads under all normal nodes in a cluster.

   Return type: record

-  pg_stat_get_sql_count()

   Description: Provides statistics on the number of **SELECT**/**UPDATE**/**INSERT**/**DELETE**/**MERGE INTO** statements executed by all users on the current node, response time, and the number of DDL, DML, and DCL statements.

   Return type: record

-  pgxc_get_sql_count()

   Description: Provides statistics on the number of **SELECT**/**UPDATE**/**INSERT**/**DELETE**/**MERGE INTO** statements executed by all users on all nodes of the current cluster, response time, and the number of DDL, DML, and DCL statements.

   Return type: record

-  pgxc_get_workload_sql_count()

   Description: Provides statistics on the number of **SELECT**/**UPDATE**/**INSERT**/**DELETE** statements executed in all workload Cgroup on all CNs of the current cluster and the number of DDL, DML, and DCL statements.

   Return type: record

-  pgxc_get_workload_sql_elapse_time()

   Description: Provides statistics on response time of **SELECT**/**UPDATE**/**INSERT**/**DELETE** statements executed in all workload Cgroup on all CNs of the current cluster.

   Return type: record

-  get_instr_unique_sql()

   Description: Provides information about Unique SQL statistics collected on the current node. If the node is a CN, the system returns the complete information about the Unique SQL statistics collected on the CN. That is, the system collects and summarizes the information about the Unique SQL statistics on other CNs and DNs. If the node is a DN, the Unique SQL statistics on the DN is returned. For details, see GS_INSTR_UNIQUE_SQL.

   Return type: record

-  reset_instr_unique_sql(cstring, cstring, INT8)

   Description: Clears collected Unique SQL statistics. The input parameters are described as follows:

   -  **GLOBAL**/**LOCAL**: Data is cleared from all nodes or the current node.
   -  **ALL**/**BY_USERID**/**BY_CNID**/**BY_GUC**: **ALL** indicates that all data is cleared. **BY_USERID/BY_CNID** indicates that data is cleared by **USERID** or **CNID**. **BY_GUC** indicates that the clearance operation is caused by the decrease of the value of the GUC parameter **instr_unique_sql_count**.
   -  The third parameter corresponds to the second parameter. The parameter is invalid for **ALL**/**BY_GUC**.

   Return type: bool

-  pgxc_get_instr_unique_sql()

   Description: Provides complete information about Unique SQL statistics collected on all CNs in a cluster. This function can be executed only on CNs.

   Return type: record

-  get_instr_unique_sql_remote_cns()

   Description: Provides complete information about Unique SQL statements collected on all CNs in the cluster, except the CN on which the function is being executed. This function can be executed only on CNs.

   Return type: record

-  pgxc_get_node_env()

   Description: Provides the environment variable information about all nodes in a cluster.

   Return type: record

-  gs_switch_relfilenode()

   Description: Exchanges meta information of two tables or partitions. (This is only used for the redistribution tool. An error message is displayed when the function is directly used by users).

   Return type: int

-  copy_error_log_create()

   Description: Creates the error table (**public.pgxc_copy_error_log**) required for creating the **COPY FROM** error tolerance mechanism.

   Return type: boolean

   .. note::

      -  This function attempts to create the **public.pgxc_copy_error_log** table. For details about the table, see :ref:`Table 1 <en-us_topic_0000001098990696__table138318280213>`.
      -  Create the B-tree index on the **relname** column and execute **REVOKE ALL on public.pgxc_copy_error_log FROM public** to manage permissions for the error table (the permissions are the same as those of the **COPY** statement).
      -  **public.pgxc_copy_error_log** is a row-store table. Therefore, this function can be executed and **COPY FROM** error tolerance is available only when row-store tables can be created in the cluster. After the GUC parameter **enable_hadoop_env** is enabled, row-based tables cannot be created in the cluster. The default value is **off**.
      -  Same as the error table and the **COPY** statement, the function requires **sysadmin** or higher permissions.
      -  If the **public.pgxc_copy_error_log** table or the **copy_error_log_relname_idx** index already exists before the function creates it, the function will report an error and roll back.

   .. _en-us_topic_0000001098990696__table138318280213:

   .. table:: **Table 1** Error table public.pgxc_copy_error_log

      +-----------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
      | Column    | Type                     | Description                                                                                                                                         |
      +===========+==========================+=====================================================================================================================================================+
      | relname   | varchar                  | Table name in the form of *Schema name*\ **.**\ *Table name*                                                                                        |
      +-----------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
      | begintime | timestamp with time zone | Time when a data format error was reported                                                                                                          |
      +-----------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
      | filename  | character varying        | Name of the source data file where a data format error occurs                                                                                       |
      +-----------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
      | rownum    | bigint                   | Number of the row where a data format error occurs in a source data file                                                                            |
      +-----------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
      | rawrecord | text                     | Raw record of a data format error in the source data file To prevent a field from being too long, the length of the field cannot exceed 1024 bytes. |
      +-----------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
      | detail    | text                     | Error details                                                                                                                                       |
      +-----------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+

-  pv_compute_pool_workload()

   Description: Provides the current load information about computing Node Groups on cloud.

   Return type: record

-  pg_stat_get_status(tid, num_node_display)

   Description: Queries for the blocking and waiting status of the backend threads and auxiliary threads in the current instance. For details about the returned results, see the PG_THREAD_WAIT_STATUS view. The input parameters are described as follows:

   -  **tid**: thread ID, which is of the bigint type. If this parameter is null, the waiting statuses of all backend threads and auxiliary threads are returned. Otherwise, only the waiting statuses of threads with the specified IDs are returned.
   -  **num_node_display**: integer type. Specifies the maximum number of waiting nodes displayed in the **wait_status** column for records whose waiting status is **wait node**.

      -  If this parameter is left empty or set to a value less than or equal to **0**, only one waiting node is displayed.
      -  If the value is greater than **20**, a maximum number of nodes can be displayed is **20**.
      -  If the value is greater than **0** and less than or equal to **20**, the smaller value between **num_node_display** and the actual number of waiting nodes is displayed. Use the **SELECT \* from pg_stat_get_status(NULL, 10)** query for example. If the number of waiting nodes is greater than **10**, the names of only 10 nodes are displayed randomly. If the number of waiting nodes is less than or equal to **10**, the names of all waiting nodes are displayed. If the number of waiting nodes is greater than the number of displayed nodes, the displayed node names are randomly selected.

   Return type: record

-  pgxc_get_thread_wait_status(num_node_display)

   Description: Queries for the call hierarchy between threads generated by all SQL statements on each node in a cluster, as well as the block waiting status of each thread. For details about the returned results, see the PGXC_THREAD_WAIT_STATUS view. The type and meaning of the input parameter **num_node_display** are the same as those of the **pg_stat_get_status** function.

   Return type: record

-  pgxc_os_run_info()

   Description: Obtains the running status of the operating system on each node in a cluster. For details about the returned results, see "System Catalogs > System Views >PV_OS_RUN_INFO" in the *Developer Guide*.

   Return type: record

-  get_instr_wait_event()

   Description: Obtains the waiting status and events of the current instance. For details about the returned results, see "System Catalogs > System Views > GS_WAIT_EVENTS" in the *Developer Guide*. If the GUC parameter **enable_track_wait_event** is **off**, this function returns **0**.

   Return type: record

-  pgxc_wait_events()

   Description: queries statistics about waiting status and events on each node in a cluster. For details about the returned results, see "System Catalogs > System Views > PGXC_WAIT_EVENTS" in the *Developer Guide*. If the GUC parameter **enable_track_wait_event** is **off**, this function returns **0**.

   Return type: record

-  pgxc_stat_bgwriter()

   Description: queries statistics about backend write processes on each node in a cluster. For details about the returned results, see "System Catalogs > System Views > PG_STAT_BGWRITER" in the *Developer Guide*.

   Return type: record

-  pgxc_stat_replication()

   Description: queries information about the log synchronization status on each node in a cluster, such as the location where the logs are sent and received. For details about the returned results, see "System Catalogs > System Views > PG_STAT_REPLICATION" in the *Developer Guide*.

   Return type: record

-  pgxc_replication_slots()

   Description: queries the replication status on each DN in a cluster. For details about the returned results, see "System Catalogs > System Views > PG_REPLICATION_SLOTS" in the *Developer Guide*.

   Return type: record

-  pgxc_settings()

   Description: queries information about runtime parameters on each node in a cluster. For details about the returned results, see "System Catalogs > System Views > PG_SETTINGS" in the *Developer Guide*.

   Return type: record

-  pgxc_instance_time()

   Description: queries the running time statistics of each node in a cluster and the time consumed in each execution phase. For details about the returned results, see "System Catalogs > System Views > PV_INSTANCE_TIME" in the *Developer Guide*.

   Return type: record

-  pg_stat_get_redo_stat()

   Description: queries Xlog redo statistics on the current node. For details about the returned results, see "System Catalogs > System Views > PV_REDO_STAT" in the *Developer Guide*.

   Return type: record

-  pgxc_redo_stat()

   Description: queries the Xlog redo statistics of each node in a cluster. For details about the returned results, see "System Catalogs > System Views > PV_REDO_STAT" in the *Developer Guide*.

   Return type: record

-  get_local_rel_iostat()

   Description: Obtains the disk I/O statistics of the current instance. For details about the returned results, see "System Catalogs > System Views > GS_REL_IOSTAT" in the *Developer Guide*.

   Return type: record

-  pgxc_rel_iostat()

   Description: queries the disk I/O statistics on each node in a cluster. For details about the returned result, see "System Catalogs > System Views > GS_REL_IOSTAT" in the *Developer Guide*.

   Return type: record

-  get_node_stat_reset_time()

   Description: Obtains the time when statistics of the current instance were reset.

   Return type: timestamptz

-  pgxc_node_stat_reset_time()

   Description: queries the time when the statistics of each node in a cluster are reset. For details about the returned result, see "System Catalogs > System Views > GS_NODE_STAT_RESET_TIME" in the *Developer Guide*.

   Return type: record

   .. note::

      When an instance is running, its statistics keep rising. In the following cases, the statistical values in the memory will be reset to **0**:

      -  The instance is restarted or a cluster switchover occurs.
      -  The database is deleted.
      -  A reset operation is performed. For example, the statistics counter in the database is reset using the **pgstat_recv_resetcounter** function or the Unique SQL statements are cleared using the **reset_instr_unique_sql** function.

      If any of the preceding events occurs, GaussDB(DWS) will record the time when the statistics are reset. You can query the time using the **get_node_stat_reset_time** function.
