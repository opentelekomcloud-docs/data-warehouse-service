:original_name: dws_04_0922.html

.. _dws_04_0922:

Workload Management
===================

If database resource usage is not controlled, concurrent tasks easily preempt resources. As a result, the OS will be overloaded and cannot respond to user tasks; or even crash and cannot provide any services to users. The GaussDB(DWS) workload management function balances the database workload based on available resources to avoid database overloading.

.. _en-us_topic_0000001145694507__sc1692143c357427cbeadd6160010fd40:

use_workload_manager
--------------------

**Parameter description**: Specifies whether to enable the resource management function. This parameter must be applied on both CNs and DNs.

**Type**: SIGHUP

**Value range**: Boolean

-  **on** indicates the resource management function is enabled.
-  **off** indicates the resource management function is disabled.

   .. note::

      -  If a method in :ref:`Setting GUC Parameters <en-us_topic_0000001145814675__s8adb68393b48467a948956afaaaf8589>` is used to change the parameter value, the new value takes effect only for the threads that are started after the change. In addition, the new value does not take effect for new jobs that are executed by backend threads and reused threads. You can make the new value take effect for these threads by using **kill session** or restarting the node.

      -  After the value of **use_workload_manager** changes from **off** to **on**, the resource management view becomes available, and you can query the storage resource usage collected in the **off** state. If there are slight errors and the storage resource usage needs to be corrected, run the following command. If data is inserted into the table during the command execution, the statistics may be inaccurate.

         ::

            select gs_wlm_readjust_user_space(0);

**Default value**: **on**

enable_control_group
--------------------

**Parameter description**: Specifies whether to enable the Cgroup management function. This parameter must be applied on both CNs and DNs.

**Type**: SIGHUP

**Value range**: Boolean

-  **on** indicates the Cgroup management function is enabled.
-  **off** indicates the Cgroup management function is disabled.

**Default value**: on

.. note::

   If a method in :ref:`Setting GUC Parameters <en-us_topic_0000001145814675__s8adb68393b48467a948956afaaaf8589>` is used to change the parameter value, the new value takes effect only for the threads that are started after the change. In addition, the new value does not take effect for new jobs that are executed by backend threads and reused threads. You can make the new value take effect for these threads by using **kill session** or restarting the node.

enable_backend_control
----------------------

**Parameter description**: Specifies whether to control the database permanent thread to the **DefaultBackend** Cgroup. This parameter must be applied on both CNs and DNs.

**Type**: POSTMASTER

**Value range**: Boolean

-  **on**: Controls the permanent thread to the **DefaultBackend** Cgroup.
-  **off**: Does not control the permanent thread to the **DefaultBackend** Cgroup.

**Default value**: **on**

enable_vacuum_control
---------------------

**Parameter description**: Specifies whether to control the database permanent thread autoVacuumWorker to the **Vacuum** Cgroup. This parameter must be applied on both CNs and DNs.

**Type**: POSTMASTER

**Value range**: Boolean

-  **on**: Controls the database permanent thread autoVacuumWorker to the **Vacuum** Cgroup.
-  **off**: Does not control the database permanent thread autoVacuumWorker to the **Vacuum** Cgroup.

**Default value**: **on**

enable_perm_space
-----------------

**Parameter description**: Specifies whether to enable the perm space function. This parameter must be applied on both CNs and DNs.

**Type**: POSTMASTER

**Value range**: Boolean

-  **on** indicates the perm space function is enabled.
-  **off** indicates the perm space function is disabled.

**Default value**: **on**

enable_verify_active_statements
-------------------------------

**Parameter description**: Specifies whether to enable the background calibration function in static adaptive load scenarios. This parameter must be used on CNs.

**Type**: SIGHUP

**Value range**: Boolean

-  **on** indicates the background calibration function is enabled.
-  **off** indicates the background calibration function is disabled.

**Default value**: **on**

max_active_statements
---------------------

**Parameter description**: Specifies the maximum global concurrency. This parameter applies to one CN.

The database administrator changes the value of this parameter based on system resources (for example, CPU, I/O, and memory resources) so that the system fully supports the concurrency tasks and avoids too many concurrency tasks resulting in system crash.

**Type**: SIGHUP

**Value range**: an integer ranging from -1 to INT_MAX. The values **-1** and **0** indicate that the number of concurrent requests is not limited.

**Default value**: **60**

parctl_min_cost
---------------

**Parameter description**: Specifies the minimum execution cost of a statement under the concurrency control of a resource pool.

**Type**: SIGHUP

**Value range**: an integer ranging from -1 to INT_MAX

-  If the value is **-1** or the cost of executing statements is less than 10, the job is controlled by the fast lane.
-  If the value is greater than or equal to **0**, and the cost of executing statements exceeds the value and is greater than or equal to 10, the statements are subject to the concurrent limit of a resource pool.

**Default value**: **100000**

cgroup_name
-----------

**Parameter description**: Specifies the name of the Cgroup in use. It can be used to change the priorities of jobs in the queue of a Cgroup.

If you set **cgroup_name** and then **session_respool**, the Cgroups associated with **session_respool** take effect. If you reverse the order, Cgroups associated with **cgroup_name** take effect.

If the Workload Cgroup level is specified during the **cgroup_name** change, the database does not check the Cgroup level. The level ranges from 1 to 10.

**Type**: USERSET

You are not advised to set **cgroup_name** and **session_respool** at the same time.

**Value range**: a string

**Default value**: **DefaultClass:Medium**

.. note::

   **DefaultClass:Medium** indicates the **Medium** Cgroup belonging to the **Timeshare** Cgroup under the **DefaultClass** Cgroup.

cpu_collect_timer
-----------------

**Parameter description**: Specifies how frequently CPU data is collected during statement execution on DNs.

The database administrator changes the value of this parameter based on system resources (for example, CPU, I/O, and memory resources) so that the system fully supports the concurrency tasks and avoids too many concurrency tasks resulting in system crash.

**Type**: SIGHUP

**Value range**: an integer ranging from -1 to INT_MAX. The unit is second.

**Default value**: **30**

enable_cgroup_switch
--------------------

**Parameter description**: Specifies whether the database automatically switches to the **TopWD** group when executing statements by group type.

**Type**: USERSET

**Value range**: Boolean

-  **on**: The database automatically switches to the **TopWD** group when executing statements by group type.
-  **off**: The database does not automatically switch to the **TopWD** group when executing statements by group type.

**Default value**: **off**

memory_tracking_mode
--------------------

**Parameter description**: Specifies the memory information recording mode.

**Type**: USERSET

**Value range**:

-  **none**: Memory statistics is not collected.
-  **normal:** Only memory statistics is collected in real time and no file is generated.
-  **executor:** The statistics file is generated, containing the context information about all allocated memory used by the execution layer.
-  **fullexec**: The generated file includes the information about all memory contexts requested by the execution layer.

**Default value**: **none**

memory_detail_tracking
----------------------

**Parameter description**: Specifies the sequence number of the memory background information distributed in the needed thread and **plannodeid** of the query where the current thread is located.

**Type**: USERSET

**Value range**: a string

**Default value**: empty

.. important::

   It is recommended that you retain the default value for this parameter.

.. _en-us_topic_0000001145694507__s9530ecdd2b0d4a98b67b66e32bf8e5d0:

enable_resource_track
---------------------

**Parameter description**: Specifies whether the real-time resource monitoring function is enabled. This parameter must be applied on both CNs and DNs.

**Type**: SIGHUP

**Value range**: Boolean

-  **on** indicates the resource monitoring function is enabled.
-  **off** indicates the resource monitoring function is disabled.

**Default value**: **on**

enable_resource_record
----------------------

**Parameter description**: Specifies whether resource monitoring records are archived. If this parameter is set to **on**, records in the **history** views (**GS_WLM_SESSION_HISTORY** and **GS_WLM_OPERATOR_HISTORY**) are archived to the corresponding **info** views (**GS_WLM_SESSION_INFO** and **GS_WLM_OPERATOR_INFO**) at an interval of 3 minutes. After being archived, the records are deleted from the **history** views. This parameter must be applied on both CNs and DNs.

**Type**: SIGHUP

**Value range**: Boolean

-  **on** indicates that the resource monitoring records are archived.
-  **off** indicates that the resource monitoring records are not archived.

**Default value**: **off**

enable_user_metric_persistent
-----------------------------

**Parameter description**: Specifies whether the user historical resource monitoring dumping function is enabled. If this function is enabled, data in view :ref:`PG_TOTAL_USER_RESOURCE_INFO <dws_04_0790>` is periodically sampled and saved to system catalog :ref:`GS_WLM_USER_RESOURCE_HISTORY <dws_04_0567>`.

**Type**: SIGHUP

**Value range**: Boolean

-  **on** indicates that the user historical resource monitoring dumping function is enabled.
-  **off** indicates that the user historical resource monitoring dumping function is disabled.

**Default value**: **on**

user_metric_retention_time
--------------------------

**Parameter description**: Specifies the retention time of the user historical resource monitoring data. This parameter is valid only when **enable_user_metric_persistent** is set to **on**.

**Type**: SIGHUP

**Value range**: an integer ranging from 0 to 3650. The unit is day.

-  If this parameter is set to **0**, user historical resource monitoring data is permanently stored.
-  If the value is greater than **0**, user historical resource monitoring data is stored for the specified number of days.

**Default value**: **7**

enable_instance_metric_persistent
---------------------------------

**Parameter description**: Specifies whether the instance resource monitoring dumping function is enabled. When this function is enabled, the instance monitoring data is saved to the system catalog :ref:`GS_WLM_INSTANCE_HISTORY <dws_04_0564>`.

**Type**: SIGHUP

**Value range**: Boolean

-  **on** indicates that the instance resource monitoring dumping function is enabled.
-  **off**: Specifies that the instance resource monitoring dumping function is disabled.

**Default value**: **on**

instance_metric_retention_time
------------------------------

**Parameter description**: Specifies the retention time of the instance historical resource monitoring data. This parameter is valid only when **enable_instance_metric_persistent** is set to **on**.

**Type**: SIGHUP

**Value range**: an integer ranging from 0 to 3650. The unit is day.

-  If this parameter is set to **0**, instance historical resource monitoring data is permanently stored.
-  If the value is greater than **0**, the instance historical resource monitoring data is stored for the specified number of days.

**Default value**: **7**

.. _en-us_topic_0000001145694507__section153571329142612:

resource_track_level
--------------------

**Parameter description**: Specifies the resource monitoring level of the current session. This parameter is valid only when **enable_resource_track** is set to **on**.

**Type**: USERSET

**Value range**: enumerated values

-  **none**: Resources are not monitored.
-  **query**: Enables query-level resource monitoring. If this function is enabled, the plan information (similar to the output information of EXPLAIN) of SQL statements will be recorded in top SQL statements.
-  **perf**: Enables the perf-level resource monitoring. If this function is enabled, the plan information (similar to the output information of EXPLAIN ANALYZE) that contains the actual execution time and the number of execution rows will be recorded in the top SQL.
-  **operator**: enables the operator-level resource monitoring. If this function is enabled, not only the information including the actual execution time and number of execution rows is recorded in the top SQL statement, but also the operator-level execution information is updated to the top SQL statement.

**Default value**: **query**

.. _en-us_topic_0000001145694507__section1089022732713:

resource_track_cost
-------------------

**Parameter description**: Specifies the minimum execution cost for resource monitoring on statements in the current session. This parameter is valid only when **enable_resource_track** is set to **on**.

**Type**: USERSET

**Value range**: an integer ranging from -1 to INT_MAX

-  **-1** indicates that resource monitoring is disabled.
-  A value greater than or equal to **0** indicates that statements whose execution cost exceeds this value will be monitored.

**Default value**: **100000**

.. _en-us_topic_0000001145694507__section347574425112:

resource_track_duration
-----------------------

**Parameter description**: Specifies the minimum statement execution time that determines whether information about jobs of a statement recorded in the real-time view (see :ref:`Table 1 <en-us_topic_0000001098974816__table16116143418462>`) will be dumped to a historical view after the statement is executed. Job information will be dumped from the real-time view (with the suffix **statistics**) to a historical view (with the suffix **history**) if the statement execution time is no less than this value.

**Type**: USERSET

**Value range**: an integer ranging from 0 to INT_MAX. The unit is second (s).

-  **0** indicates that information about all statements recorded in the real-time resource monitoring view (see :ref:`Table 1 <en-us_topic_0000001098974816__table16116143418462>`) will be archived into historical views.
-  If the value is greater than **0**, information about statements recorded in the real-time resource monitoring view (see :ref:`Table 1 <en-us_topic_0000001098974816__table16116143418462>`), whose execution time exceeds this value will be archived into historical views.

**Default value**: **1min**

dynamic_memory_quota
--------------------

**Parameter description**: Specifies the memory quota in adaptive load scenarios, that is, the proportion of maximum available memory to total system memory.

**Type**: SIGHUP

**Value range**: an integer ranging from 1 to 100

**Default value**: **80**

disable_memory_protect
----------------------

**Parameter description:** Stops memory protection. To query system views when system memory is insufficient, set this parameter to **on** to stop memory protection. This parameter is used only to diagnose and debug the system when system memory is insufficient. Set it to **off** in other scenarios.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that memory protection stops.
-  **off** indicates that memory is protected.

**Default value**: **off**

query_band
----------

**Parameter description**: Specifies the job type of the current session.

**Type**: USERSET

**Value range**: a string

**Default value**: empty

enable_bbox_dump
----------------

**Parameter description**: Specifies whether the black box function is enabled. The core files can be generated even through the core dump mechanism is not configured in the system. This parameter must be simultaneously used on CNs and DNs.

**Type**: SIGHUP

**Value range**: Boolean

-  **on** indicates that the black box function is enabled.
-  **off** indicates that the black box function is disabled.

**Default value**: **off**

enable_dynamic_workload
-----------------------

**Parameter description**: Specifies whether to enable the dynamic workload management function.

**Type**: POSTMASTER

**Value range**: Boolean

-  **on** indicates the dynamic workload management function is enabled.
-  **off** indicates the dynamic workload management function is disabled.

**Default value:** **on**

.. important::

   -  If memory adaptation is enabled, you do not need to use **work_mem** to optimize the operator memory usage after collecting statistics. The system will generate a plan for each statement based on the current load, estimating the memory used by each operator and by the entire statement. In a concurrency scenario, statements are queued based on the system load and their memory usage.
   -  The optimizer cannot accurately estimate the number of rows and will probably underestimate or overestimate memory usage. If the memory usage is underestimated, the allocated memory will be automatically increased during statement running. If the memory usage is overestimated, system resources will not be fully used, and the number of statements waiting in a queue will increase, which probably results in low performance. To improve performance, identify the statements whose estimated memory usage is much greater than the DN peak memory and adjust the value of **query_max_mem**. For details, see :ref:`Adjusting Key Parameters During SQL Tuning <dws_04_0453>`.

bbox_dump_count
---------------

**Parameter description**: Specifies the maximum number of core files that are generated by GaussDB(DWS) and can be stored in the path specified by **bbox_dump_path**. If the number of core files exceeds this value, old core files will be deleted. This parameter is valid only if **enable_bbox_dump** is set to **on**.

**Type**: USERSET

**Value range**: an integer ranging from 1 to 20

**Default value**: **8**

.. note::

   When core files are generated during concurrent SQL statement execution, the number of files may be larger than the value of **bbox_dump_count**.

io_limits
---------

**Parameter description**: Specifies the upper limit of IOPS triggered.

**Type**: USERSET

**Value range**: an integer ranging from 0 to 1073741823

**Default value**: **0**

io_priority
-----------

**Parameter description**: Specifies the I/O priority for jobs that consume many I/O resources. It takes effect when the I/O usage reaches 90%.

**Type**: USERSET

**Value range**: enumerated values

-  **None** indicates no control.
-  **Low** indicates that the IOPS is reduced to 20% of the original value.
-  **Medium** indicates that the IOPS is reduced to 50% of the original value.
-  **High** indicates that the IOPS is reduced to 80% of the original value.

**Default value**: **None**

session_respool
---------------

**Parameter description**: Specifies the resource pool associated with the current session.

**Type**: USERSET

If you set **cgroup_name** and then **session_respool**, the Cgroups associated with **session_respool** take effect. If you reverse the order, Cgroups associated with **cgroup_name** take effect.

If the Workload Cgroup level is specified during the **cgroup_name** change, the database does not check the Cgroup level. The level ranges from 1 to 10.

You are not advised to set **cgroup_name** and **session_respool** at the same time.

**Value range**: a string. This parameter can be set to the resource pool configured through **create resource pool**.

**Default value**: **invalid_pool**

enable_transaction_parctl
-------------------------

**Parameter description**: whether to control transaction block statements and stored procedure statements.

**Type**: USERSET

**Value range**: Boolean

-  **on**: Transaction block statements and stored procedure statements are controlled.
-  **off**: Transaction block statements and stored procedure statements are not controlled.

**Default value**: **on**

.. _en-us_topic_0000001145694507__section4520191223820:

session_statistics_memory
-------------------------

**Parameter description**: Specifies the memory size of a real-time query view.

**Type**: SIGHUP

**Value range**: an integer ranging from 5 MB to 50% of **max_process_memory**

**Default value**: **5 MB**

session_history_memory
----------------------

**Parameter description**: Specifies the memory size of a historical query view.

**Type**: SIGHUP

**Value range**: an integer ranging from 10 MB to 50% of **max_process_memory**

**Default value:** **100 MB**

topsql_retention_time
---------------------

**Parameter description**: Specifies the retention period of historical TopSQL data in the **gs_wlm_session_info** and **gs_wlm_operator_info** tables.

**Type**: SIGHUP

**Value range**: an integer ranging from 0 to 3650. The unit is day.

-  If it is set to **0**, the data is stored permanently.
-  If the value is greater than **0**, the data is stored for the specified number of days.

**Default value**: **0**

.. caution::

   Before setting this GUC parameter to enable the data retention function, delete data from the **gs_wlm_session_info** and **gs_wlm_operator_info** tables.

transaction_pending_time
------------------------

**Parameter description**: maximum queuing time of transaction block statements and stored procedure statements if **enable_transaction_parctl** is set to **on**.

**Type**: USERSET

**Value range**: an integer ranging from -1 to INT_MAX. The unit is second (s).

-  **-1** or **0**: No queuing timeout is specified for transaction block statements and stored procedure statements. The statements can be executed when resources are available.
-  Value greater than **0**: If transaction block statements and stored procedure statements have been queued for a time longer than the specified value, they are forcibly executed regardless of the current resource situation.

**Default value**: **0**

.. important::

   This parameter is valid only for internal statements of stored procedures and transaction blocks. That is, this parameter takes effect only for the statements whose **enqueue** value (for details, see :ref:`PG_SESSION_WLMSTAT <dws_04_0749>`) is **Transaction** or **StoredProc**.

wlm_sql_allow_list
------------------

**Parameter description**: Specifies whitelisted SQL statements for resource management. Whitelisted SQL statements are not monitored by resource management.

**Type**: SIGHUP

**Value range**: a string

**Default value**: empty

.. important::

   -  One or more whitelisted SQL statements can be specified in **wlm_sql_allow_list**. If multiple SQL statements are to be whitelisted, use semicolons (;) to separate them.
   -  The system determines whether SQL statements are monitored based on the prefix match. The SQL statements are case insensitive. For example, if **wlm_sql_allow_list** is set to **'SELECT'**, all **SELECT** statements are not monitored by the resource management module.
   -  The system identifies spaces at the beginning of the parameter value. For example, **'SELECT'** and **' SELECT'** have different representations. **' SELECT'** filters only the **SELECT** statements with spaces at the beginning.
   -  The system has some whitelisted SQL statements by default, which cannot be modified. You can query the default whitelisted SQL statements and the SQL statements that have been successfully added to the whitelist by GUC through the system view **gs_wlm_sql_allow**.
   -  New SQL statements cannot be appended to the whitelisted SQL statements specified by **wlm_sql_allow_list** but can be set only through overwriting. To add an SQL statement, query the original GUC value, add the new statement to the end of the original value, separate the statements with a semicolon (;), and set the GUC value again.
