:original_name: dws_04_0397.html

.. _dws_04_0397:

Real-time TopSQL
================

You can query real-time Top SQL in real-time resource monitoring views at different levels. The real-time resource monitoring view records the resource usage (including memory, disk, CPU time, and I/O) and performance alarm information during job running.

The following table describes the external interfaces of the real-time views.

.. _en-us_topic_0000001098974816__table16116143418462:

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

   -  The view level is determined by the resource monitoring level, that is, the :ref:`resource_track_level <en-us_topic_0000001145694507__section153571329142612>` configuration.

   -  The perf and operator levels affect the values of the **query_plan** and **warning** columns in :ref:`GS_WLM_SESSION_STATISTICS <dws_04_0706>`/:ref:`PGXC_WLM_SESSION_INFO <dws_04_0839>`. For details, see :ref:`SQL Self-Diagnosis <dws_04_0446>`.

   -  Prefixes **gs** and **pgxc** indicate views showing single CN information and those showing cluster information, respectively. Common users can log in to a CN in the cluster to query only views with the **gs** prefix.

   -  When you query this type of views, there will be network latency, because the views obtain resource usage in real time.

   -  If instance fault occurs, some SQL statement information may fail to be recorded in real-time resource monitoring views.

   -  .. _en-us_topic_0000001098974816__li12942257154712:

      SQL statements are recorded in real-time resource monitoring views as follows:

      -  DDL statements are not recorded, such as the execution of **CREATE**, **ALTER**, **DROP**, **GRANT**, **REVOKE**, and **VACUUM** statements.
      -  DML statements are recorded, including:

         -  the execution of **SELECT**, **INSERT**, **UPDATE**, and **DELETE**
         -  the execution of **EXPLAIN ANALYZE** and **EXPLAIN PERFORMANCE**
         -  the use of a query-level/perf-level view, which also supports the **CREATE TABLE AS** syntax.

      -  Statements in functions and stored procedures and statements used for calling functions and stored procedures are recorded. Statements in loop bodies (if any) of functions and stored procedures are not recorded.
      -  Statements in anonymous blocks are not recorded.
      -  Statements in transaction blocks are recorded. Statements in loop bodies (if any) of transaction blocks are not recorded.
      -  Cursor statements are recorded.
      -  Jobs in a redistribution process are not monitored.

Prerequisites
-------------

-  The GUC parameter enable_resource_track is set to **on**. The default value is **on**.
-  The GUC parameter :ref:`resource_track_level <en-us_topic_0000001145694507__section153571329142612>` is set to **query**, **perf**, or **operator**. The default value is **query**.
-  Job monitoring rules are as follows:

   -  Jobs whose execution cost estimated by the optimizer is greater than or equal to :ref:`resource_track_cost <en-us_topic_0000001145694507__section1089022732713>`.
   -  Long query jobs

-  If the Cgroups function is properly loaded, you can run the **gs_cgroup -P** command to view information about Cgroups.

In the preceding prerequisites, enable_resource_track is a system-level parameter that specifies whether to enable resource monitoring. :ref:`resource_track_level <en-us_topic_0000001145694507__section153571329142612>` is a session-level parameter. You can set the resource monitoring level of a session as needed. The following table describes the values of the two parameters.

.. _en-us_topic_0000001098974816__table874434715481:

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
