:original_name: dws_06_0133.html

.. _dws_06_0133:

ALTER RESOURCE POOL
===================

Function
--------

**ALTER RESOURCE POOL** changes the Cgroup of a resource pool.

Precautions
-----------

Users having the ALTER permission can modify resource pools.

Syntax
------

::

   ALTER RESOURCE POOL pool_name
       WITH ({MEM_PERCENT=pct | CONTROL_GROUP='group_name' | ACTIVE_STATEMENTS=stmt | MAX_DOP = dop | MEMORY_LIMIT='memory_size' | io_limits=io_limits | io_priority='priority' | nodegroup='nodegroup_name' | except_rule='except_rule' | weight=bandwidth_weight | enable_concurrency_scaling=boolean}[, ... ]);

Parameter Description
---------------------

.. table:: **Table 1** ALTER RESOURCE POOL parameters

   +----------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------+
   | Parameter                  | Description                                                                                                                                                                                                                                                                          | Value Range                                                                                                     |
   +============================+======================================================================================================================================================================================================================================================================================+=================================================================================================================+
   | pool_name                  | Specifies the name of the resource pool to be modified.                                                                                                                                                                                                                              | Name of an existing resource pool                                                                               |
   +----------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------+
   | group_name                 | Specifies the name of a Cgroup.                                                                                                                                                                                                                                                      | Name of an existing Cgroup                                                                                      |
   +----------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------+
   | stmt                       | Specifies the maximum number of statements that can be concurrently executed in a resource pool.                                                                                                                                                                                     | Numeral type. The value ranges from -1 to INT_MAX.                                                              |
   +----------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------+
   | dop                        | Specifies the maximum number of simple SQL statements that can be concurrently executed in a resource pool.                                                                                                                                                                          | Numeral type. The value ranges from **1** to *INT_MAX*.                                                         |
   +----------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------+
   | memory_size                | Specifies the estimated maximum memory for a resource pool.                                                                                                                                                                                                                          | Value range: a string                                                                                           |
   |                            |                                                                                                                                                                                                                                                                                      |                                                                                                                 |
   |                            |                                                                                                                                                                                                                                                                                      | -  **default**: The memory limit is half of the resource pool memory.                                           |
   |                            |                                                                                                                                                                                                                                                                                      | -  **unlimited**: no limit.                                                                                     |
   |                            |                                                                                                                                                                                                                                                                                      | -  Value range: 1 KB to 2047 GB.                                                                                |
   +----------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------+
   | mem_percent                | Specifies the proportion of available resource pool memory to the total memory or group user memory.                                                                                                                                                                                 | The value of **mem_percent** for a common user is an integer ranging from 0 to 100. The default value is **0**. |
   +----------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------+
   | io_limits                  | This parameter has been discarded in the 8.1.2 cluster version and is reserved for compatibility with earlier versions.                                                                                                                                                              | ``-``                                                                                                           |
   +----------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------+
   | io_priority                | This parameter has been discarded in the 8.1.2 cluster version and is reserved for compatibility with earlier versions.                                                                                                                                                              | ``-``                                                                                                           |
   +----------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------+
   | except_rule                | Specifies the exception rule set.                                                                                                                                                                                                                                                    | ``-``                                                                                                           |
   |                            |                                                                                                                                                                                                                                                                                      |                                                                                                                 |
   |                            | An exception rule set is a set of rule thresholds that define the job execution time, job queuing time, CPU usage, and maximum disk flushing. It is used together with a resource pool. When an exception rule is triggered during job execution, the job is terminated or degraded. |                                                                                                                 |
   |                            |                                                                                                                                                                                                                                                                                      |                                                                                                                 |
   |                            | For more information, see :ref:`CREATE EXCEPT RULE <dws_06_0281>`.                                                                                                                                                                                                                   |                                                                                                                 |
   +----------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------+
   | weight                     | Specifies the network bandwidth weight of a resource pool.                                                                                                                                                                                                                           | ``-``                                                                                                           |
   +----------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------+
   | enable_concurrency_scaling | Specifies whether to enable the elastic concurrency expansion function. This function is supported only by clusters of version 9.1.0.100 or later. The default value is **false**.                                                                                                   | **true** or **false**                                                                                           |
   |                            |                                                                                                                                                                                                                                                                                      |                                                                                                                 |
   |                            |                                                                                                                                                                                                                                                                                      | -  **true** indicates that the elastic concurrent expansion function of jobs in the resource pool is enabled.   |
   |                            |                                                                                                                                                                                                                                                                                      | -  **false** indicates that the elastic concurrent expansion function of jobs in the resource pool is disabled. |
   +----------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------+

.. note::

   -  You can use double quotation marks (") or single quotation marks (') to set the Cgroup name **group_name**.
   -  The value of **group_name** is case-sensitive.
   -  If **group_name** is not specified, the string "Medium" will be used by default in the syntax, indicating the **Medium** Timeshare Cgroup under **DefaultClass**.
   -  If a database user specifies the Timeshare string (**Rush**, **High**, **Medium**, or **Low**) in the syntax, for example, if **control_group** is set to **High**, the resource pool will be associated with the **High** Timeshare Cgroup under **DefaultClass**.

Examples
--------

Create an example resource pool **pool_test**, whose Cgroup is **Medium Timeshare Workload** under **DefaultClass**.

::

   DROP RESOURCE POOL IF EXISTS pool_test;
   CREATE RESOURCE POOL pool_test;

Specify **High Timeshare Workload** under **DefaultClass** as the Cgroup for the resource pool **pool_test**.

::

   ALTER RESOURCE POOL pool_test WITH (CONTROL_GROUP="High");

Disable the elastic concurrent expansion function for jobs in a specified resource pool.

::

   ALTER RESOURCE POOL pool_test WITH (enable_concurrency_scaling=false);

Helpful Links
-------------

:ref:`CREATE RESOURCE POOL <dws_06_0171>` and :ref:`DROP RESOURCE POOL <dws_06_0202>`
