:original_name: dws_04_0398.html

.. _dws_04_0398:

Historical TopSQL
=================

You can query historical Top SQL in historical resource monitoring views. The historical resource monitoring view records the resource usage (of memory, disk, CPU time, and I/O), running status (including errors, termination, and exceptions), and performance alarm information during job running. For queries that abnormally terminate due to FATAL or PANIC errors, their status is displayed as **aborted** and no detailed information is recorded. Status information about query parsing in the optimization phase cannot be monitored.

The following table describes the external interfaces of the historical views.

+------------------------+----------------+--------------------------------------+------------------------------------------------+
| Level                  | Monitored Node | View                                 |                                                |
+========================+================+======================================+================================================+
| Query level/perf level | Current CN     | History (Database Manager interface) | :ref:`GS_WLM_SESSION_HISTORY <dws_04_0705>`    |
+------------------------+----------------+--------------------------------------+------------------------------------------------+
|                        |                | History (internal dump interface)    | :ref:`GS_WLM_SESSION_INFO <dws_04_0704>`       |
+------------------------+----------------+--------------------------------------+------------------------------------------------+
|                        | All CNs        | History (Database Manager interface) | :ref:`PGXC_WLM_SESSION_HISTORY <dws_04_0840>`  |
+------------------------+----------------+--------------------------------------+------------------------------------------------+
|                        |                | History (internal dump interface)    | :ref:`PGXC_WLM_SESSION_INFO <dws_04_0839>`     |
+------------------------+----------------+--------------------------------------+------------------------------------------------+
| Operator               | Current CN     | History (Database Manager interface) | :ref:`GS_WLM_OPERATOR_HISTORY <dws_04_0702>`   |
+------------------------+----------------+--------------------------------------+------------------------------------------------+
|                        |                | History (internal dump interface)    | :ref:`GS_WLM_OPERAROR_INFO <dws_04_0701>`      |
+------------------------+----------------+--------------------------------------+------------------------------------------------+
|                        | All CNs        | History (Database Manager interface) | :ref:`PGXC_WLM_OPERATOR_HISTORY <dws_04_0836>` |
+------------------------+----------------+--------------------------------------+------------------------------------------------+
|                        |                | History (internal dump interface)    | :ref:`PGXC_WLM_OPERATOR_INFO <dws_04_0837>`    |
+------------------------+----------------+--------------------------------------+------------------------------------------------+

.. note::

   -  The view level is determined by the resource monitoring level, that is, the :ref:`resource_track_level <en-us_topic_0000001145694507__section153571329142612>` configuration.
   -  The perf and operator levels affect the values of the **query_plan** and **warning** columns in :ref:`GS_WLM_SESSION_STATISTICS <dws_04_0706>`/:ref:`PGXC_WLM_SESSION_INFO <dws_04_0839>`. For details, see :ref:`SQL Self-Diagnosis <dws_04_0446>`.
   -  Prefixes **gs** and **pgxc** indicate views showing single CN information and those showing cluster information, respectively. Common users can log in to a CN in the cluster to query only views with the **gs** prefix.
   -  You can query the **gs_wlm_session_info**, **gs_wlm_operator_info**, **pgxc_wlm_session_info**, and **pgxc_wlm_operator_info** views only after connecting to the **gaussdb** database.
   -  If instance fault occurs, some SQL statement information may fail to be recorded in historical resource monitoring views.
   -  In some abnormal cases, the status information column in the historical TopSQL may be displayed as **unknown**. The recorded monitoring information may be inaccurate.
   -  The SQL statements that can be recorded in historical resource monitoring views are the same as those recorded in real-time resource monitoring views. For details, see :ref:`SQL statements recorded in real-time resource monitoring views <en-us_topic_0000001098974816__li12942257154712>`.

Prerequisites
-------------

-  The GUC parameter enable_resource_track is set to **on**. The default value is **on**.
-  The GUC parameter :ref:`resource_track_level <en-us_topic_0000001145694507__section153571329142612>` is set to **query**, **perf**, or **operator**. The default value is **query**. For details, see :ref:`Table 2 <en-us_topic_0000001098974816__table874434715481>`.
-  The GUC parameter enable_resource_record is set to **on**. The default value is **off**.
-  The value of the :ref:`resource_track_duration <en-us_topic_0000001145694507__section347574425112>` parameter (**60s** by default) is less than the job execution time.
-  Job monitoring rules are as follows:

   -  Jobs whose execution time recorded in the real-time resource monitoring view (see :ref:`Table 1 <en-us_topic_0000001098974816__table16116143418462>`) is greater than or equal to :ref:`resource_track_duration <en-us_topic_0000001145694507__section347574425112>`.

-  If the Cgroups function is properly loaded, you can run the **gs_cgroup -P** command to view information about Cgroups.
-  This function will cause storage space expansion and slightly affect system performance. You are not advised to set it as a default system function.

Procedure
---------

#. Query the load records of the current CN after its latest job is complete in the **gs_wlm_session_history** view.

   ::

      SELECT * FROM gs_wlm_session_history;

#. Query the load records of all the CNs after their latest job are complete in the **pgxc_wlm_session_history** view.

   ::

       SELECT * FROM pgxc_wlm_session_history;

#. Query the load records of the current CN through the **gs_wlm_session_info** table after the task is complete. To query the historical records successfully, set enable_resource_record to **on**.

   ::

      SELECT * FROM gs_wlm_session_info;

   -  Top 10 queries that consume the most memory (You can specify a query period.)

   ::

      SELECT * FROM gs_wlm_session_info order by max_peak_memory desc limit 10;

   ::

      SELECT * FROM gs_wlm_session_info WHERE start_time >= '2022-05-15 21:00:00' and finish_time <='2022-05-15 23:30:00'order by max_peak_memory desc limit 10;

   -  Top 10 queries that consume the most CPU (You can specify a query period.)

   ::

      SELECT * FROM gs_wlm_session_info order by total_cpu_time desc limit 10;

   ::

      SELECT * FROM gs_wlm_session_info WHERE start_time >= '2022-05-15 21:00:00' and finish_time <='2022-05-15 23:30:00'order by total_cpu_time desc limit 10;

#. Query for the load records of all the CNs after their jobs are complete in the **pgxc_wlm_session_info** view. To query the historical records successfully, set enable_resource_record to **on**.

   ::

      SELECT * FROM pgxc_wlm_session_info;

   -  Query the top 10 queries that take up the most CN processing time (You can specify a query period.)

   ::

      SELECT * FROM pgxc_wlm_session_info order by duration desc limit 10;

   ::

      SELECT * FROM pgxc_wlm_session_info WHERE start_time >= '2022-05-15 21:00:00' and finish_time <='2022-05-15 23:30:00'order by nodename,max_peak_memory desc limit 10;

#. Use the **pgxc_get_wlm_session_info_bytime** function to filter and query the **pgxc_wlm_session_info** view. To query the historical records successfully, set enable_resource_record to **on**. You are advised to use this function if the view contains a large number of records.

   .. note::

      A GaussDB(DWS) cluster uses the UTC time by default, which has an 8-hour time difference with the system time. Before queries, ensure that the database time is the same as the system time.

   -  Return the queries started between **2019-09-10 15:30:00** and **2019-09-10 15:35:00** on all CNs. For each CN, a maximum of 10 queries will be returned.

   ::

      SELECT * FROM pgxc_get_wlm_session_info_bytime('start_time', '2019-09-10 15:30:00', '2019-09-10 15:35:00', 10);

   -  Return the queries ended between **2019-09-10 15:30:00** and **2019-09-10 15:35:00** on all CNs. For each CN, a maximum of 10 queries will be returned.

   ::

      SELECT * FROM pgxc_get_wlm_session_info_bytime('finish_time', '2019-09-10 15:30:00', '2019-09-10 15:35:00', 10);

#. Query the recent resource information of the job operators on the current CN in the **gs_wlm_operator_history** view. Ensure that :ref:`resource_track_level <en-us_topic_0000001145694507__section153571329142612>` is set to **operator**.

   ::

      SELECT * FROM gs_wlm_operator_history;

#. Query the recent resource information of the job operators on all the CNs in the **pgxc_wlm_operator_history** view. Ensure that :ref:`resource_track_level <en-us_topic_0000001145694507__section153571329142612>` is set to **operator**.

   ::

      SELECT * FROM pgxc_wlm_operator_history;

#. Query the recent resource information of the job operators on the current CN in the **gs_wlm_operator_info** view. Ensure that :ref:`resource_track_level <en-us_topic_0000001145694507__section153571329142612>` is set to **operator** and enable_resource_record to **on**.

   ::

      SELECT * FROM gs_wlm_operator_info;

#. Query for the historical resource information of job operators on all the CNs in the **pgxc_wlm_operator_info** view. Ensure that :ref:`resource_track_level <en-us_topic_0000001145694507__section153571329142612>` is set to **operator** and enable_resource_record to **on**.

   ::

      SELECT * FROM pgxc_wlm_operator_info;

.. note::

   -  The number of data records that can be retained in the memory is limited, because certain memory is reserved. After a real-time query is complete, its information is imported to historical views and will be deleted when the information has been stored in the hash table in the memory for 3 minutes. The upper limit of records varies by view. For a query-level view, whether a record can be stored depends on the upper limit of records allowed by the memory at that time point. On each CN, the maximum number of real-time query-level records that can be stored in memory (default: **5MB**) is **max_session_realt_num** (set to **12850** by default). The maximum number of historical records that can be stored in memory (default: **100MB**) is **max_session_hist_num** (set to **137970** by default). The average execution time of queries in a service system is greater than **run_time** (in the unit of s). The maximum number of concurrent jobs allowed by real-time views on each CN is: **num_realt_active** = **max_session_realt_num**. The maximum number of concurrent jobs allowed by historical views on each CN is: **num_hist_active** = **max_session_hist_num**/(**180**/**run_time**).
   -  For operator-level views, whether a record can be stored depends on the upper limit allowed by the memory at that time point. If the number of plan nodes plus the number of records in the memory exceeds the upper limit, the record cannot be stored. On each CN, the maximum numbers of real-time and historical operator-level records that can be stored in the memory are **max_oper_realt_num** (set to **56987** by default) and **max_oper_hist_num** (set to **113975** by default), respectively. The average number of plan nodes of a query is **num_plan_node**. Maximum number of concurrent tasks allowed by real-time views on each CN is: **num_realt_active** = **max_oper_realt_num**/**num_plan_node**. Maximum number of concurrent tasks allowed by historical views on each CN is: **num_hist_active** = **max_oper_hist_num**/(**180**/**run_time**)/**num_plan_node**.
   -  In high concurrency, ensure that the number of queries to be recorded does not exceed the maximum values set for query- and operator-level queries. You can modify the memory of the real-time query view by setting :ref:`session_statistics_memory <en-us_topic_0000001145694507__section4520191223820>` and the memory of the historical query view by setting **session_history_memory**. The memory size increases in direct proportion to the maximum number of queries that can be recorded.
