:original_name: dws_06_0062.html

.. _dws_06_0062:

Other Functions
===============

pgxc_pool_check()
-----------------

Description: Checks whether the connection data buffered in the pool is consistent with **pgxc_node**.

Return type: boolean

Example:

::

   SELECT pgxc_pool_check();
    pgxc_pool_check
   -----------------
    t
   (1 row)

pgxc_pool_reload()
------------------

Description: Updates the connection information buffered in the pool.

Return type: boolean

Example:

::

   SELECT pgxc_pool_reload();
    pgxc_pool_reload
   ------------------
    t
   (1 row)

pg_pool_validate(clear boolean, co_node_name cstring)
-----------------------------------------------------

Description: Clears invalid backend threads on a CN. (These backend threads hold invalid pooler connections to standby DNs.)

Return type: record

pg_nodes_memory()
-----------------

Description: queries the memory usage of all nodes.

Return type: record

Example:

::

   SELECT * FROM pg_nodes_memory();
         node_name       | used_memory |        shared_buffer_cache         |      top_context_memory
   ----------------------+-------------+------------------------------------+------------------------------
    dn_6003_6004         | 353 MB      | 108 MB(Utilization: 512 MB/21.00%) | PgStat BackendStatus(101 MB)
                         |             |                                    | TopMemoryContext(59 MB)
                         |             |                                    | gs_signal(56 MB)
    dn_6005_6006         | 353 MB      | 202 MB(Utilization: 512 MB/39.00%) | PgStat BackendStatus(101 MB)
                         |             |                                    | TopMemoryContext(59 MB)
                         |             |                                    | gs_signal(56 MB)
    dn_6001_6002         | 351 MB      | 201 MB(Utilization: 512 MB/39.00%) | PgStat BackendStatus(101 MB)
                         |             |                                    | TopMemoryContext(58 MB)
                         |             |                                    | gs_signal(56 MB)
    cn_5001              | 79 MB       | 48 MB(Utilization: 256 MB/19.00%)  | CacheMemoryContext(22 MB)
                         |             |                                    | PgStat BackendStatus(19 MB)
                         |             |                                    | gs_signal(16 MB)
    cn_5002              | 77 MB       | 95 MB(Utilization: 256 MB/37.00%)  | CacheMemoryContext(21 MB)
                         |             |                                    | PgStat BackendStatus(19 MB)
                         |             |                                    | gs_signal(16 MB)
    cn_5003              | 67 MB       | 50 MB(Utilization: 256 MB/19.00%)  | CacheMemoryContext(26 MB)
                         |             |                                    | gs_signal(16 MB)
                         |             |                                    | TopMemoryContext(9732 KB)
   (18 rows)

plan_seed()
-----------

Description: Obtains the seed value of the previous query statement (internal use).

Return type: integer

Example:

::

   SELECT plan_seed();
    plan_seed
   -----------
            0
   (1 row)

pg_stat_get_env()
-----------------

Description: Obtains the environment variable information about the current node.

Return type: record

Example:

::

   SELECT * FROM  pg_stat_get_env();
    node_name |   host    | process | port |   installpath    |        datapath        |            log_directory
   -----------+-----------+---------+------+------------------+------------------------+--------------------------------------
    cn_5003   | localhost |   28811 | 8000 | /DWS/manager/app | /DWS/data1/coordinator | /DWS/manager/log/Ruby/pg_log/cn_5003
   (1 row)

pg_stat_get_thread()
--------------------

Description: Provides information about the status of all threads under the current node.

Return type: record

Example:

::

   SELECT * FROM pg_stat_get_thread();
    node_name |       pid       |  lwpid  |    thread_name     |         creation_time
   -----------+-----------------+---------+--------------------+-------------------------------
    cn_5003   | 281471515199536 |   28930 | JobScheduler       | 2023-01-04 07:03:16.086885+00
    cn_5003   | 281471498418224 |   28931 | StatCollector      | 2000-01-01 00:00:00+00
    cn_5003   | 281471464855600 |   28933 | WDRSnapshot        | 2023-01-04 07:03:16.086775+00
    cn_5003   | 281471380949040 |   28938 | WorkloadMonitor    | 2023-01-04 07:03:16.074454+00
    cn_5003   | 281471414511664 |   28936 | workload           | 2023-01-04 07:03:16.075457+00
    cn_5003   | 281471364167728 |   28939 | WLMArbiter         | 2023-01-04 07:03:16.076753+00
    cn_5003   | 281471397730352 |   28937 | CalculateSpaceInfo | 2023-01-04 07:03:16.078981+00
    cn_5003   | 281470777964592 | 1933534 | wlm                | 2023-01-13 08:01:32.350808+00
    cn_5003   | 281470889130032 | 1786064 | cn_5002            | 2023-01-13 07:01:50.173568+00
    cn_5003   | 281471299672112 |   29006 | cm_agent           | 2023-01-04 07:03:18.03415+00
    cn_5003   | 281471222065200 |   29970 | cn_5002            | 2023-01-04 07:03:39.694702+00
    cn_5003   | 281471238846512 | 1897367 | cn_5002            | 2023-01-04 20:01:40.611019+00
    cn_5003   | 281470905911344 |   30053 | cn_5002            | 2023-01-04 07:03:44.065774+00
    cn_5003   | 281470410537008 | 1933902 | cn_5002            | 2023-01-13 08:01:38.972574+00
    cn_5003   | 281470872348720 | 1880248 | cn_5001            | 2023-01-13 07:39:24.231418+00
    cn_5003   | 281471316453424 | 1883059 | cn_5001            | 2023-01-13 07:40:16.885667+00
    cn_5003   | 281470845081648 | 1305053 | cn_5001            | 2023-01-13 03:40:17.366784+00
    cn_5003   | 281470700357680 | 1500466 | wlm                | 2023-01-13 05:02:05.714544+00
    cn_5003   | 281470473455664 | 1883060 | cn_5001            | 2023-01-13 07:40:16.885963+00
    cn_5003   | 281470717138992 |   32065 | cm_agent           | 2023-01-04 07:04:23.906691+00
    cn_5003   | 281470807328816 | 1977925 | gsql               | 2023-01-13 08:20:04.509437+00
    cn_5003   | 281470683576368 | 1835242 | cn_5001            | 2023-01-13 07:20:16.549546+00
    cn_5003   | 281471584946224 |   28927 | Background writer  | 2023-01-04 07:03:16.065631+00
    cn_5003   | 281471633184816 |   28926 | CheckPointer       | 2023-01-04 07:03:16.065872+00
    cn_5003   | 281471548762160 |   28928 | Wal Writer         | 2023-01-04 07:03:16.066366+00
    cn_5003   | 281471448074288 |   28934 | TwoPhase Cleaner   | 2023-01-04 07:03:16.071172+00
    cn_5003   | 281471431292976 |   28935 | LWLock Monitor     | 2023-01-04 07:03:16.072897+00
    cn_5003   | 281470666795056 | 1210459 | CBM Writer         | 2023-01-04 15:16:05.543143+00
   (28 rows)

pgxc_get_os_threads()
---------------------

Description: Provides information about the status of threads under all normal nodes in a cluster.

Return type: record

pg_stat_get_sql_count()
-----------------------

Description: Provides statistics on the number of **SELECT**/**UPDATE**/**INSERT**/**DELETE**/**MERGE INTO** statements executed by all users on the current node, response time, and the number of DDL, DML, and DCL statements.

Return type: record

Example:

::

   SELECT * FROM pg_stat_get_sql_count();
    node_name |       user_name        | select_count | update_count | insert_count | delete_count | mergeinto_count | ddl_count | dml_count | dcl_count | total_select_elapse | avg_select_elapse | max_select_el
   apse | min_select_elapse | total_update_elapse | avg_update_elapse | max_update_elapse | min_update_elapse | total_insert_elapse | avg_insert_elapse | max_insert_elapse | min_insert_elapse | total_delete_ela
   pse | avg_delete_elapse | max_delete_elapse | min_delete_elapse
   -----------+------------------------+--------------+--------------+--------------+--------------+-----------------+-----------+-----------+-----------+---------------------+-------------------+--------------
   -----+-------------------+---------------------+-------------------+-------------------+-------------------+---------------------+-------------------+-------------------+-------------------+-----------------
   ----+-------------------+-------------------+-------------------
    cn_5003   | gs_role_read_all_stats |            0 |            0 |            0 |            0 |               0 |         0 |         0 |         0 |                   0 |                 0 |
      0 |                 0 |                   0 |                 0 |                 0 |                 0 |                   0 |                 0 |                 0 |                 0 |
     0 |                 0 |                 0 |                 0
    cn_5003   | gs_role_signal_backend |            0 |            0 |            0 |            0 |               0 |         0 |         0 |         0 |                   0 |                 0 |
      0 |                 0 |                   0 |                 0 |                 0 |                 0 |                   0 |                 0 |                 0 |                 0 |
     0 |                 0 |                 0 |                 0
    cn_5003   | gs_role_analyze_any    |            0 |            0 |            0 |            0 |               0 |         0 |         0 |         0 |                   0 |                 0 |
      0 |                 0 |                   0 |                 0 |                 0 |                 0 |                   0 |                 0 |                 0 |                 0 |
     0 |                 0 |                 0 |                 0
    cn_5003   | gs_role_vacuum_any     |            0 |            0 |            0 |            0 |               0 |         0 |         0 |         0 |                   0 |                 0 |
      0 |                 0 |                   0 |                 0 |                 0 |                 0 |                   0 |                 0 |                 0 |                 0 |
     0 |                 0 |                 0 |                 0
    cn_5003   | dbadmin                |          641 |            0 |            3 |            0 |               0 |        18 |       651 |         5 |            19236129 |             30009 |           814
   0206 |               357 |                   0 |                 0 |                 0 |                 0 |               70595 |             23531 |             62102 |              2750 |
     0 |                 0 |                 0 |                 0
    cn_5003   | Ruby                   |      2078187 |         3263 |        22841 |            0 |               0 |     10436 |   2242517 |     16979 |          3753441293 |              1806 |            52
   6891 |               191 |            67483165 |             20681 |             38076 |             15444 |           291598980 |             12766 |             35376 |              3791 |
     0 |                 0 |                 0 |                 0
    cn_5003   | joe                    |            0 |            0 |            0 |            0 |               0 |         0 |         0 |         0 |                   0 |                 0 |
      0 |                 0 |                   0 |                 0 |                 0 |                 0 |                   0 |                 0 |                 0 |                 0 |
     0 |                 0 |                 0 |                 0
    cn_5003   | sea                    |          192 |            0 |            5 |            3 |               0 |         3 |       205 |         0 |             2561878 |             13343 |            68
   1866 |               388 |                   0 |                 0 |                 0 |                 0 |               11349 |              2269 |              3241 |              1521 |               19
   255 |              6418 |             10656 |              2798
    cn_5003   | jj                     |            0 |            0 |            0 |            0 |               0 |         0 |         0 |         0 |                   0 |                 0 |
      0 |                 0 |                   0 |                 0 |                 0 |                 0 |                   0 |                 0 |                 0 |                 0 |
     0 |                 0 |                 0 |                 0
    cn_5003   | u1                     |            2 |            0 |            2 |            0 |               0 |         1 |         4 |         0 |                3712 |              1856 |
   2407 |              1305 |                   0 |                 0 |                 0 |                 0 |                5366 |              2683 |              3359 |              2007 |
     0 |                 0 |                 0 |                 0
   (10 rows)

pgxc_get_sql_count()
--------------------

Description: Provides statistics on the number of **SELECT**/**UPDATE**/**INSERT**/**DELETE**/**MERGE INTO** statements executed by all users on all nodes of the current cluster, response time, and the number of DDL, DML, and DCL statements.

Return type: record

pgxc_get_workload_sql_count()
-----------------------------

Description: Provides statistics on the number of **SELECT**/**UPDATE**/**INSERT**/**DELETE** statements executed in all workload Cgroup on all CNs of the current cluster and the number of DDL, DML, and DCL statements.

Return type: record

Example:

::

   SELECT * FROM pgxc_get_workload_sql_count();
    node_name |   workload   | select_count | update_count | insert_count | delete_count | ddl_count | dml_count | dcl_count
   -----------+--------------+--------------+--------------+--------------+--------------+-----------+-----------+-----------
    cn_5003   | default_pool |      2079352 |         3264 |        22858 |            3 |     10460 |   2243738 |     16988
    cn_5001   | default_pool |      2201345 |            9 |            0 |            0 |     10474 |   2359633 |     10465
    cn_5002   | default_pool |      3784696 |            0 |       103106 |          136 |     10438 |   4039090 |     10498
   (3 rows)

pgxc_get_workload_sql_elapse_time()
-----------------------------------

Description: Provides statistics on response time of **SELECT**/**UPDATE**/**INSERT**/**DELETE** statements executed in all workload Cgroup on all CNs of the current cluster.

Return type: record

Example:

::

   SELECT * FROM pgxc_get_workload_sql_elapse_time();
    node_name |   workload   | total_select_elapse | max_select_elapse | min_select_elapse | avg_select_elapse | total_update_elapse | max_update_elapse | min_update_elapse | avg_update_elapse | total_insert_el
   apse | max_insert_elapse | min_insert_elapse | avg_insert_elapse | total_delete_elapse | max_delete_elapse | min_delete_elapse | avg_delete_elapse
   -----------+--------------+---------------------+-------------------+-------------------+-------------------+---------------------+-------------------+-------------------+-------------------+----------------
   -----+-------------------+-------------------+-------------------+---------------------+-------------------+-------------------+-------------------
    cn_5003   | default_pool |          3776420502 |           8140206 |                 0 |              1816 |            67505332 |             38076 |                 0 |             20682 |           29178
   9830 |             62102 |                 0 |             12765 |               19255 |             10656 |                 0 |              6418
    cn_5001   | default_pool |          8599339496 |           3390159 |                 0 |              3906 |               52789 |             18207 |                 0 |              5865 |
      0 |                 0 |                 0 |                 0 |                   0 |                 0 |                 0 |                 0
    cn_5002   | default_pool |         40483096221 |           2178781 |                 0 |             10695 |                   0 |                 0 |                 0 |                 0 |        13310238
   8148 |           2398854 |                 0 |           1290752 |             2072031 |             52877 |                 0 |             15236
   (3 rows)

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

Example:

::

   SELECT * FROM pgxc_get_node_env();
     node_name   |     host      | process | port  |   installpath    |         datapath          |            log_directory
   --------------+---------------+---------+-------+------------------+---------------------------+--------------------------------------
    dn_6001_6002 | 172.16.102.5  |   24443 | 40000 | /DWS/manager/app | /DWS/data1/h0dn1/primary0 | /DWS/manager/log/Ruby/pg_log/dn_6001
    dn_6003_6004 | 172.16.70.17  |   21823 | 40000 | /DWS/manager/app | /DWS/data1/h1dn1/primary0 | /DWS/manager/log/Ruby/pg_log/dn_6003
    dn_6005_6006 | 172.16.120.50 |   22331 | 40000 | /DWS/manager/app | /DWS/data1/h2dn1/primary0 | /DWS/manager/log/Ruby/pg_log/dn_6005
    cn_5003      | localhost     |   28811 |  8000 | /DWS/manager/app | /DWS/data1/coordinator    | /DWS/manager/log/Ruby/pg_log/cn_5003
    cn_5001      | 172.16.102.5  |   30873 |  8000 | /DWS/manager/app | /DWS/data1/coordinator    | /DWS/manager/log/Ruby/pg_log/cn_5001
    cn_5002      | 172.16.70.17  |   29229 |  8000 | /DWS/manager/app | /DWS/data1/coordinator    | /DWS/manager/log/Ruby/pg_log/cn_5002
   (6 rows)

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

Example:

::

   SELECT * FROM pgxc_stat_bgwriter();
     node_name   | checkpoints_timed | checkpoints_req | checkpoint_write_time | checkpoint_sync_time | buffers_checkpoint | buffers_clean | maxwritten_clean | buffers_backend | buffers_backend_fsync | buffers_
   alloc |      stats_reset
   --------------+-------------------+-----------------+-----------------------+----------------------+--------------------+---------------+------------------+-----------------+-----------------------+---------
   ------+------------------------
    dn_6001_6002 |                 0 |               0 |                     0 |                    0 |                  0 |             0 |                0 |               0 |                     0 |
       0 | 2000-01-01 00:00:00+00
    dn_6003_6004 |                 0 |               0 |                     0 |                    0 |                  0 |             0 |                0 |               0 |                     0 |
       0 | 2000-01-01 00:00:00+00
    dn_6005_6006 |                 0 |               0 |                     0 |                    0 |                  0 |             0 |                0 |               0 |                     0 |
       0 | 2000-01-01 00:00:00+00
    cn_5003      |                 0 |               0 |                     0 |                    0 |                  0 |             0 |                0 |               0 |                     0 |
       0 | 2000-01-01 00:00:00+00
    cn_5001      |                 0 |               0 |                     0 |                    0 |                  0 |             0 |                0 |               0 |                     0 |
       0 | 2000-01-01 00:00:00+00
    cn_5002      |                 0 |               0 |                     0 |                    0 |                  0 |             0 |                0 |               0 |                     0 |
       0 | 2000-01-01 00:00:00+00
   (6 rows)

pgxc_stat_replication()
-----------------------

Description: queries information about the log synchronization status on each node in a cluster, such as the location where the logs are sent and received. For details about the returned results, see "System Catalogs > System Views > PG_STAT_REPLICATION" in the *Developer Guide*.

Return type: record

Example:

::

   SELECT * FROM pgxc_stat_replication();
     node_name   |       pid       | usesysid | usename |    application_name    |  client_addr  | client_hostname | client_port |         backend_start         |   state   | sender_sent_location | receiver_wri
   te_location | receiver_flush_location | receiver_replay_location | sync_priority | sync_state
   --------------+-----------------+----------+---------+------------------------+---------------+-----------------+-------------+-------------------------------+-----------+----------------------+-------------
   ------------+-------------------------+--------------------------+---------------+------------
    dn_6001_6002 | 281469637695536 |       10 | Ruby    | WalSender to Standby   | 172.16.70.17  |                 |       26084 | 2023-01-04 07:01:27.348647+00 | Streaming | 0/4940D6B8           | 0/4940D6B8
               | 0/4940D6B8              | 0/4940D6B8               |             1 | Sync
    dn_6001_6002 | 281469304735792 |       10 | Ruby    | WalSender to Secondary | 172.16.120.50 |                 |       35214 | 2023-01-04 07:01:29.51929+00  | Streaming | 0/4000000            | 0/4000000
               | 0/4000000               | 0/4000000                |             0 | Sync
    dn_6003_6004 | 281469634050096 |       10 | Ruby    | WalSender to Standby   | 172.16.120.50 |                 |       13072 | 2023-01-04 07:01:26.28706+00  | Streaming | 0/493EF000           | 0/493EF000
               | 0/493EF000              | 0/493EF000               |             1 | Sync
    dn_6003_6004 | 281469563295792 |       10 | Ruby    | WalSender to Secondary | 172.16.102.5  |                 |       55068 | 2023-01-04 07:01:29.310595+00 | Streaming | 0/4000000            | 0/4000000
               | 0/4000000               | 0/4000000                |             0 | Sync
    dn_6005_6006 | 281470349690928 |       10 | Ruby    | WalSender to Standby   | 172.16.102.5  |                 |       40376 | 2023-01-04 07:01:26.768434+00 | Streaming | 0/49415A70           | 0/49415A70
               | 0/49415A70              | 0/49415A70               |             1 | Sync
    dn_6005_6006 | 281470010435632 |       10 | Ruby    | WalSender to Secondary | 172.16.70.17  |                 |       33750 | 2023-01-04 07:01:29.499269+00 | Streaming | 0/4000000            | 0/4000000
               | 0/4000000               | 0/4000000                |             0 | Sync
   (6 rows)

pgxc_replication_slots()
------------------------

Description: queries the replication status on each DN in a cluster. For details about the returned results, see "System Catalogs > System Views > PG_REPLICATION_SLOTS" in the *Developer Guide*.

Return type: record

Example:

::

   SELECT * FROM pgxc_replication_slots();
     node_name   |    slot_name    | plugin | slot_type | datoid | database | active | x_min | catalog_xmin |    restart_lsn    | dummy_standby
   --------------+-----------------+--------+-----------+--------+----------+--------+-------+--------------+-------------------+---------------
    dn_6001_6002 | dn_6001_3002    |        | physical  |      0 |          | t      |       |              |                   | t
    dn_6001_6002 | dn_6001_6002    |        | physical  |      0 |          | t      |       |              | 0/49448720        | f
    dn_6001_6002 | gs_roach_common |        | physical  |      0 |          | f      |       |    635143447 | FFFFFFFF/FFFFFFFF | f
    dn_6003_6004 | dn_6003_3003    |        | physical  |      0 |          | t      |       |              |                   | t
    dn_6003_6004 | dn_6003_6004    |        | physical  |      0 |          | t      |       |              | 0/4942B760        | f
    dn_6003_6004 | gs_roach_common |        | physical  |      0 |          | f      |       |    634883623 | FFFFFFFF/FFFFFFFF | f
    dn_6005_6006 | dn_6005_3004    |        | physical  |      0 |          | t      |       |              |                   | t
    dn_6005_6006 | dn_6005_6006    |        | physical  |      0 |          | t      |       |              | 0/4944CE80        | f
    dn_6005_6006 | gs_roach_common |        | physical  |      0 |          | f      |       |    635285455 | FFFFFFFF/FFFFFFFF | f
   (9 rows)

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

Example:

::

   SELECT * FROM pg_stat_get_redo_stat();
    phywrts | phyblkwrt | writetim | avgiotim | lstiotim | miniotim | maxiowtm
   ---------+-----------+----------+----------+----------+----------+----------
     400171 |    552783 | 11040710 |       27 |       20 |        8 |     7401
   (1 row)

pgxc_redo_stat()
----------------

Description: queries the Xlog redo statistics of each node in a cluster. For details about the returned results, see "System Catalogs > System Views > PV_REDO_STAT" in the *Developer Guide*.

Return type: record

Example:

.. code-block::

   SELECT * FROM  pgxc_redo_stat();
     node_name   | phywrts | phyblkwrt | writetim | avgiotim | lstiotim | miniotim | maxiowtm
   --------------+---------+-----------+----------+----------+----------+----------+----------
    dn_6001_6002 |  698244 |    836088 | 17608019 |       25 |       15 |        8 |    13115
    dn_6003_6004 |  661128 |    799636 | 16714302 |       25 |       21 |        8 |     8195
    dn_6005_6006 |  698146 |    836178 | 18117951 |       25 |       24 |        8 |     8326
    cn_5003      |  400206 |    552823 | 11041701 |       27 |       18 |        8 |     7401
    cn_5001      |  380931 |    514233 | 10174114 |       26 |       19 |        8 |     7726
    cn_5002      |  551727 |    687991 | 11859292 |       21 |       31 |        8 |    10310
   (6 rows)

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

.. _en-us_topic_0000001233510125__section1032165918182:

pgxc_node_stat_reset_time()
---------------------------

Description: queries the time when the statistics of each node in a cluster are reset. For details about the returned result, see "System Catalogs > System Views > GS_NODE_STAT_RESET_TIME" in the *Developer Guide*.

Return type: record

.. note::

   When an instance is running, its statistics keep rising. In the following cases, the statistical values in the memory will be reset to **0**:

   -  The instance is restarted or a cluster switchover occurs.
   -  The database is deleted.
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

generate_wdr_report(begin_snap_id bigint, end_snap_id bigint, report_type cstring, report_scope cstring, node_name cstring)
---------------------------------------------------------------------------------------------------------------------------

Description: Creates a load analysis report. The input parameters are described as follows:

-  **begin_snap_id** and **end_snap_id**: IDs of the start and end snapshots, respectively. The IDs are of the bigint type. The value of **begin_snap_id** must be less than that of **end_snap_id**, and the time for the start and end snapshots cannot overlap. You can check whether the snapshot time overlaps by querying **select s1.end_ts < s2.start_ts from (select \* from dbms_om.snapshot where snapshot_id=$begin_snap_id) as s1, (select \* from dbms_om.snapshot where snapshot_id=$end_snap_id) as s2** in the **dbms_om.snapshot** table. If **true** is returned, the snapshot time does not overlap. Otherwise, the snapshot time overlaps.
-  **report_type**: report type. The value is a cstring and can be **summary**, **detail**, or **all**.
-  **report_scope**: report scope. The value is a cstring and can be **cluster** or **node**.
-  **node_name**: node name. The value is a cstring. If **report_scope** is **node**, the value of this parameter must be **pg_catalog**, which indicates the CN or DN name in the **node_name** column of the **pgxc_node** table.

Return type: text

.. note::

   -  Only the database administrator **SYSADMIN** can execute this function.
   -  This function can be executed only on CNs. If it is executed on DNs, the following message will be returned: "WDR report can only be created on coordinator."
   -  If the report is created successfully, message "Report %s has been generated" will be returned.
   -  The statistics cannot be reset between the time the start snapshot is taken and the time the end snapshot is taken. Otherwise, error message "Instance reset time is different" will be displayed. For details about the events that cause a statistics reset, see the :ref:`pgxc_node_stat_reset_time <en-us_topic_0000001233510125__section1032165918182>` function.

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

         SELECT snapshot_id, db_name, schemaname, relname, distribute_mode, seq_scan ,seq_tuple_read ,index_scan ,index_tuple_read ,tuple_inserted
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
