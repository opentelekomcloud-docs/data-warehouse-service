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
       WITH ({MEM_PERCENT=pct | CONTROL_GROUP="group_name" | ACTIVE_STATEMENTS=stmt | MAX_DOP = dop | MEMORY_LIMIT='memory_size' | io_limits=io_limits | io_priority='priority' | nodegroup='nodegroup_name' | except_rule='except_rule' | weight=bandwidth_weight | enable_concurrency_scaling=boolean}[, ... ]);

Parameter Description
---------------------

-  **pool_name**

   Specifies the name of the resource pool.

   The name of the resource pool is the name of an existing resource pool.

   Value range: a string. It must comply with the naming convention.

-  **group_name**

   Specifies the name of a Cgroup.

   .. note::

      -  You can use either double quotation marks ("") or single quotation mark ('') in the syntax when setting the name of a Cgroup.

      -  The value of **group_name** is case-sensitive.

      -  When **group_name** is not specified, the default value "Medium" is used. It is the "Medium" Timeshare Cgroup

         of the DefaultClass Cgroup.

      -  If a database user specifies the Timeshare string (**Rush**, **High**, **Medium**, or **Low**) in the syntax, for example, if **control_group** is set to **High**, the resource pool will be associated with the **High** Timeshare Cgroup under **DefaultClass**.

   Value range: an existing control group.

-  **stmt**

   Specifies the maximum number of statements that can be concurrently executed in a resource pool.

   Value range: Numeric data ranging from **-1** to **INT_MAX**.

-  **dop**

   Specifies the maximum number of simple SQL statements that can be concurrently executed in a resource pool.

   Value range: Numeric data ranging from **-1** to **INT_MAX**.

-  **memory_size**

   Specifies the maximum storage for a resource pool.

   Value range: a string, from **1KB** to **2047GB**.

-  **mem_percent**

   Specifies the proportion of available resource pool memory to the total memory or group user memory.

   The value of **mem_percent** for a common user is an integer ranging from 0 to 100. The default value is **0**.

-  **io_limits**

   This parameter has been discarded in the 8.1.2 cluster version and is reserved for compatibility with earlier versions.

-  **io_priority**

   This parameter has been discarded in the 8.1.2 cluster version and is reserved for compatibility with earlier versions.

-  **except_rule**

   Name of an exception rule set

-  **weight**

   Specifies the network bandwidth weight of the resource pool.

-  **enable_concurrency_scaling**

   Specifies whether to enable the elastic concurrency expansion function. This function is supported only by clusters of version 9.1.0.100 or later.

   Value range:

   -  **true** indicates that the elastic concurrent expansion function of jobs in the resource pool is enabled.
   -  **false** indicates that the elastic concurrent expansion function of jobs in the resource pool is disabled.

   The default value is **false**.

Examples
--------

Modify a resource pool, and associate it with the **High** Timeshare Cgroup under Workload under **DefaultClass**.

::

   ALTER RESOURCE POOL pool1 WITH (CONTROL_GROUP="High");

Disable the elastic concurrent expansion function for jobs in a specified resource pool.

::

   ALTER RESOURCE POOL pool6 WITH (enable_concurrency_scaling=false);

Helpful Links
-------------

:ref:`CREATE RESOURCE POOL <dws_06_0171>`, :ref:`DROP RESOURCE POOL <dws_06_0202>`
