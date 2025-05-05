:original_name: dws_04_0922.html

.. _dws_04_0922:

Resource Management
===================

If database resource usage is not controlled, concurrent tasks easily preempt resources. As a result, the OS will be overloaded and cannot respond to user tasks; or even crash and cannot provide any services to users. The GaussDB(DWS) workload management function balances the database workload based on available resources to avoid database overloading.

space_once_adjust_num
---------------------

**Parameter description**: In the space control and space statistics functions, specifies the threshold of the number of files processed each time during slow building and fine-grained calibration. This parameter is supported only by clusters of version 8.1.3 or later.

**Type**: SIGHUP

**Value range**: an integer ranging from 0 to INT_MAX

-  The value **0** indicates that the slow build and fine-grained calibration functions are disabled.

**Default value**: **300**

.. note::

   The file quantity threshold affects database resources. You are advised to set the threshold to a proper value.

default_partition_cache_strategy
--------------------------------

**Parameter description**: Specifies the default policy for controlling partition caching. This parameter is supported only by clusters of version 8.3.0 or later.

**Type**: USERSET

**Value range**: enumerated values

-  **cache_each_partition_as_possible** enables maximum data caching. Data may not be written to CUs when being inserted into different partitions.
-  **flush_when_switch_partition** indicates that data is written to CUs if the data belongs to different partitions during insertion.

**Default value**: **cache_each_partition_as_possible**

max_active_statements
---------------------

**Parameter description**: Specifies the maximum global concurrency. This parameter applies to a job on a CN.

The database administrator changes the value of this parameter based on system resources (for example, CPU, I/O, and memory resources) so that the system fully supports the concurrency tasks and avoids too many concurrency tasks resulting in system crash.

**Type**: SIGHUP

**Value range**: an integer ranging from -1 to INT_MAX. The values **-1** and **0** indicate that the number of concurrent requests is not limited.

**Default value**: **60**

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

.. _en-us_topic_0000001811490709__s9530ecdd2b0d4a98b67b66e32bf8e5d0:

enable_resource_track
---------------------

**Parameter description**: Specifies whether the real-time resource monitoring function is enabled. This parameter must be applied on both CNs and DNs.

**Type**: SIGHUP

**Value range**: Boolean

-  **on** indicates the resource monitoring function is enabled.
-  **off** indicates the resource monitoring function is disabled.

**Default value**: **on**

.. _en-us_topic_0000001811490709__s5f116e109a2944e3854abcc56772eaa1:

enable_resource_record
----------------------

**Parameter description**: Specifies whether resource monitoring records are archived. When this parameter is enabled, records that have been executed are archived to the corresponding **INFO** views (:ref:`GS_WLM_SESSION_INFO <dws_04_0704>` and :ref:`GS_WLM_OPERATOR_INFO <dws_04_0701>`). This parameter must be applied on both CNs and DNs.

**Type**: SIGHUP

**Value range**: Boolean

-  **on** indicates that the resource monitoring records are archived.
-  **off** indicates that the resource monitoring records are not archived.

**Default value**: **on**

.. note::

   The default value of this parameter is **on** for a new cluster. In upgrade scenarios, the default value of this parameter is the same as that of the source version.

.. _en-us_topic_0000001811490709__section7181949101319:

enable_track_record_subsql
--------------------------

**Parameter description**: Specifies whether to enable the function of recording and archiving sub-statements. When this function is enabled, sub-statements in stored procedures and anonymous blocks are recorded and archived to the corresponding **INFO** table (:ref:`GS_WLM_SESSION_INFO <dws_04_0566>`). This parameter is a session-level parameter. It can be configured and take effect in the session connected to the CN and affects only the statements in the session. It can also be configured on both the CN and DN and take effect globally.

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that the sub-statement resource monitoring records are archived.
-  **off** indicates that the sub-statement resource monitoring records are not archived.

**Default value**: **on**

block_rule_cost
---------------

**Parameter description**: Specifies the minimum cost to trigger a query filter. This is supported only by clusters of version 9.1.0.200 or later.

**Type**: USERSET

**Value range**: an integer ranging from -1 to INT_MAX

-  Value **-1** indicates that the system filters all statements without considering the cost.
-  Value **0** indicates that all statements whose cost is greater than 0 are intercepted, but special statements (whose cost is 0) are not intercepted.
-  If the value is greater than **0** and the statement cost is less than the value of **block_rule_cost**, the statement is not intercepted; otherwise, it is intercepted.

**Default value**: **-1**

.. _en-us_topic_0000001811490709__section827402723813:

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

**Parameter description**: Specifies the retention time of the user historical resource monitoring data.

**Type**: SIGHUP

**Value range**: an integer ranging from 0 to 3650. The unit is day.

-  If this parameter is set to **0**, user historical resource monitoring data is permanently stored.
-  If the value is greater than **0**, user historical resource monitoring data is stored for the specified number of days.

**Default value**: **7**

.. _en-us_topic_0000001811490709__section153571329142612:

resource_track_level
--------------------

**Parameter description**: Specifies the resource monitoring level of the current session. This parameter is valid only when **enable_resource_track** is set to **on**.

**Type**: USERSET

**Value range**: enumerated values

-  **none**: Resources are not monitored.
-  **query**: enables query-level resource monitoring. If this function is enabled, the plan information (similar to the output information of EXPLAIN) of SQL statements will be recorded in top SQL statements.
-  **perf**: enables the perf-level resource monitoring. If this function is enabled, the plan information (similar to the output information of EXPLAIN ANALYZE) that contains the actual execution time and the number of execution rows will be recorded in the top SQL.
-  **operator_realtime**: enables the operator-level resource monitoring. If this function is enabled, the operator information of jobs running in real time is recorded in the top SQL statements, but is not persisted to the historical top SQL statements.
-  **operator**: enables the operator-level resource monitoring. If this function is enabled, not only the information including the actual execution time and number of execution rows is recorded in the top SQL statement, but also the operator-level execution information is updated to the top SQL statement.

**Default value**: **query**

fast_obs_tablesize_method
-------------------------

**Parameter description**: Specifies the method for quickly calculating the size of column-store V3 and V3 HStore Opt tables. This parameter is supported only by clusters of version 9.1.0.100 or later.

**Type**: USERSET

**Value range**: enumerated values

-  **0**: The table size is calculated by listing OBS files.
-  **1**: The table size is calculated through WLM background statistics using **pg_relfilenode_size**.
-  **2**: The table size is estimated by calculating the maximum offset of each file in cudesc.

**Default value**: **2**

fast_obs_dbsize_method
----------------------

**Parameter description**: Specifies the method for quickly calculating the size of database data on OBS. This parameter is supported only by clusters of version 9.1.0.100 or later.

**Type**: USERSET

**Value range**: enumerated values

-  **0**: The size of the database is directly estimated based on the OBS bucket.
-  **1**: The size of the entire database is normally calculated in regular mode.

**Default value**: **0**

.. _en-us_topic_0000001811490709__section1089022732713:

resource_track_cost
-------------------

**Parameter description**: Specifies the minimum execution cost for resource monitoring on statements in the current session. This parameter is valid only when **enable_resource_track** is set to **on**.

**Type**: USERSET

**Value range**: an integer ranging from -1 to INT_MAX

-  **-1** indicates that resource monitoring is disabled.
-  A value greater than or equal to **0** indicates that statements whose execution cost exceeds this value will be monitored.

**Default value**: **0**

.. note::

   The default value of this parameter is **0** for a new cluster. In upgrade scenarios, the default value of this parameter is the same as that of the source version.

.. _en-us_topic_0000001811490709__section347574425112:

resource_track_duration
-----------------------

**Parameter description**: Specifies the minimum statement execution time that determines whether information about jobs of a statement recorded in the real-time view (see :ref:`Table 1 <en-us_topic_0000001764491512__table16116143418462>`) will be dumped to a historical view after the statement is executed. Job information will be dumped from the real-time view (with the suffix **statistics**) to a historical view (with the suffix **history**) if the statement execution time is no less than this value. This parameter is valid only when **enable_resource_track** is set to **on**.

**Type**: USERSET

**Value range**: an integer ranging from 0 to INT_MAX. The unit is second (s).

-  **0** indicates that information about all statements recorded in the real-time resource monitoring view (see :ref:`Table 1 <en-us_topic_0000001764491512__table16116143418462>`) will be archived into historical views.
-  If the value is greater than **0**, the system archives historical information if the total execution and queuing time of statements in the real-time resource monitoring view (:ref:`Table 1 <en-us_topic_0000001764491512__table16116143418462>`) goes over the parameter value.

**Default value**: **60s**

.. _en-us_topic_0000001811490709__section177585466812:

resource_track_subsql_duration
------------------------------

**Parameter description**: Filters the minimum execution time of substatements in a stored procedure. This parameter is supported only by clusters of version 8.2.1 or later.

If the execution time of a sub-statement in a stored procedure is greater than the value of this parameter, the job information is dumped to the top SQL archive table. This parameter takes effect only when :ref:`enable_track_record_subsql <en-us_topic_0000001811490709__section7181949101319>` is set to **on**.

**Type**: USERSET

**Value range**: an integer ranging from 0 to INT_MAX. The unit is second (s).

-  If the value is **0**, historical information about all substatements in the stored procedure is archived.
-  If the value is greater than **0**, historical information is archived when the execution time of a substatement in a stored procedure exceeds the value of this parameter.

**Default value**: 180s

query_exception_count_limit
---------------------------

**Parameter description**: Specifies the maximum number of times that a job triggers an exception rule. If the number of times that a job triggers an exception rule reaches the upper limit, the job will be automatically added to the blocklist and cannot be executed anymore. The job can be resumed only after it is removed from the blocklist.

**Type**: USERSET

**Value range**: an integer ranging from -1 to INT_MAX

-  If the value is **-1**, the number of times that a job triggers an exception rule is not limited. That is, the job will not be automatically added to blocklist even if it triggers an exception rule for many times.
-  If the value is greater than or equal to **0**, the job will be added to the blocklist immediately when the number of times it triggers an exception rule reaches the threshold. The values **0** and **1** both indicate that a job is added to blocklist once the job triggers an exception rule.

**Default value**: **-1**

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

.. caution::

   Pay attention to the following when modifying this parameter:

   #. When this parameter is disabled, it means that the user does not need CCN control function, and the CCN memory negative feedback mechanism will be invalid.
   #. When a job is running, if the value of GUC is changed from **OFF** to **ON**, the CCN memory negative feedback mechanism takes effect. If the concurrency is high, the memory may be temporarily unavailable. After the running job is done, the dynamic load function recovers.
   #. When a job is running and most jobs are delivered by users from the default resource pool, you are not advised to change the GUC parameter from **enabled** to **disabled** . It may cause a memory error. When there is no job delivered by users from the default resource pool, then you can change the parameter. You are advised to bind a user resource pool before delivering jobs.

wlm_memory_feedback_adjust
--------------------------

**Parameter description**: Specifies whether to enable memory negative feedback in dynamic load management. (This parameter is supported only by clusters of version 8.2.0 or later.)

Memory is preempted based on the estimated statement memory usage calculated on the CN. If the estimated memory usage of a statement is too high, it will preempt too much memory, causing subsequent jobs to be queued. With the negative memory feedback mechanism, if the cluster memory usage has been overestimated for a period of time, the CCN node will dynamically release some memory for subsequent jobs, improving resource utilization.

**Type**: SIGHUP

**Value range**: a string

-  **on** indicates that memory negative feedback is enabled.
-  **off** indicates that memory negative feedback is disabled.
-  **on()** enables the memory negative feedback function and specifies the time and estimated memory percentage parameter required to trigger the negative feedback. For example, **on(60,50)** indicates that to trigger the negative feedback mechanism, the memory must be overestimated for 60 consecutive seconds, and the preempted memory needs must exceed 50% of the available memory. By default, the wait time before the negative feedback mechanism takes effect is 50 seconds. The minimum estimated total memory usage for triggering the mechanism is over 40% of the available system memory.

**Default value**: **on**

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

.. _en-us_topic_0000001811490709__section27306369458:

session_history_memory
----------------------

**Parameter description**: Specifies the memory size of a historical query view.

**Type**: SIGHUP

**Value range**: an integer ranging from 10 MB to 50% of **max_process_memory**

**Default value**: **100MB**

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

enable_concurrency_scaling
--------------------------

**Parameter description**: Specifies whether to enable the elastic concurrent expansion function. This parameter is supported only by clusters of version 9.1.0.100 or later.

**Type**: SIGHUP

**Value range**: Boolean

-  **on** indicates that the elastic concurrent expansion function is enabled.
-  **off** indicates that the elastic concurrent expansion function is disabled.

**Default value**: **off**

concurrency_scaling_max_idle_time
---------------------------------

**Parameter description**: Specifies the maximum idle time of the elastic logical cluster created for concurrent expansion. If it exceeds the set value of this parameter, it will enter the process of destroying the elastic logical cluster. This parameter is supported only by clusters of version 9.1.0.100 or later.

**Type**: SIGHUP

**Value range**: an integer ranging from 0 to 60, in minutes. The value **0** indicates that the elastic logical cluster created during concurrent expansion will not be destroyed. Exercise caution when setting this value.

**Default value**: **5**

concurrency_scaling_limit_per_main_vw
-------------------------------------

**Parameter description**: Specifies the maximum number of concurrent elastic logical clusters that can be created for each classic logical cluster. This parameter is supported only by clusters of version 9.1.0.100 or later.

**Type**: SIGHUP

**Value range**: an integer ranging from 0 to 32

**Default value**: **5**

concurrency_scaling_max_vw_active_statements
--------------------------------------------

**Parameter description**: Specifies the maximum number of concurrent tasks that can be executed in an elastic logical cluster. This parameter is supported only by clusters of version 9.1.0.100 or later.

**Type**: SIGHUP

**Value range**: an integer ranging from -1 to INT_MAX

-  Value **0** indicates that the elastic logical cluster does not execute any jobs. Exercise caution when setting this value.
-  **-1** indicates that the elastic logical cluster is not under concurrency control. Exercise caution when setting this value. This value is supported only by clusters of version 9.1.0.200 or later.

**Default value**: **60**

concurrency_scaling_max_waiting_statements
------------------------------------------

**Parameter description**: Specifies the number of queued elastic jobs in the global queue that trigger the creation process of the elastic logical cluster for concurrent expansion. This parameter is supported only by clusters of version 9.1.0.100 or later.

If the number of queued elastic jobs is greater than or equal to the value set for this parameter, and the number of concurrently created elastic logical clusters does not exceed the value set for **concurrency_scaling_limit_per_main_vw**, the process of creating a concurrent expansion elastic logical cluster will be triggered automatically.

**Type**: SIGHUP

**Value range**: an integer ranging from 0 to INT_MAX. The value **0** indicates that an elastic logical cluster is created even if there are no queued jobs. If set to **0**, a large number of resources are consumed. Exercise caution when setting this value.

**Default value**: **10**
