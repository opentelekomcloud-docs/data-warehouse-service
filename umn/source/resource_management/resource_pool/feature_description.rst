:original_name: dws_01_1512.html

.. _dws_01_1512:

Feature Description
===================

GaussDB(DWS) resource pools provide concurrency management, memory management, CPU management, and exception rules.

Concurrency Management
----------------------

Concurrency represents the maximum number of concurrent queries in a resource pool. Concurrency management can limit the number of concurrent queries to reduce resource contention and improve resource utilization.

The concurrency management rules are as follows:

-  If short query acceleration is enabled, complex queries are under resource pool concurrency control, and simple queries are under short query concurrency control.
-  If short query acceleration is disabled, complex and simple queries are both under resource pool concurrency control. Short query concurrency control is invalid.

Memory Management
-----------------

Each resource pool occupies a certain percentage of memory.

Memory management aims to prevent out of memory (OOM) in a database, isolate the memory of different resource pools, and to control memory usage. Memory is managed from the following aspects:

-  Global memory management

   To prevent OOM, set the global memory upper limit (**max_process_memory**) to a proper value. Global memory management before a query controls memory usage to prevent OOM management. Global memory management during a query prevents errors during query execution.

   -  Management before a query

      The service checks the estimated memory usage of a query in the slow queue, and compares it with the actual usage. The estimation will be adjusted if it is smaller than the actual usage. Before a query is executed, the service checks whether the available memory is sufficient for the query. If yes, the query can be executed directly. If no, the query needs to be queued and executed after other queries release resources.

   -  Management during a query

      During a query, the service checks whether the requested memory exceeds a certain limit. If yes, an error will be reported, and memory occupied by the query will be released.

-  Resource pool memory management

   Resource pool memory management puts a limit on dedicated quotas. A workload queue can only use the memory allocated to it, and cannot use idle memory in other resource pools.

   The resource pool memory is allocated in percentage. The value range is 0 to 100. The value **0** indicates that the resource pool does not perform memory management. The value **100** indicates that the resource pool performs memory management and can use all the global memory.

   The sum of memory percentages allocated to all resource pools cannot exceed 100. Resource pool memory management is performed only before a query in the slow queue starts. It works in a way similar to the global memory management before a query. Before a query in the slow queue in a resource pool is executed, its memory usage is estimated. If the estimation is greater than the resource pool memory, the query needs to be queued and can be executed only after earlier queries in the pool are complete and resources released.

CPU Management
--------------

CPU share and CPU limit can be managed.

-  CPU share: If the system is heavily loaded, CPU resources are allocated to resource pools based on the specific CPU shares. If the system not busy, this configuration does not take effect.
-  CPU limit: It specifies the maximum number of CPU cores used by a resource pool. The resource usage of jobs in the resource pool cannot exceed this limit no matter whether the system is busy or not.

Choose either of the preceding management methods as needed. In CPU share management, CPUs can be shared and fully utilized, but resource pools are not isolated and may affect the query performance of each other. In CPU limit management, the CPUs of different resource pools are isolated, but this may result in the waste of idle resources.

.. note::

   Only 8.1.3 and later versions support the CPU limit management.

Exception Rules
---------------

To avoid query blocking or performance deterioration, you can configure exception rules to let the service automatically identify and handle abnormal queries, preventing slow SQL statements from occupying too many resources for a long time.

|image1|

The following table describes exception rules.

.. _en-us_topic_0000001467074066__table595493692317:

.. table:: **Table 1** Exception rule parameters

   +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------+-----------------------------------+
   | Parameter                           | Description                                                                                                                                                                                                                    | Value Range (0 Means No Limit)                                                  | Operation                         |
   +=====================================+================================================================================================================================================================================================================================+=================================================================================+===================================+
   | Blocking Time                       | Job blocking time. It refers to the total time spent in global and local concurrent queuing. The unit is second.                                                                                                               | An integer in the range 1 to 2,147,483,647. The value **0** indicates no limit. | **Terminated** or **Not limited** |
   |                                     |                                                                                                                                                                                                                                |                                                                                 |                                   |
   |                                     | For example, if the blocking time is set to 300s, a job executed by a user in the resource pool will be terminated after being blocked for 300 seconds.                                                                        |                                                                                 |                                   |
   +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------+-----------------------------------+
   | Execution Time                      | Time that has been spent in executing the job, in seconds.                                                                                                                                                                     | An integer in the range 1 to 2,147,483,647. The value **0** indicates no limit. | **Terminated** or **Not limited** |
   |                                     |                                                                                                                                                                                                                                |                                                                                 |                                   |
   |                                     | For example, if **Time required for execution** is set to 100s, a job executed by a user in the resource pool will be terminated after being executed for more than 100 seconds.                                               |                                                                                 |                                   |
   +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------+-----------------------------------+
   | Total CPU time on all DNs.          | Total CPU time spent in executing a job on all DNs, in seconds.                                                                                                                                                                | An integer in the range 1 to 2,147,483,647. The value **0** indicates no limit. | **Terminated** or **Not limited** |
   +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------+-----------------------------------+
   | Interval for Checking CPU Skew Rate | Interval for checking the CPU skew, in seconds. This parameter must be set together with **Total CPU Time on All DNs**.                                                                                                        | An integer in the range 1 to 2,147,483,647. The value **0** indicates no limit. | **Terminated** or **Not limited** |
   +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------+-----------------------------------+
   | Total CPU Time Skew Rate on All DNs | CPU time skew rate of a job executed on DNs. The value depends on the setting of **Interval for Checking CPU Skew Rate**.                                                                                                      | An integer in the range 1 to 100. The value **0** indicates no limit.           | **Terminated** or **Not limited** |
   +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------+-----------------------------------+
   | Data Spilled to Disk Per DN         | Allowed maximum job data spilled to disks on a DN. The unit is MB.                                                                                                                                                             | An integer in the range 1 to 2,147,483,647. The value **0** indicates no limit. | **Terminated** or **Not limited** |
   +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------+-----------------------------------+
   | Average CPU Usage Per DN            | Average CPU usage of a job on each DN. If **Interval for Checking CPU Skew Rate** is configured, the interval takes effect for this parameter. If the interval is not configured, the check interval is 30 seconds by default. | An integer in the range 1 to 100. The value **0** indicates no limit.           | **Terminated** or **Not limited** |
   +-------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------+-----------------------------------+

.. |image1| image:: /_static/images/en-us_image_0000001517914141.png
