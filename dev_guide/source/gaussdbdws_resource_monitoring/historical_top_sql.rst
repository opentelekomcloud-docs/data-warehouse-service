:original_name: dws_04_0398.html

.. _dws_04_0398:

Historical Top SQL
==================

You can query historical Top SQL in historical resource monitoring views. The historical resource monitoring view records the resource usage (including memory, data spilled to disks, and CPU time), running status (including errors, termination, and exceptions), and performance alarm information when a job is complete. For queries that abnormally terminate due to FATAL or PANIC errors, their status is displayed as **aborted** and no detailed information is recorded. Status information about query parsing in the optimization phase cannot be monitored.

The following table describes the external interfaces of the historical views.

+--------------------------+----------------+-------------------------------------------------------------------------------------------------------------+------------------------------------------------+
| Level                    | Monitored Node | View                                                                                                        |                                                |
+==========================+================+=============================================================================================================+================================================+
| Query/perf (recommended) | Current CN     | History (Internal dump interface. Only statements that have ended in the last three minutes are displayed.) | :ref:`GS_WLM_SESSION_HISTORY <dws_04_0705>`    |
+--------------------------+----------------+-------------------------------------------------------------------------------------------------------------+------------------------------------------------+
|                          |                | History (all statements)                                                                                    | :ref:`GS_WLM_SESSION_INFO <dws_04_0704>`       |
+--------------------------+----------------+-------------------------------------------------------------------------------------------------------------+------------------------------------------------+
|                          | All CNs        | History (Internal dump interface. Only statements that have ended in the last three minutes are displayed.) | :ref:`PGXC_WLM_SESSION_HISTORY <dws_04_0840>`  |
+--------------------------+----------------+-------------------------------------------------------------------------------------------------------------+------------------------------------------------+
|                          |                | History (all statements)                                                                                    | :ref:`PGXC_WLM_SESSION_INFO <dws_04_0839>`     |
+--------------------------+----------------+-------------------------------------------------------------------------------------------------------------+------------------------------------------------+
| Operator level           | Current CN     | History (Only statements that have ended in the last three minutes are displayed.)                          | :ref:`GS_WLM_OPERATOR_HISTORY <dws_04_0702>`   |
+--------------------------+----------------+-------------------------------------------------------------------------------------------------------------+------------------------------------------------+
|                          |                | History (internal dump interface, all statements)                                                           | :ref:`GS_WLM_OPERAROR_INFO <dws_04_0701>`      |
+--------------------------+----------------+-------------------------------------------------------------------------------------------------------------+------------------------------------------------+
|                          | All CNs        | History (Only statements that have ended in the last three minutes are displayed.)                          | :ref:`PGXC_WLM_OPERATOR_HISTORY <dws_04_0836>` |
+--------------------------+----------------+-------------------------------------------------------------------------------------------------------------+------------------------------------------------+
|                          |                | History (internal dump interface, all statements)                                                           | :ref:`PGXC_WLM_OPERATOR_INFO <dws_04_0837>`    |
+--------------------------+----------------+-------------------------------------------------------------------------------------------------------------+------------------------------------------------+

Precautions
-----------

-  The view level is determined by the resource monitoring level, that is, the :ref:`resource_track_level <en-us_topic_0000001510522653__section153571329142612>` configuration.
-  The perf and operator levels affect the values of the **query_plan** and **warning** columns in :ref:`GS_WLM_SESSION_STATISTICS <dws_04_0706>`/:ref:`PGXC_WLM_SESSION_INFO <dws_04_0839>`. For details, see :ref:`SQL Self-Diagnosis <dws_04_0446>`.
-  Prefixes **gs** and **pgxc** indicate views showing single CN information and those showing cluster information, respectively. Common users can log in to a CN in the cluster to query only views with the **gs** prefix.
-  If instance fault occurs, some SQL statement information may fail to be recorded in historical resource monitoring views.
-  In some abnormal cases, the status information column in the historical Top SQL may be displayed as **unknown**. The recorded monitoring information may be inaccurate.
-  The SQL statements that can be recorded in historical resource monitoring views are the same as those recorded in real-time resource monitoring views. For details, see "SQL statements recorded in real-time resource monitoring views" in secition :ref:`Real-time Top SQL <dws_04_0397>`.
-  Historical Top SQL records data only when the GUC parameter :ref:`enable_resource_record <en-us_topic_0000001510522653__s5f116e109a2944e3854abcc56772eaa1>` is enabled.
-  You can query historical Top SQL queries and operator-level data only through the gaussdb database.
-  Historical Top SQL focuses on locating and demarcating query performance problems. It is not used for auditing or recording syntax analysis error statements.
-  In 8.2.1 and later cluster versions, the **resource_track_subsql_duration** parameter (default value: 180s) is added to filter out substatements in the stored procedure whose execution time is less than the value of this parameter and archive only substatements whose execution time is greater than the value of this parameter. In 8.2.1 and later versions, the default value of **enable_track_record_subsql** is changed from **off** to **on**, which means substatements in stored procedures are recorded by default. If a substatement is recorded, it must meet the following conditions:

   -  In the session where the statement is, the **enable_track_record_subsql** parameter is enabled.
   -  The substatement must be pushed down to DNs for execution. (To prevent TopSQL from recording too many substatements, substatements that are not pushed down to DNs will be filtered out.)
   -  The execution time of the substatement exceeds the value of **resource_track_subsql_duration** in the session.

-  By default, the History view queries statements that end in the last 3 minutes. It does this by querying tables. It is actually a temporary view for performance considerations. Since the 8.1.3 cluster version, the real-time monitoring and archiving functions of the TopSQL monitoring have been greatly improved are no performance considerations are needed. Therefore, you are not advised to use the History view.
-  In 8.1.3 and later versions, the TopSQL real-time monitoring has no impact on statement performance. You can set the GUC **parameter resource_track_cost** to **0** to monitor the running information of all statements. The statement archiving in the TopSQL history monitoring also has no impact on statement performance. However, when the TPS is high, the following factors need to be considered:

   -  Record the disk overhead of all statements. You can estimate the disk space required for archiving a statement as 8 KB, calculate the space usage based on the peak TPS, and adjust the values of **resource_track_duration** and **resource_track_subsql_duration**.
   -  For memory overhead for caching all statements, you can estimate the memory size required for archiving a statement as 16 KB, and the interval for archiving statements in batches as 5 seconds, then calculate the required peak memory size based on the peak service TPS. The calculation method is as follows: 5 seconds x TPS x 16 KB. The value of **session_history_memory GUC** (default value: 100 MB) must be greater than the calculation result to ensure that all statements can be recorded.

Prerequisites
-------------

-  The GUC parameter :ref:`enable_resource_track <en-us_topic_0000001510522653__s9530ecdd2b0d4a98b67b66e32bf8e5d0>` is set to **on**. The default value is **on**.
-  The GUC parameter :ref:`resource_track_level <en-us_topic_0000001510522653__section153571329142612>` is set to **query**, **perf**, or **operator**. The default value is **query**. For details, see :ref:`Table 2 <en-us_topic_0000001460722660__table0310615145919>`.
-  The GUC parameter :ref:`enable_resource_record <en-us_topic_0000001510522653__s5f116e109a2944e3854abcc56772eaa1>` is set to **on**. The default value is **on**.
-  The value of the :ref:`resource_track_duration <en-us_topic_0000001510522653__section347574425112>` parameter (**60s** by default) is less than the job execution time.
-  The GUC parameter :ref:`enable_track_record_subsql <en-us_topic_0000001510522653__section7181949101319>` specifies whether to record internal statements of a stored procedure or anonymous block. The default value is **on**.
-  The value of :ref:`resource_track_subsql_duration <en-us_topic_0000001510522653__section177585466812>` is less than the execution time of the internal statement in the stored procedure (180s by default).
-  Jobs whose execution time recorded in the real-time resource monitoring view (see :ref:`Table 1 <en-us_topic_0000001460722660__table16116143418462>`) is greater than or equal to :ref:`resource_track_duration <en-us_topic_0000001510522653__section347574425112>` are monitored.
-  If the Cgroups function is properly loaded, you can run the **gs_cgroup -P** command to view information about Cgroups.

Procedure
---------

#. Query the load records of the current CN after its latest job is complete in the **gs_wlm_session_history** view.

   ::

      SELECT * FROM gs_wlm_session_history;

#. Query the load records of all the CNs after their latest job are complete in the **pgxc_wlm_session_history** view.

   ::

       SELECT * FROM pgxc_wlm_session_history;

#. Query the load records of the current CN through the **gs_wlm_session_info** table after the task is complete. To query the historical records successfully, set :ref:`enable_resource_record <en-us_topic_0000001510522653__s5f116e109a2944e3854abcc56772eaa1>` to **on**.

   ::

      SELECT * FROM gs_wlm_session_info;

   -  Show the 10 queries that consume the most memory (You can specify a query period.):

   ::

      SELECT * FROM gs_wlm_session_info order by max_peak_memory desc limit 10;

   ::

      SELECT * FROM gs_wlm_session_info WHERE start_time >= '2022-05-15 21:00:00' and finish_time <='2022-05-15 23:30:00' order by max_peak_memory desc limit 10;

   -  Show the 10 queries consuming the most CPU resources:

   ::

      SELECT * FROM gs_wlm_session_info order by total_cpu_time desc limit 10;

   ::

      SELECT * FROM gs_wlm_session_info WHERE start_time >= '2022-05-15 21:00:00' and finish_time <='2022-05-15 23:30:00' order by total_cpu_time desc limit 10;

#. Query for the load records of all the CNs after their jobs are complete in the **pgxc_wlm_session_info** view. To query the historical records successfully, set :ref:`enable_resource_record <en-us_topic_0000001510522653__s5f116e109a2944e3854abcc56772eaa1>` to **on**.

   ::

      SELECT * FROM pgxc_wlm_session_info;

   -  Showing the 10 queries on which the CN spends the most time:

   ::

      SELECT * FROM pgxc_wlm_session_info order by duration desc limit 10;

   -  Query the execution information about a query statement that has been executed. For example, query the execution information about the statement whose **queryid** is **76561193695026478**.

   .. code-block::

      SELECT * FROM pgxc_wlm_session_info where queryid = '76561193695026478';

#. Use the **pgxc_get_wlm_session_info_bytime** function to filter and query the **pgxc_wlm_session_info** view. To query the historical records successfully, set :ref:`enable_resource_record <en-us_topic_0000001510522653__s5f116e109a2944e3854abcc56772eaa1>` to **on**. You are advised to use this function if the view contains a large number of records.

   .. note::

      A GaussDB(DWS) cluster uses the UTC time by default, which has an 8-hour time difference with the system time. Before queries, ensure that the database time is the same as the system time.

   -  Return the queries started between **2019-09-10 15:30:00** and **2019-09-10 15:35:00** on all CNs. For each CN, a maximum of 10 queries will be returned.

   ::

      SELECT * FROM pgxc_get_wlm_session_info_bytime('start_time', '2019-09-10 15:30:00', '2019-09-10 15:35:00', 10);

   -  Return the queries ended between **2019-09-10 15:30:00** and **2019-09-10 15:35:00** on all CNs. For each CN, a maximum of 10 queries will be returned.

   ::

      SELECT * FROM pgxc_get_wlm_session_info_bytime('finish_time', '2019-09-10 15:30:00', '2019-09-10 15:35:00', 10);

#. Query the recent resource information of the job operators on the current CN in the **gs_wlm_operator_history** view. Ensure that :ref:`resource_track_level <en-us_topic_0000001510522653__section153571329142612>` is set to **operator**.

   ::

      SELECT * FROM gs_wlm_operator_history;

#. Query the recent resource information of the job operators on all the CNs in the **pgxc_wlm_operator_history** view. Ensure that :ref:`resource_track_level <en-us_topic_0000001510522653__section153571329142612>` is set to **operator**.

   ::

      SELECT * FROM pgxc_wlm_operator_history;

#. Query the recent resource information of the job operators on the current CN in the **gs_wlm_operator_info** view. Ensure that :ref:`resource_track_level <en-us_topic_0000001510522653__section153571329142612>` is set to **operator** and :ref:`enable_resource_record <en-us_topic_0000001510522653__s5f116e109a2944e3854abcc56772eaa1>` to **on**.

   ::

      SELECT * FROM gs_wlm_operator_info;

#. Query for the historical resource information of job operators on all the CNs in the **pgxc_wlm_operator_info** view. Ensure that :ref:`resource_track_level <en-us_topic_0000001510522653__section153571329142612>` is set to **operator** and :ref:`enable_resource_record <en-us_topic_0000001510522653__s5f116e109a2944e3854abcc56772eaa1>` to **on**.

   ::

      SELECT * FROM pgxc_wlm_operator_info;

.. note::

   -  The number of data records that can be retained in the memory is limited due to the preset memory limit. After the real-time query is complete, the data records are imported to historical views. For a query-level view, when the number of queries to be recorded exceeds the upper limit allowed by the memory, the current query cannot be recorded and the next query is performed based on a new rule. On each CN, the memory usage of the query-level historical view is recorded (100 MB by default). You can query the data in the :ref:`PG_TOTAL_MEMORY_DETAIL <dws_04_0788>` view.
   -  For operator-level views, whether a record can be stored depends on the upper limit allowed by the memory at that time point. If the number of plan nodes plus the number of records in the memory exceeds the upper limit, the record cannot be stored. On each CN, the maximum numbers of real-time and historical operator-level records that can be stored in the memory are **max_oper_realt_num** (set to **56987** by default) and **max_oper_hist_num** (set to **113975** by default), respectively. The average number of plan nodes of a query is **num_plan_node**. Maximum number of concurrent tasks allowed by real-time views on each CN is: **num_realt_active** = **max_oper_realt_num**/**num_plan_node**. Maximum number of concurrent tasks allowed by historical views on each CN is: **num_hist_active** = **max_oper_hist_num**/(**180**/**run_time**)/**num_plan_node**.
   -  In high concurrency, ensure that the number of queries to be recorded does not exceed the maximum values set for query- and operator-level views. You can modify the memory of the historical query view by configuring the :ref:`session_history_memory <en-us_topic_0000001510522653__section27306369458>` parameter. The memory size increases in direct proportion to the maximum number of queries that can be recorded.
