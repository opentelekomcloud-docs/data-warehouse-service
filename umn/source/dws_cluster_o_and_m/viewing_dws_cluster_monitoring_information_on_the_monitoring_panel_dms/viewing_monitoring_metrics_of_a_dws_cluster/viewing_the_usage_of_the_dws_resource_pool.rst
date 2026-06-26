:original_name: dws_01_1339.html

.. _dws_01_1339:

Viewing the Usage of the DWS Resource Pool
==========================================

You can check the real-time statistics and resource consumption history about resource pools.

Accessing the Resource Monitoring Page
--------------------------------------

#. Log in to the DWS console.
#. Choose **Dedicated Clusters** > **Clusters** and locate the cluster to be monitored.
#. In the **Operation** column of the target cluster, click **Monitoring Panel**.
#. In the navigation pane on the left, choose **Monitoring** > **Resource Pool Monitoring**.

Resource Pool
-------------

You can check user-defined resource pools, real-time and historical resource consumption, and the resource quotas of resource pools. You can click the setting button in the upper right corner of the list to select the metrics to be displayed in the list. For details about the metrics, see :ref:`Table 1 <en-us_topic_0000002329649390__en-us_topic_0000001423159729_table260617164391>`.

.. _en-us_topic_0000002329649390__en-us_topic_0000001423159729_table260617164391:

.. table:: **Table 1** Resource pool metrics

   +-----------------------------------------+--------------------------------------------------------------------------------------------------------------------------------+--------------------------------+
   | Metric Name                             | Description                                                                                                                    | Monitoring Interval (Raw Data) |
   +=========================================+================================================================================================================================+================================+
   | Resource Pool                           | Resource pool name.                                                                                                            | 120s                           |
   +-----------------------------------------+--------------------------------------------------------------------------------------------------------------------------------+--------------------------------+
   | Monitoring                              | You can click the monitoring icon to display the historical consumption trends of resources such as the CPU, memory, and disk. | 120s                           |
   +-----------------------------------------+--------------------------------------------------------------------------------------------------------------------------------+--------------------------------+
   | CPU Usage                               | Real-time CPU usage of a resource pool                                                                                         | 120s                           |
   +-----------------------------------------+--------------------------------------------------------------------------------------------------------------------------------+--------------------------------+
   | CPU Share                               | Percentage of CPU time that can be used by users associated with the current resource pool to execute jobs.                    | 120s                           |
   +-----------------------------------------+--------------------------------------------------------------------------------------------------------------------------------+--------------------------------+
   | Storage                                 | Storage space of a resource pool                                                                                               | 120s                           |
   +-----------------------------------------+--------------------------------------------------------------------------------------------------------------------------------+--------------------------------+
   | Disk Usage                              | Real-time disk usage of a resource pool                                                                                        | 120s                           |
   +-----------------------------------------+--------------------------------------------------------------------------------------------------------------------------------+--------------------------------+
   | Memory                                  | Memory quota of a resource pool                                                                                                | 120s                           |
   +-----------------------------------------+--------------------------------------------------------------------------------------------------------------------------------+--------------------------------+
   | Memory Usage                            | Real-time memory usage of a resource pool                                                                                      | 120s                           |
   +-----------------------------------------+--------------------------------------------------------------------------------------------------------------------------------+--------------------------------+
   | Real-Time Simple Statement Concurrency  | Number of concurrent simple queries in a resource pool. Concurrent simple queries are not controlled by the resource pool      | 120s                           |
   +-----------------------------------------+--------------------------------------------------------------------------------------------------------------------------------+--------------------------------+
   | Simple Statement Concurrency            | Quota for simple concurrency in a resource pool.                                                                               | 120s                           |
   +-----------------------------------------+--------------------------------------------------------------------------------------------------------------------------------+--------------------------------+
   | Real-Time Complex Statement Concurrency | Number of concurrent complex queries in a resource pool. Concurrent complex queries are controlled by the resource pool.       | 120s                           |
   +-----------------------------------------+--------------------------------------------------------------------------------------------------------------------------------+--------------------------------+
   | Complex Statement Concurrency           | Quota for complex concurrency in a resource pool.                                                                              | 120s                           |
   +-----------------------------------------+--------------------------------------------------------------------------------------------------------------------------------+--------------------------------+
   | Operation                               | Resource pool configurations                                                                                                   | ``-``                          |
   +-----------------------------------------+--------------------------------------------------------------------------------------------------------------------------------+--------------------------------+

User Resource Usage
-------------------

Click the arrow next to a resource pool name to expand resource usage details. For details about the metrics, see :ref:`Table 2 <en-us_topic_0000002329649390__en-us_topic_0000001423159729_table218512568387>`.

.. note::

   You can view the resource usage details of the default resource pool only for clusters of version 8.2.1 or later.

.. _en-us_topic_0000002329649390__en-us_topic_0000001423159729_table218512568387:

.. table:: **Table 2** User resource usage metrics

   +--------------+-------------------------------------------------------------------------------------------------------------+--------------------------------+
   | Metric Name  | Description                                                                                                 | Monitoring Interval (Raw Data) |
   +==============+=============================================================================================================+================================+
   | Username     | Name of a user in the current resource pool                                                                 | 120s                           |
   +--------------+-------------------------------------------------------------------------------------------------------------+--------------------------------+
   | CPU Usage    | Real-time CPU usage of a user                                                                               | 120s                           |
   +--------------+-------------------------------------------------------------------------------------------------------------+--------------------------------+
   | CPU Share    | Percentage of CPU time that can be used by users associated with the current resource pool to execute jobs. | 120s                           |
   +--------------+-------------------------------------------------------------------------------------------------------------+--------------------------------+
   | Storage      | Storage space used by a user                                                                                | 120s                           |
   +--------------+-------------------------------------------------------------------------------------------------------------+--------------------------------+
   | Disk Usage   | Real-time disk usage of a user                                                                              | 120s                           |
   +--------------+-------------------------------------------------------------------------------------------------------------+--------------------------------+
   | Memory       | Real-time memory space of a user                                                                            | 120s                           |
   +--------------+-------------------------------------------------------------------------------------------------------------+--------------------------------+
   | Memory Usage | Real-time memory usage of a user                                                                            | 120s                           |
   +--------------+-------------------------------------------------------------------------------------------------------------+--------------------------------+

Queries Waiting in a Resource Pool
----------------------------------

You can view the queries waiting in a resource pool in real time to check workload status. For details about the metrics, see :ref:`Table 3 <en-us_topic_0000002329649390__en-us_topic_0000001423159729_table14926181714436>`.

.. _en-us_topic_0000002329649390__en-us_topic_0000001423159729_table14926181714436:

.. table:: **Table 3** Metrics of queries waiting in a resource pool

   +------------------+--------------------------------------------------------------------+--------------------------------+
   | Metric Name      | Description                                                        | Monitoring Interval (Raw Data) |
   +==================+====================================================================+================================+
   | User             | Username of a query statement                                      | 60s                            |
   +------------------+--------------------------------------------------------------------+--------------------------------+
   | Application name | Application name of a query statement                              | 60s                            |
   +------------------+--------------------------------------------------------------------+--------------------------------+
   | Database         | Name of the database corresponding to a query statement            | 60s                            |
   +------------------+--------------------------------------------------------------------+--------------------------------+
   | Queuing Status   | Queuing status of a query statement in a resource pool             | 60s                            |
   +------------------+--------------------------------------------------------------------+--------------------------------+
   | Wait Time (ms)   | Waiting time before a query statement is executed, in milliseconds | 60s                            |
   +------------------+--------------------------------------------------------------------+--------------------------------+
   | Resource Pool    | Resource pool to which a query statement belongs                   | 60s                            |
   +------------------+--------------------------------------------------------------------+--------------------------------+
   | Statement        | Details of a query statement submitted by a user                   | 60s                            |
   +------------------+--------------------------------------------------------------------+--------------------------------+

Checking Circuit Breaking Queries
---------------------------------

You can view the status of a triggered circuit breaking query in a resource pool. For details about the metrics, see :ref:`Table 4 <en-us_topic_0000002329649390__en-us_topic_0000001423159729_table1640916109467>`.

.. _en-us_topic_0000002329649390__en-us_topic_0000001423159729_table1640916109467:

.. table:: **Table 4** Circuit breaking query metrics

   +---------------------+--------------------------------------------------------------------+--------------------------------+
   | Metric Name         | Description                                                        | Monitoring Interval (Raw Data) |
   +=====================+====================================================================+================================+
   | Query ID            | ID of a circuit breaking statement                                 | 60s                            |
   +---------------------+--------------------------------------------------------------------+--------------------------------+
   | Statement           | Circuit breaking query statement                                   | 60s                            |
   +---------------------+--------------------------------------------------------------------+--------------------------------+
   | Blocking Time (ms)  | Blocking time of a circuit breaking statement, in milliseconds     | 60s                            |
   +---------------------+--------------------------------------------------------------------+--------------------------------+
   | Execution Time (ms) | Execution time of a circuit breaking statement, in milliseconds    | 60s                            |
   +---------------------+--------------------------------------------------------------------+--------------------------------+
   | CPU Time (ms)       | CPU time consumed by a circuit breaking statement, in milliseconds | 60s                            |
   +---------------------+--------------------------------------------------------------------+--------------------------------+
   | CPU Skew (%)        | CPU skew of a circuit breaking statement on each DN                | 60s                            |
   +---------------------+--------------------------------------------------------------------+--------------------------------+
   | Exception Handling  | Exception handling method of a circuit breaking statement          | 60s                            |
   +---------------------+--------------------------------------------------------------------+--------------------------------+
   | Status              | Real-time status of a circuit breaking statement                   | 60s                            |
   +---------------------+--------------------------------------------------------------------+--------------------------------+
