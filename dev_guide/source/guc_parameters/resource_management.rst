:original_name: dws_04_0922.html

.. _dws_04_0922:

Resource Management
===================

If database resource usage is not controlled, concurrent tasks easily preempt resources. As a result, the OS will be overloaded and cannot respond to user tasks; or even crash and cannot provide any services to users. The GaussDB(DWS) workload management function balances the database workload based on available resources to avoid database overloading.

.. _en-us_topic_0000001233563121__sc1692143c357427cbeadd6160010fd40:

use_workload_manager
--------------------

**Parameter description**: Specifies whether to enable the resource management function. This parameter must be applied on both CNs and DNs.

**Type**: SIGHUP

**Value range**: Boolean

-  **on** indicates the resource management function is enabled.
-  **off** indicates the resource management function is disabled.

   .. note::

      -  If method 2 in :ref:`Setting GUC Parameters <en-us_topic_0000001233563311__s8adb68393b48467a948956afaaaf8589>` is used to change the parameter value, the new value takes effect only for the threads that are started after the change. In addition, the new value does not take effect for new jobs that are executed by backend threads and reused threads. You can make the new value take effect for these threads by using **kill session** or restarting the node.

      -  After the value of **use_workload_manager** changes from **off** to **on**, the resource management view becomes available, and you can query the storage resource usage collected in the **off** state. If there are slight errors and the storage resource usage needs to be corrected, run the following command. If data is inserted into the table during the command execution, the statistics may be inaccurate.

         ::

            SELECT gs_wlm_readjust_user_space(0);

**Default value**: **on**

enable_perm_space
-----------------

**Parameter description**: Specifies whether to enable the perm space function. This parameter must be applied on both CNs and DNs.

**Type**: POSTMASTER

**Value range**: Boolean

-  **on** indicates the perm space function is enabled.
-  **off** indicates the perm space function is disabled.

**Default value**: **on**

space_once_adjust_num
---------------------

**Parameter description**: In the space control and space statistics functions, specifies the threshold of the number of files processed each time during slow building and fine-grained calibration. This parameter is supported by version 8.1.3 or later clusters.

**Type**: SIGHUP

**Value range**: an integer ranging from 1 to INT_MAX

-  The value **0** indicates that the slow build and fine-grained calibration functions are disabled.

**Default value**: **300**

.. note::

   The file quantity threshold affects database resources. You are advised to set the threshold to a proper value.

space_readjust_schedule
-----------------------

**Parameter description**: In the space control and space statistics functions, specifies the space error threshold for triggering automatic calibration. This parameter is supported by version 8.1.3 or later clusters.

**Type**: SIGHUP

**Value range**: string

-  **off** indicates that the automatic calibration function is disabled.
-  **auto** indicates that the automatic calibration function is enabled and the error threshold for triggering automatic calibration is **1 GB**.
-  **auto (space size + K/M/G)** indicates that the automatic calibration is enabled and the error threshold for triggering automatic calibration is *xxx* KB/MB/GB (user-defined). For example, **auto(200M)** indicates that the automatic calibration is enabled and the error threshold for triggering automatic calibration is **200 MB**.

**Default value**: **auto**

max_active_statements
---------------------

**Parameter description**: Specifies the maximum global concurrency. This parameter applies to one CN.

The database administrator changes the value of this parameter based on system resources (for example, CPU, I/O, and memory resources) so that the system fully supports the concurrency tasks and avoids too many concurrency tasks resulting in system crash.

**Type**: SIGHUP

**Value range**: an integer ranging from -1 to INT_MAX. The values **-1** and **0** indicate that the number of concurrent requests is not limited.

**Default value**: **60**

parctl_min_cost
---------------

**Parameter description**: Specifies the minimum estimated cost of a complex job under static resource management. Threshold for dividing simple jobs and complex jobs. A job whose estimated cost is less than the value of this parameter is a simple job, and a job whose estimated cost is larger than or equal to the value of this parameter is a complex job.

**Type**: SIGHUP

**Value range**: an integer ranging from -1 to INT_MAX

-  If **parctl_min_cost** is **-1**, all jobs are simple jobs.
-  Jobs whose estimated cost is less than 10 are simple jobs.

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

**Value range**: an integer ranging from 1 to INT_MAX. The unit is second.

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

.. _en-us_topic_0000001233563121__s9530ecdd2b0d4a98b67b66e32bf8e5d0:

enable_resource_track
---------------------

**Parameter description**: Specifies whether the real-time resource monitoring function is enabled. This parameter must be applied on both CNs and DNs.

**Type**: SIGHUP

**Value range**: Boolean

-  **on** indicates the resource monitoring function is enabled.
-  **off** indicates the resource monitoring function is disabled.

**Default value**: **on**

.. _en-us_topic_0000001233563121__s5f116e109a2944e3854abcc56772eaa1:

enable_resource_record
----------------------

**Parameter description**: Specifies whether resource monitoring records are archived. When this parameter is enabled, records that have been executed are archived to the corresponding **INFO** views (:ref:`GS_WLM_SESSION_INFO <dws_04_0704>` and :ref:`GS_WLM_OPERAROR_INFO <dws_04_0701>`). This parameter must be applied on both CNs and DNs.

**Type**: SIGHUP

**Value range**: Boolean

-  **on** indicates that the resource monitoring records are archived.
-  **off** indicates that the resource monitoring records are not archived.

**Default value**: **on**

.. note::

   The default value of this parameter is **on** for a new cluster. In upgrade scenarios, the default value of this parameter is the same as that of the source version.

.. _en-us_topic_0000001233563121__section7181949101319:

enable_track_record_subsql
--------------------------

**Parameter description**: Specifies whether to enable the function of recording and archiving sub-statements. When this function is enabled, sub-statements in stored procedures and anonymous blocks are recorded and archived to the corresponding **INFO** table (:ref:`GS_WLM_SESSION_INFO <dws_04_0566>`). This parameter is a session-level parameter. It can be configured and take effect in the session connected to the CN and affects only the statements in the session. It can also be configured on both the CN and DN and take effect globally.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that the sub-statement resource monitoring records are archived.
-  **off** indicates that the sub-statement resource monitoring records are not archived.

**Default value**: **off**

.. _en-us_topic_0000001233563121__section827402723813:

enable_user_metric_persistent
-----------------------------

**Parameter description**: Specifies whether the user historical resource monitoring dumping function is enabled. When this function is enabled, data in the :ref:`PG_TOTAL_USER_RESOURCE_INFO <dws_04_0790>` view is periodically sampled and saved to the :ref:`GS_WLM_USER_RESOURCE_HISTORY <dws_04_0567>` system catalog, and data in the :ref:`GS_RESPOOL_RESOURCE_INFO <dws_04_0977>` view is periodically sampled and saved to the :ref:`GS_RESPOOL_RESOURCE_HISTORY <dws_04_0975>` system catalog.

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

.. _en-us_topic_0000001233563121__section18113133910385:

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

**Parameter description**: Specifies the retention time of the instance historical resource monitoring data. This parameter is valid only when :ref:`enable_instance_metric_persistent <en-us_topic_0000001233563121__section18113133910385>` is set to **on**.

**Type**: SIGHUP

**Value range**: an integer ranging from 0 to 3650. The unit is day.

-  If this parameter is set to **0**, instance historical resource monitoring data is permanently stored.
-  If the value is greater than **0**, the instance historical resource monitoring data is stored for the specified number of days.

**Default value**: **7**

.. _en-us_topic_0000001233563121__section153571329142612:

resource_track_level
--------------------

**Parameter description**: Specifies the resource monitoring level of the current session. This parameter is valid only when :ref:`enable_resource_track <en-us_topic_0000001233563121__s9530ecdd2b0d4a98b67b66e32bf8e5d0>` is set to **on**.

**Type**: USERSET

**Value range**: enumerated values

-  **none**: Resources are not monitored.
-  **query**: Enables query-level resource monitoring. If this function is enabled, the plan information (similar to the output information of EXPLAIN) of SQL statements will be recorded in top SQL statements.
-  **perf**: Enables the perf-level resource monitoring. If this function is enabled, the plan information (similar to the output information of EXPLAIN ANALYZE) that contains the actual execution time and the number of execution rows will be recorded in the top SQL.
-  **operator**: enables the operator-level resource monitoring. If this function is enabled, not only the information including the actual execution time and number of execution rows is recorded in the top SQL statement, but also the operator-level execution information is updated to the top SQL statement.

**Default value**: **query**

.. _en-us_topic_0000001233563121__section1089022732713:

resource_track_cost
-------------------

**Parameter description**: Specifies the minimum execution cost for resource monitoring on statements in the current session. This parameter is valid only when :ref:`enable_resource_track <en-us_topic_0000001233563121__s9530ecdd2b0d4a98b67b66e32bf8e5d0>` is set to **on**.

**Type**: USERSET

**Value range**: an integer ranging from -1 to INT_MAX

-  **-1** indicates that resource monitoring is disabled.
-  A value greater than or equal to **0** indicates that statements whose execution cost exceeds this value will be monitored.

**Default value**: **0**

.. note::

   The default value of this parameter is **0** for a new cluster. In upgrade scenarios, the default value of this parameter is the same as that of the source version.

.. _en-us_topic_0000001233563121__section347574425112:

resource_track_duration
-----------------------

**Parameter description**: Specifies the minimum statement execution time that determines whether information about jobs of a statement recorded in the real-time view (see :ref:`Table 1 <en-us_topic_0000001233681601__table16116143418462>`) will be dumped to a historical view after the statement is executed. Job information will be dumped from the real-time view (with the suffix **statistics**) to a historical view (with the suffix **history**) if the statement execution time is no less than this value. This parameter is valid only when :ref:`enable_resource_track <en-us_topic_0000001233563121__s9530ecdd2b0d4a98b67b66e32bf8e5d0>` is set to **on**.

**Type**: USERSET

**Value range**: an integer ranging from 0 to INT_MAX. The unit is second (s).

-  **0** indicates that information about all statements recorded in the real-time resource monitoring view (see :ref:`Table 1 <en-us_topic_0000001233681601__table16116143418462>`) will be archived into historical views.
-  If the value is greater than **0**, information about statements recorded in the real-time resource monitoring view (see :ref:`Table 1 <en-us_topic_0000001233681601__table16116143418462>`), whose execution time exceeds this value will be archived into historical views.

**Default value**: **60s**

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

**Parameter description**: This parameter has been discarded in version 8.1.2 and is reserved for compatibility with earlier versions. This parameter is invalid in the current version.

**Type**: USERSET

**Value range**: an integer ranging from 0 to 1073741823

**Default value**: **0**

io_priority
-----------

**Parameter description**: This parameter has been discarded in version 8.1.2 and is reserved for compatibility with earlier versions. This parameter is invalid in the current version.

**Type**: USERSET

**Value range**: enumerated values

-  None
-  Low
-  Medium
-  High

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

.. _en-us_topic_0000001233563121__section27306369458:

session_history_memory
----------------------

**Parameter description**: Specifies the memory size of a historical query view.

**Type**: SIGHUP

**Value range**: an integer ranging from 10240 to 50% of **max_process_memory**. The unit is KB.

**Default value:** **100 MB**

topsql_retention_time
---------------------

**Parameter description**: Specifies the retention period of historical Top SQL data in the **gs_wlm_session_info** and **gs_wlm_operator_info** tables.

**Type**: SIGHUP

**Value range**: an integer ranging from 0 to 3650. The unit is day.

-  If it is set to **0**, the data is stored permanently.
-  If the value is greater than **0**, the data is stored for the specified number of days.

**Default value**: **30**

.. caution::

   -  Before setting this GUC parameter to enable the data retention function, delete data from the **gs_wlm_session_info** and **gs_wlm_operator_info** tables.
   -  The default value of this parameter is **30** for a new cluster. In upgrade scenarios, the default value of this parameter is the same as that of the source version.

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

.. _en-us_topic_0000001233563121__section3917839115:

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
