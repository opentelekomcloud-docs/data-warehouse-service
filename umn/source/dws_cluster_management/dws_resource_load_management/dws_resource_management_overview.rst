:original_name: dws_01_1510.html

.. _dws_01_1510:

DWS Resource Management Overview
================================

The system resources (CPU, memory, I/O, and storage resources) of a database are limited. When multiple types of services (such as data loading, batch analysis, and real-time query) are running at the same time, they may compete for resources and hinder operations. As a result, the throughput decreases and the overall performance deteriorates. Allocating system resources correctly prevents efficiency loss due to poor resource use. DWS offers a resource management feature that lets you create separate resource pools for different needs. These pools keep resources independent from one another.

:ref:`Figure 1 Resource management architecture <en-us_topic_0000002344412841__en-us_topic_0000001422799525_fig177484215719>` shows the basic process structure of resource management.

-  CPU, memory, and I/O resources are managed using a resource pool. Centralized control over resources prevents resource conflicts of jobs, performs high-priority jobs before others, and isolates user resources.
-  Data storage space: You can specify the storage space size by setting **Storage Resource (MB)** when creating a resource pool. It limits the storage space quotas for users.
-  Memory can be controlled at node and job levels. You can allocate and manage CPUs, memory, I/O, and storage resources according to the defined resource pool during job execution.

.. _en-us_topic_0000002344412841__en-us_topic_0000001422799525_fig177484215719:

.. figure:: /_static/images/en-us_image_0000002323336312.png
   :alt: **Figure 1** Resource management architecture

   **Figure 1** Resource management architecture

Functions
---------

The resource management function consists of the following parts:

-  Resource management configuration: you can configure whether to enable resource management and the maximum number of global concurrent requests. The system allows a limited number of simultaneous tasks per CN. Disabling the resource manager turns off all its features.
-  :ref:`Resource pools <dws_01_07231>`: Compute resources are isolated and restricted to prevent cluster-level exceptions caused by abnormal SQL queries.
-  :ref:`Resource management plan <dws_01_72363>`: Resources are managed automatically based on a preconfigured plan, which can flexibly cope with complex scenarios.
-  :ref:`Schema space management <dws_01_72366>`: Set the maximum storage space of a schema to prevent the disk space from being used up and the database from being read-only.
-  **User space management**: When creating a resource pool, you can set the storage space size and associate the resource pool with a user. For details, see :ref:`Creating a resource pool <en-us_topic_0000002344532677__en-us_topic_0000001372520138_section1239585215495>`.
-  :ref:`Exception rules <dws_01_72367>`: To avoid query blocking or performance deterioration, you can configure exception rules to let the service automatically identify and handle abnormal queries, preventing slow SQL statements from occupying too many resources for a long time.
-  :ref:`Query filter <dws_01_72368>`: The query filter creates rules to block problematic SQL statements early, preventing issues like poor performance or complete cluster failure.

Notes and Constraints
---------------------

-  Resource management is supported only by clusters of version 8.0.0 or later.
-  Only clusters of version 8.1.3 and later versions support the dedicated CPU limit.
-  Resource management plans are supported only by clusters of version 8.1.0.100 or later.

Concurrency Management
----------------------

DWS supports fine-grained resource management. Before resource management is implemented, queries are classified into complex queries (with long execution time and high resource consumption) and simple queries (with short execution time and low resource consumption). Simple and complex queries also differ in their estimated memory usage.

-  The estimated memory usage of a simple query is less than 32 MB.
-  The estimated memory usage of a complex query is 32 MB or higher.

In a hybrid load database, complex queries often occupy a large number of resources for a long time. A simple query queued after a complex query is time consuming, because it has to wait for the complex query to complete and resources to be freed up. To improve execution efficiency and system throughput, DWS provides the short query acceleration function, managing simple queries separately. In the **Short Query Configuration** area, you can determine whether to enable the short query acceleration function. To change the number of concurrent simple statements (the default value is **-1**, and **0** or **-1** indicates no control), you can enable short query acceleration.

The concurrency management rules are as follows:

-  If short query acceleration is enabled, complex queries are under resource pool concurrency control, and simple queries are under short query concurrency control. Simple queries and complex queries are managed separately. Simple queries do not need to compete with complex queries for resources.
-  If short query acceleration is disabled, complex and simple queries are both under resource pool concurrency control. Short query concurrency control is invalid. Both simple and complex queries handle resources similarly. Memory-based query division relies on accurate memory estimates. However, query runtime and CPU usage do not always align with memory usage. For less critical or specialized workloads, disabling short query acceleration allows better resource control and error handling for simple tasks.

While one simple task uses minimal resources, many such tasks together consume significant amounts. When short query acceleration is active, managing concurrency—limiting the number of simultaneous queries—helps reduce resource conflicts and ensures efficiency. Without this management, simple queries bypass resource controls, and exception rules will not apply, potentially impacting overall performance and system capacity.

Memory Management
-----------------

Each resource pool occupies a certain percentage of memory.

Memory management aims to prevent out of memory (OOM) in a database, isolate the memory of different resource pools, and to control memory usage. Memory is managed from the following aspects:

-  Global memory management

   To prevent OOM, set the global memory upper limit (**max_process_memory**) to a proper value. Global memory management before a query controls memory usage to prevent OOM management. Global memory management during a query prevents errors during query execution.

   -  Management before a query

      The service checks the estimated memory usage of a query in the slow queue, and compares it with the actual usage. The estimation will be adjusted if it is smaller than the actual usage. Before a query is executed, the service checks whether the available memory is sufficient for the query. If yes, the query can be executed directly. If no, the query needs to be queued and executed after other queries release resources. **Concurrency** and **memory** are **managed** in this phase.

   -  Management during a query

      During a query, the service checks whether the requested memory exceeds a certain limit. If yes, an error will be reported, and memory occupied by the query will be released. **Memory, CPUs, storage space, and exception rules** are **managed** in this phase.

-  Resource pool memory management

   Resource pool memory management puts a limit on dedicated quotas. A workload queue can only use the memory allocated to it, and cannot use idle memory in other resource pools.

   The resource pool memory is allocated in percentage. The value range is 0 to 100. The value **0** indicates that the resource pool does not perform memory management. The value **100** indicates that the resource pool performs memory management and can use all the global memory.

   The sum of memory percentages allocated to all resource pools cannot exceed 100. Resource pool memory management is performed only before a query in the slow queue starts. It works in a way similar to the global memory management before a query. Before a query in the slow queue in a resource pool is executed, its memory usage is estimated. If the estimation is greater than the resource pool memory, the query needs to be queued and can be executed only after earlier queries in the pool are complete and resources released.

.. _en-us_topic_0000002344412841__section8129175313310:

CPU Management
--------------

DWS primarily utilizes Cgroups to manage CPU resources, which involves the CPU, cpuacct, and cpuset subsystems. The CPU share is implemented based on the **cpu.shares** of the CPU subsystem. The CPU control is triggered only when the OS CPU is fully occupied. CPU limit is implemented based on the **cpuset**. The **cpuacct** subsystem is used to monitor the CPU resource usage. CPU sharing and CPU limit can be managed. In the **Resource Configuration** area, you can modify **CPU Sharing** and **CPU Limit** of the current resource pool.

-  **CPU Sharing**: If the system is heavily loaded, CPU resources are allocated to resource pools based on the specific CPU shares. If the system not busy, this configuration does not take effect.

   -  **Sharing**: The CPU is shared by all Cgroups, and other Cgroups can use idle CPU resources.
   -  **Limit**: When the CPU is fully loaded during peak hours, Cgroups preempt CPU resources based on their limits.

   CPU share is implemented based on **cpu.shares** and takes effect only when the CPU is fully loaded. When the CPU is idle, there is no guarantee that a Cgroup will preempt CPU resources appropriate to its quota. There can still be resource contention when the CPU is idle. Tasks in a Cgroup can use CPU resources without restriction. Although the average CPU usage may not be high, CPU resource contention may still occur at a specific time.

   For example, 10 jobs are running on 10 CPUs, and one job is running on each CPU. In this case, any job request for CPU resources will be responded to instantly, and there is no contention. If 20 jobs are running on 10 CPUs, the CPU usage may still not be high because the jobs do not always occupy the CPU and may wait for I/O and network resources. The CPU resources seem idle. However, if 2 or more jobs request one CPU at the same time, CPU resource contention occurs, affecting job performance.

-  **CPU Limit**: It specifies the maximum number of CPU cores used by a resource pool. The resource usage of jobs in the resource pool cannot exceed this limit no matter whether the system is busy or not.

   -  **Dedicated**: The CPU is dedicated to a Cgroup. Other Cgroups with quotas cannot use idle CPU resources.
   -  **Quota**: Only the CPU resources in the allocated quota can be used. Idle CPU resources of other Cgroups cannot be preempted.

   CPU limit is implemented based on **cpuset.cpu**. You can set a proper quota to implement absolute isolation of CPU resources between Cgroups. In this way, tasks of different Cgroups will not affect each other. However, the absolute CPU isolation will cause idle CPU resources in a Cgroup to be wasted. Therefore, the limit cannot be too large. A larger limit may not bring a better performance.

   For example, in one case, 10 jobs are running on 10 CPUs and the average CPU usage is about 5%. In another case, 10 jobs are running on 5 CPUs and the average CPU usage is about 10%. According to the preceding analysis, although the CPU usage is low when 10 jobs run on five CPUs. However, CPU resource contention still exists. Therefore, the performance of running 10 jobs on 10 CPUs is better than that of running 10 jobs on 5 CPUs. However, it is not the more CPUs, the better. If ten jobs run on 20 CPUs, at any time point, at least 10 CPUs are idle. Therefore, theoretically, running 10 jobs on 20 CPUs does not have better performance than running 10 CPUs. For a Cgroup with a concurrency of N, if the number of allocated CPUs is less than N, the job performance is better with more CPUs. However, if the number of allocated CPUs is greater than N, the job performance will not be improved with more CPUs.

Choose either of the preceding management methods as needed. In CPU share management, CPUs can be shared and fully utilized, but resource pools are not isolated and may affect the query performance of each other. In CPU limit management, the CPUs of different resource pools are isolated, but this may result in the waste of idle resources. Compared with CPU limit, CPU share has higher CPU usage and overall job throughput. Compared with CPU share, CPU limit has complete CPU isolation, which can better meet the requirements of performance-sensitive users.

If CPU contention occurs when multiple types of jobs are running in the database system, you can select different CPU resource control modes based on different scenarios.

-  Scenario 1: Fully utilize CPU resources. Focus on the overall CPU throughput instead of the performance of a single type of jobs.

   Suggestion: You are advised not to isolate CPUs for individual users as any CPU management can impact overall CPU usage.

-  Scenario 2: A certain degree of CPU resource contention and performance loss are allowed. When the CPU is idle, the CPU resources are fully utilized. When the CPU is fully loaded, each service type needs to use the CPU proportionally.

   Suggestion: You can use CPU share to improve the overall CPU usage while implementing CPU isolation and control when the CPUs are fully loaded.

-  Scenario 3: Some jobs are sensitive to performance and CPU resource waste is allowed.

   Suggestion: You can use CPU limit to implement absolute CPU isolation between different types of jobs.
