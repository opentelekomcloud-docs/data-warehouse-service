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
       WITH ({MEM_PERCENT= pct | CONTROL_GROUP="group_name" | ACTIVE_STATEMENTS=stmt | MAX_DOP = dop | MEMORY_LIMIT='memory_size' | io_limits=io_limits | io_priority='io_priority'}[, ... ]);

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

   This is a reserved parameter.

   Value range: Numeric data ranging from **-1** to **INT_MAX**.

-  **memory_size**

   Specifies the maximum storage for a resource pool.

   Value range: a string, from **1KB** to **2047GB**.

-  **mem_percent**

   Specifies the proportion of available resource pool memory to the total memory or group user memory.

   The value of **mem_percent** for a common user is an integer ranging from 0 to 100. The default value is **0**.

-  **io_limits**

   This parameter has been discarded in 8.1.2 and is reserved for compatibility with earlier versions.

-  **io_priority**

   This parameter has been discarded in 8.1.2 and is reserved for compatibility with earlier versions.

Examples
--------

Create an example resource pool **pool_test**, whose Cgroup is **Medium Timeshare Workload** under **DefaultClass**.

::

   CREATE RESOURCE POOL pool_test;

Specify **High Timeshare Workload** under **DefaultClass** as the Cgroup for the resource pool **pool_test**.

::

   ALTER RESOURCE POOL pool_test WITH (CONTROL_GROUP="High");

Helpful Links
-------------

:ref:`CREATE RESOURCE POOL <dws_06_0171>`, :ref:`DROP RESOURCE POOL <dws_06_0202>`
