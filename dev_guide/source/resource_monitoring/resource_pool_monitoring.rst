:original_name: dws_04_0974.html

.. _dws_04_0974:

Resource Pool Monitoring
========================

Overview
--------

In the multi-tenant management framework, if queries are associated with resource pools, the resources occupied by the queries are summarized to the associated resource pools. You can query the real-time resource usage of all resource pools in the resource pool monitoring view and query the historical resource usage of resource pools in the resource pool monitoring history table.

The resource pool monitoring data is updated every 5s. However, due to the time difference between CNs and DNs, the actual monitoring data update time may be longer than 5s. Generally, the time does not exceed 10s. The resource pool monitoring data is persisted every 30 seconds. The resource pool monitoring logic is basically the same as that of the user resource monitoring. Therefore, the **enable_user_metric_persistent** and **user_metric_retention_time** parameters are used to control the persistence and aging of resource pool monitoring data, respectively.

Resources monitored by a resource pool include the running and queuing information of fast and slow lane jobs, and CPU, memory, and logical I/O resource monitoring information. The monitoring views and history tables are as follows:

#. Real-time monitoring view of resource pools (single CN): :ref:`GS_RESPOOL_RUNTIME_INFO <dws_04_0976>`
#. Real-time monitoring view of resource pools (all CNs): :ref:`PGXC_RESPOOL_RUNTIME_INFO <dws_04_0980>`
#. Real-time monitoring view of resource pool resources (single CN): :ref:`GS_RESPOOL_RESOURCE_INFO <dws_04_0977>`
#. Real-time monitoring view of resource pool resources (all CNs): :ref:`PGXC_RESPOOL_RESOURCE_INFO <dws_04_0981>`
#. Historical resource monitoring table of the resource pool (single CN):
#. Monitoring view of historical resource pool resources (all CNs):

.. note::

   -  Resource pool monitoring monitors the CPU, I/O, and memory usage of all jobs on the fast and slow lanes.
   -  Currently, the memory and CPU usage of fast track jobs are not controlled. When the fast lane jobs occupy a large number of resources, the used resources may exceed the resource limit.
   -  In the monitoring view of DN resource pools, I/O, memory, and CPU display the resource usage and limits of resource pools.
   -  In the monitoring view of CN resource pools, I/O, memory, and CPU display the total resource usage and limit of all DN resource pools in the cluster.
   -  Resource pool monitoring information on DNs is updated every 5 seconds. CNs collect resource pool monitoring information from DNs every 5 seconds. Because each instance updates or collects resource pool monitoring information independently, the monitoring information update time on each instance may be different.
   -  The auxiliary thread automatically invokes the persistence function every 30 seconds to make the resource pool monitoring data persistent. So, normally, you don't need to do this.

Procedure
---------

-  Querying the real-time running status of jobs in a resource pool.

   ::

      SELECT * FROM GS_RESPOOL_RUNTIME_INFO;

   The result view is as follows:

   ::

       nodegroup |    rpname    | ref_count | fast_run | fast_wait | slow_run | slow_wait
      -----------+--------------+-----------+----------+-----------+----------+-----------
       vc1       | p2           |        10 |        0 |         0 |        0 |         0
       vc2       | p3           |        10 |        5 |         5 |        0 |         0
       vc2       | p4           |         0 |        0 |         0 |        0 |         0
       vc1       | default_pool |         0 |        0 |         0 |        0 |         0
       vc2       | default_pool |         0 |        0 |         0 |        0 |         0
       vc1       | p1           |        20 |        5 |         5 |        3 |         7
      (6 rows)

   Where,

   #. **ref_count** indicates the number of jobs that reference the current resource pool information. Its value will be retained until the management ends.
   #. **fast_run** and **slow_run** are load management accounting information. Their values are valid only when **fast_limit** and **slow_limit** are larger than **0**.
   #. This view is valid only on CNs. The persistence information is stored in **GS_RESPOOL_RESOURCE_HISTORY**.
   #. For details about each field, see :ref:`GS_RESPOOL_RUNTIME_INFO <dws_04_0976>`.

-  Querying the resource quota and real-time resource usage of a resource pool.

   ::

      SELECT * FROM GS_RESPOOL_RESOURCE_INFO;

   The result view is as follows:

   ::

      nodegroup |    rpname    |       cgroup        | ref_count | fast_run | fast_wait | fast_limit | slow_run | slow_wait | slow_limit | used_cpu | cpu_limit | used_mem | estimate_mem | mem_limit |read_kbytes | write_kbytes | read_counts | write_counts | read_speed | write_speed
      -----------+--------------+---------------------+-----------+----------+-----------+------------+----------+-----------+------------+----------+-----------+----------+--------------+-----------+-------------+--------------+-------------+--------------+------------+-------------
       vc1       | p2           | DefaultClass:Rush   |        10 |        0 |         0 |         -1 |        0 |         0 |         10 |     9.97 |        48 |       20 |            0 |     11555 |          8 |         2880 |           1 |          360 |          1 |         589
       vc2       | p3           | DefaultClass:Rush   |        10 |        5 |         5 |          5 |        0 |         0 |         10 |     4.98 |        48 |       11 |            0 |     11555 |          0 |          848 |           0 |          106 |          0 |         173
       vc2       | p4           | DefaultClass:Rush   |         0 |        0 |         0 |         -1 |        0 |         0 |         10 |        0 |        48 |        0 |            0 |     11555 |          0 |            0 |           0 |            0 |          0 |           0
       vc1       | default_pool | DefaultClass:Medium |         0 |        0 |         0 |         -1 |        0 |         0 |         -1 |        0 |        48 |        0 |            0 |     11555 |          0 |            0 |           0 |            0 |          0 |           0
       vc2       | default_pool | DefaultClass:Medium |         0 |        0 |         0 |         -1 |        0 |         0 |         -1 |        0 |        48 |        0 |            0 |     11555 |          0 |            0 |           0 |            0 |          0 |           0
       vc1       | p1           | DefaultClass:Rush   |        20 |        5 |         5 |          5 |        3 |         7 |          3 |     7.98 |        48 |       16 |          768 |     11555 |          8 |         2656 |           1 |          332 |          1 |         543
      (6 rows)

   #. This view is valid on both CNs and DNs. The CPU, memory, and I/O usage on a DN indicates the resource consumption of the DN. The CPU, memory, and I/O usage on a CN is the total resource consumption of all DNs in the cluster.
   #. **estimate_mem** is valid only on CNs under dynamic load management. It displays the estimated memory accounting of the resource pool.
   #. I/O monitoring information is recorded only when **enable_logical_io_statistics** is enabled.
   #. For details about each field, see :ref:`GS_RESPOOL_RESOURCE_INFO <dws_04_0977>`.

-  Querying the resource quota and historical resource usage of a resource pool.

   ::

      SELECT * FROM GS_RESPOOL_RESOURCE_HISTORY ORDER BY timestamp DESC;

   The result view is as follows:

   ::

      timestamp           |  nodegroup   |    rpname    |       cgroup        | ref_count | fast_run | fast_wait | fast_limit | slow_run | slow_wait | slow_limit | used_cpu | cpu_limit | used_mem | estimate_mem | mem_limit | read_kbytes | write_kbytes | read_counts | write_counts | read_speed | write_speed
      -------------------------------+--------------+--------------+---------------------+-----------+----------+-----------+------------+----------+-----------+------------+----------+-----------+----------+--------------+-----------+-------------+--------------+-------------+--------------+------------+-------------
       2022-03-04 09:41:57.53739+08  | vc1          | p2           | DefaultClass:Rush   |        10 |        0 |         0 |         -1 |        0 |         0 |         10 |     9.97 |        48 |   20 |            0 |     11555 |           0 |         2320 |           0 |          290 |          0 |         474
       2022-03-04 09:41:57.53739+08  | vc1          | p1           | DefaultClass:Rush   |        20 |        5 |         5 |          5 |        3 |         7 |          3 |     7.98 |        48 |   16 |          768 |     11555 |           0 |         1896 |           0 |          237 |          0 |         387
       2022-03-04 09:41:57.53739+08  | vc2          | default_pool | DefaultClass:Medium |         0 |        0 |         0 |         -1 |        0 |         0 |         -1 |        0 |        48 |    0 |            0 |     11555 |           0 |            0 |           0 |            0 |          0 |           0
       2022-03-04 09:41:57.53739+08  | vc1          | default_pool | DefaultClass:Medium |         0 |        0 |         0 |         -1 |        0 |         0 |         -1 |        0 |        48 |    0 |            0 |     11555 |           0 |            0 |           0 |            0 |          0 |           0
       2022-03-04 09:41:57.53739+08  | vc2          | p4           | DefaultClass:Rush   |         0 |        0 |         0 |         -1 |        0 |         0 |         10 |        0 |        48 |    0 |            0 |     11555 |           0 |            0 |           0 |            0 |          0 |           0
       2022-03-04 09:41:57.53739+08  | vc2          | p3           | DefaultClass:Rush   |        10 |        5 |         5 |          5 |        0 |         0 |         10 |     4.99 |        48 |   11 |            0 |     11555 |           0 |          880 |           0 |          110 |          0 |         180
       2022-03-04 09:41:27.335234+08 | vc2          | p3           | DefaultClass:Rush   |        10 |        5 |         5 |          5 |        0 |         0 |         10 |     4.98 |        48 |   11 |            0 |     11555 |           0 |          856 |           0 |          107 |          0 |         175

   #. The monitoring information comes from the resource pool monitoring history table. When **enable_user_metric_persistent** is enabled, the monitoring information is recorded every 30 seconds.
   #. The storage duration of the table data is specified by the **user_metric_retention_time** parameter.
   #. For details about each field, see :ref:`GS_RESPOOL_RESOURCE_HISTORY <dws_04_0975>`.
