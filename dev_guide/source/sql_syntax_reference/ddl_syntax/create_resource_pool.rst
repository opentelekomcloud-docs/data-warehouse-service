:original_name: dws_06_0171.html

.. _dws_06_0171:

CREATE RESOURCE POOL
====================

Function
--------

**CREATE RESOURCE POOL** creates a resource pool and specifies the Cgroup for the resource pool.

Precautions
-----------

As long as the current user has **CREATE** permission, it can create a resource pool.

Syntax
------

::

   CREATE RESOURCE POOL pool_name
       [WITH ({MEM_PERCENT=pct | CONTROL_GROUP="group_name" | ACTIVE_STATEMENTS=stmt | MAX_DOP = dop | MEMORY_LIMIT='memory_size' | io_limits=io_limits | io_priority='io_priority' | nodegroup="nodegroupname" | is_foreign=boolean }[, ... ])];

Parameter Description
---------------------

-  **pool_name**

   Specifies the name of a resource pool.

   The name of a resource pool cannot be same as that of an existing resource pool.

   Value range: a string. It must comply with the naming convention.

-  **group_name**

   Specifies the name of a Cgroup.

   .. note::

      -  You can use either double quotation marks ("") or single quotation mark ('') in the syntax when setting the name of a Cgroup.
      -  The value of **group_name** is case-sensitive.
      -  If **group_name** is not specified, the string "Medium" will be used by default in the syntax, indicating the **Medium** Timeshare Cgroup under DefaultClass.
      -  If an administrator specifies a Workload Cgroup under Class, for example, **control_group** set to **class1:workload1**, the resource pool will be associated with the **workload1** Cgroup under **class1**. The level of Workload can also be specified. For example, **control_group** is set to **class1:workload1:1**.
      -  If a database user specifies the Timeshare string (**Rush**, **High**, **Medium**, or **Low**) in the syntax, for example, if **control_group** is set to **High**, the resource pool will be associated with the **High** Timeshare Cgroup under **DefaultClass**.
      -  In multi-tenant scenarios, the Cgroup associated with a group resource pool is a Class Cgroup, and that associated with a service resource pool is a Workload Cgroup. Additionally, switching Cgroups between different resource pools is not allowed.

   Value range: a string. It must comply with the rule in the description, specifying an existing Cgroup.

-  **stmt**

   Specifies the maximum number of statements that can be concurrently executed in a resource pool.

   Value range: Numeric data ranging from **-1** to **INT_MAX**.

-  **dop**

   This is a reserved parameter.

   Value range: Numeric data ranging from **1** to **INT_MAX**.

-  **memory_size**

   Specifies the maximum storage for a resource pool.

   Value range: a string, from **1KB** to **2047GB**.

-  **mem_percent**

   Specifies the proportion of available resource pool memory to the total memory or group user memory.

   In multi-tenant scenarios, **mem_percent** of group users or service users ranges from **1** to **100**. The default value is **20**.

   In common scenarios, **mem_percent** of common users ranges from **0** to **100**. The default value is **0**.

   .. note::

      When both of **mem_percent** and **memory_limit** are specified, only **mem_percent** takes effect.

-  **io_limits**

   Specifies the upper limit of IOPS in a resource pool.

   The IOPS is counted by ones for column storage and by 10 thousands for row storage.

-  **io_priority**

   Specifies the I/O priority for jobs that consume many I/O resources. It takes effect when the I/O usage reaches 90%.

   There are three priorities: **Low**, **Medium**, and **High**. If you do not want to control I/O resources, use the default value **None**.

   .. note::

      The settings of **io_limits** and **io_priority** are valid only for complex jobs, such as batch import (using **INSERT INTO SELECT**, **COPY FROM**, or **CREATE TABLE AS**), complex queries involving over 500 MB data on each DN, and **VACUUM FULL**.

-  **nodegroup**

   Specifies the name of a logical cluster where the resource pool is. The logical cluster must already exist.

   If the logical cluster name contains uppercase letters or special characters or begins with a digit, enclose the name with double quotation marks in SQL statements.

-  **is_foreign**

   In logical cluster mode, lets the current resource pool to control the resources of common users that are not associated with the logical cluster specified by **nodegroup**.

   .. note::

      -  **nodegroup** must specify an existing logical cluster, and cannot be **elastic_group** or the default Node Group (**group_version1**), which is generated during cluster installation.
      -  If **is_foreign** is set to **true**, the resource pool cannot be associated with users. That is, **CREATE USER...** **RESOURCE POOL** cannot be used to configure resource pools for users. The resource pool automatically checks whether the users are associated with its logical cluster. If they are not, they will be controlled by the resource pool when performing operations on DNs in the logical cluster.

Examples
--------

This example assumes that Cgroups have been created by users in advance.

Create a default resource pool, and associate it with the **Medium** Timeshare Cgroup under Workload under **DefaultClass**.

::

   CREATE RESOURCE POOL pool1;

Create a resource pool, and associate it with the **High** Timeshare Cgroup under Workload under **DefaultClass**.

::

   CREATE RESOURCE POOL pool2 WITH (CONTROL_GROUP="High");

Create a resource pool, and associate it with the **Low** Timeshare Cgroup under Workload under **class1**.

::

   CREATE RESOURCE POOL pool3 WITH (CONTROL_GROUP="class1:Low");

Create a resource pool, and associate it with the **wg1** Workload Cgroup under **class1**.

::

   CREATE RESOURCE POOL pool4 WITH (CONTROL_GROUP="class1:wg1");

Create a resource pool, and associate it with the **wg2** Workload Cgroup under **class1**.

::

   CREATE RESOURCE POOL pool5 WITH (CONTROL_GROUP="class1:wg2:3");

Helpful Links
-------------

:ref:`ALTER RESOURCE POOL <dws_06_0133>`, :ref:`DROP RESOURCE POOL <dws_06_0202>`
