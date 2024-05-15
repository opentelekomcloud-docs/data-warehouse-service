:original_name: dws_04_0397.html

.. _dws_04_0397:

Real-time Top SQL
=================

You can query real-time Top SQL in real-time resource monitoring views at different levels. The real-time resource monitoring view records the resource usage (including memory, data flushed to disks, and CPU time) and performance alarm information during job running.

The following table describes the external interfaces of the real-time views.

.. _en-us_topic_0000001233681601__table16116143418462:

.. table:: **Table 1** Real-time resource monitoring views

   +------------------------+----------------+---------------------------------------------------+
   | Level                  | Monitored Node | View                                              |
   +========================+================+===================================================+
   | Query level/perf level | Current CN     | :ref:`GS_WLM_SESSION_STATISTICS <dws_04_0706>`    |
   +------------------------+----------------+---------------------------------------------------+
   |                        | All CNs        | :ref:`PGXC_WLM_SESSION_STATISTICS <dws_04_0841>`  |
   +------------------------+----------------+---------------------------------------------------+
   | Operator level         | Current CN     | :ref:`GS_WLM_OPERATOR_STATISTICS <dws_04_0703>`   |
   +------------------------+----------------+---------------------------------------------------+
   |                        | All CNs        | :ref:`PGXC_WLM_OPERATOR_STATISTICS <dws_04_0838>` |
   +------------------------+----------------+---------------------------------------------------+

.. note::

   -  The view level is determined by the resource monitoring level, that is, the :ref:`resource_track_level <en-us_topic_0000001233563121__section153571329142612>` configuration.

   -  The perf and operator levels affect the values of the **query_plan** and **warning** columns in :ref:`GS_WLM_SESSION_STATISTICS <dws_04_0706>`/:ref:`PGXC_WLM_SESSION_INFO <dws_04_0839>`. For details, see :ref:`SQL Self-Diagnosis <dws_04_0446>`.

   -  Prefixes **gs** and **pgxc** indicate views showing single CN information and those showing cluster information, respectively. Common users can log in to a CN in the cluster to query only views with the **gs** prefix.

   -  When you query this type of views, there will be network latency, because the views obtain resource usage in real time.

   -  If an instance fault occurs, some Top SQL statement information may fail to be recorded in real-time resource monitoring views.

   -  .. _en-us_topic_0000001233681601__li12942257154712:

      Top SQL statements are recorded in real-time resource monitoring views as follows:

      -  Special DDL statements, such as **SET**, **RESET**, **SHOW**, **ALTER SESSION SET**, and **SET CONSTRAINTS**, are not recorded.
      -  DDL statements, such as **CREATE**, **ALTER**, **DROP**, **GRANT**, **REVOKE**, and **VACUUM**, are recorded.
      -  DML statements are recorded, including:

         -  the execution of **SELECT**, **INSERT**, **UPDATE**, and **DELETE**
         -  the execution of **EXPLAIN ANALYZE** and **EXPLAIN PERFORMANCE**
         -  the use of the query-level or perf-level views

      -  The entry statements for invoking functions and stored procedures are recorded. When the GUC parameter :ref:`enable_track_record_subsql <en-us_topic_0000001233563121__section7181949101319>` is enabled, some internal statements (except the **DECLARE** definition statement) of a stored procedure can be recorded. Only the internal statements delivered to DNs for execution are recorded, and the remaining internal statements are filtered out.
      -  The anonymous block statement is recorded. When the GUC parameter :ref:`enable_track_record_subsql <en-us_topic_0000001233563121__section7181949101319>` is enabled, some internal statements of an anonymous block can be recorded. Only the internal statements delivered to DNs for execution are recorded, and the remaining internal statements are filtered out.
      -  The cursor statements are recorded. If a cursor does not read data from the cache but triggers the condition for delivering the statement to a DN for execution, the cursor statement is recorded and the statement and execution plan are enhanced. However, if the cursor reads data from the cache, the cursor statement is not recorded. When a cursor statement is used in an anonymous block or function and the cursor reads a large amount of data from a DN but is not fully used, the monitoring information about the cursor on the DN cannot be recorded due to the current architecture limitation. The **With Hold** cursor syntax has a special execution logic. It executes queries during transaction committing. If a statement execution error is reported during this period of time, the **aborted** status of the job cannot be recorded in the TopSQL history table.
      -  Statistics are not collected for jobs in the redistribution process.
      -  The parameters of a statement with placeholders executed by JDBC are generally specified. However, if the length of the parameter and the original statement exceeds 64 KB, the parameter is not recorded. If the statement is a lightweight statement, it is directly delivered to the DN for execution and the parameter is not recorded.
      -  Scheduled task statements are not recorded. This function is supported only in versions later than 8.2.1.

Prerequisites
-------------

-  The GUC parameter :ref:`enable_resource_track <en-us_topic_0000001233563121__s9530ecdd2b0d4a98b67b66e32bf8e5d0>` is set to **on**. The default value is **on**.
-  The GUC parameter :ref:`resource_track_level <en-us_topic_0000001233563121__section153571329142612>` is set to **query**, **perf**, or **operator**. The default value is **query**.
-  Job monitoring rules are as follows:

   -  Jobs whose execution cost estimated by the optimizer is greater than or equal to :ref:`resource_track_cost <en-us_topic_0000001233563121__section1089022732713>`.

-  If the Cgroups function is properly loaded, you can run the **gs_cgroup -P** command to view information about Cgroups.
-  The GUC parameter :ref:`enable_track_record_subsql <en-us_topic_0000001233563121__section7181949101319>` specifies whether to record internal statements of a stored procedure or anonymous block.

In the preceding prerequisites, :ref:`enable_resource_track <en-us_topic_0000001233563121__s9530ecdd2b0d4a98b67b66e32bf8e5d0>` is a system-level parameter that specifies whether to enable resource monitoring. :ref:`resource_track_level <en-us_topic_0000001233563121__section153571329142612>` is a session-level parameter. You can set the resource monitoring level of a session as needed. The following table describes the values of the two parameters.

.. _en-us_topic_0000001233681601__table874434715481:

.. table:: **Table 2** Setting the resource monitoring level to collect statistics

   +-----------------------+----------------------+-------------------------+----------------------------+
   | enable_resource_track | resource_track_level | Query-Level Information | Operator-Level Information |
   +=======================+======================+=========================+============================+
   | on(default)           | none                 | Not collected           | Not collected              |
   +-----------------------+----------------------+-------------------------+----------------------------+
   |                       | query(default)       | Collected               | Not collected              |
   +-----------------------+----------------------+-------------------------+----------------------------+
   |                       | perf                 | Collected               | Not collected              |
   +-----------------------+----------------------+-------------------------+----------------------------+
   |                       | operator             | Collected               | Collected                  |
   +-----------------------+----------------------+-------------------------+----------------------------+
   | off                   | none/query/operator  | Not collected           | Not collected              |
   +-----------------------+----------------------+-------------------------+----------------------------+

Procedure
---------

#. Query for the real-time CPU information in the **gs_session_cpu_statistics** view.

   ::

      SELECT * FROM gs_session_cpu_statistics;

#. Query for the real-time memory information in the **gs_session_memory_statistics** view.

   ::

      SELECT * FROM gs_session_memory_statistics;

#. Query for the real-time resource information about the current CN in the **gs_wlm_session_statistics** view.

   ::

      SELECT * FROM gs_wlm_session_statistics;

#. Query for the real-time resource information about all CNs in the **pgxc_wlm_session_statistics** view.

   ::

      SELECT * FROM pgxc_wlm_session_statistics;

#. Query for the real-time resource information about job operators on the current CN in the **gs_wlm_operator_statistics** view.

   ::

      SELECT * FROM gs_wlm_operator_statistics;

#. Query for the real-time resource information about job operators on all CNs in the **pgxc_wlm_operator_statistics** view.

   ::

      SELECT * FROM pgxc_wlm_operator_statistics;

#. Query for the load management information about the jobs executed by the current user in the **PG_SESSION_WLMSTAT** view.

   ::

      SELECT * FROM pg_session_wlmstat;

#. Query the job execution status of the current user on each CN in the **pgxc_wlm_workload_records** view (this view is available when the dynamic load function is enabled, that is, **enable_dynamic_workload** is set to **on**).

   ::

      SELECT * FROM pgxc_wlm_workload_records;
