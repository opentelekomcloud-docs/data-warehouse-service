:original_name: dws_01_1339.html

.. _dws_01_1339:

Resource Pool Monitoring
========================

Accessing the Resource Monitoring Page
--------------------------------------

#. Log in to the GaussDB(DWS) console.

#. Choose **Dedicated Clusters** > **Clusters** and locate the cluster to be monitored.

#. In the **Operation** column of the target cluster, click **Monitoring Panel**.

#. In the navigation pane on the left, choose **Monitoring** > **Resource Pool Monitoring**.

   You can check the real-time statistics and resource consumption history about resource pools.

Resource Pool
-------------

You can check user-defined resource pools, real-time and historical resource consumption, and the resource quotas of resource pools. You can click the setting button in the upper right corner of the list to select the metrics to be displayed in the list. The metrics are as follows:

-  **Resource Pool**: Resource pool name.
-  **Monitoring**: You can click the monitoring icon to display the historical consumption trends of resources such as the CPU, memory, and disk.
-  **CPU Usage**: real-time CPU usage of a resource pool.
-  CPU share: percentage of CPU time that can be used by users associated with the current resource pool to execute jobs.
-  **Storage**: storage space of a resource pool.
-  **Disk Usage**: real-time disk usage of a resource pool.
-  **Memory Resource**: memory quota of a resource pool.
-  **Memory Usage**: percentage of used memory.
-  **Real-Time Concurrent Short Queries**: number of concurrent simple queries in a resource pool. Concurrent simple queries are not controlled by the resource pool.
-  **Simple Statement Concurrency**: quota of simple concurrent queries in a resource pool.
-  **Real-Time Concurrent Queries**: number of concurrent complex queries in a resource pool. Concurrent complex queries are controlled by the resource pool.
-  **Complex Statement Concurrency**: quota of complex concurrent queries in a resource pool.
-  **Operation**: resource pool configurations.

User Resource Usage
-------------------

Click the arrow next to a resource pool name to expand resource usage details. The metrics are as follows:

-  **User Name**: name of a user in the current resource pool
-  **CPU Usage**: real-time CPU usage of a user.
-  **CPU Share**: percentage of CPU time that can be used by users associated with the current resource pool to execute jobs.
-  **Storage Resource**: storage space used by a user.
-  **Disk Usage**: disks used by a user.
-  **Memory Resource**: memory used by a user.
-  **Memory Usage**: percentage of memory used by a user.


.. figure:: /_static/images/en-us_image_0000002203427085.png
   :alt: **Figure 1** User Resource Usage

   **Figure 1** User Resource Usage

Queries Waiting in a Resource Pool
----------------------------------

You can view the queries waiting in a resource pool in real time to check workload status.

-  **User**: user name of a query statement
-  **Application**: application name of a query statement
-  **Database**: name of the database to which a query statement is connected
-  **Queuing Status**: queuing status of a query statement in a resource pool
-  **Wait Time (ms)**: waiting time before a query statement is executed.
-  **Resource Pool**: resource pool that the query belongs to
-  **Query Statement**: details of a query statement submitted by a user

Checking Circuit Breaking Queries
---------------------------------

You can view the status of a triggered circuit breaking query in a resource pool.

-  **Query ID**: ID of a circuit breaking query
-  **Query Statement**: circuit breaking query statement
-  **Blocking Time (ms)**: blocking time of a circuit breaking statement.
-  **Execution Time (ms)**: execution time of a circuit breaking statement.
-  **CPU Time (ms)**: CPU time consumed by a circuit breaking statement.
-  **CPU Skew (%)**: CPU skew of a circuit breaking statement on each DN
-  **Exception Handling**: exception handling method of a circuit breaking statement
-  **Status**: real-time status of a circuit breaking statement
