:original_name: dws_01_1339.html

.. _dws_01_1339:

Load Monitoring
===============


Load Monitoring
---------------

#. Log in to the GaussDB(DWS) management console.

#. On the **Clusters** page, locate the target cluster.

#. In the **Operation** column of the target cluster, click **Monitoring Panel**.

#. In the navigation pane on the left, choose **Monitoring** > **Load Monitoring**.

   On the **Load Monitoring** page, you can view the real-time and historical resource consumption of workload queues.

Workload Queues
---------------

The DMS displays the user-defined workload queue name, real-time and historical resource consumption, and workload queue resource quotas.

-  **Workload Queues**: name of a workload queue
-  **Monitoring**: You can click the monitoring icon to display the historical consumption trends of resources such as the CPU, memory, and disk.
-  **CPU Usage (%)**: real-time CPU usage of a workload queue
-  **CPU Resource (%)**: CPU usage quotas of a workload queue
-  **Real-Time Concurrent Short Queries**: number of concurrent simple queries in a workload queue. Concurrent simple queries are not controlled by the workload queue.
-  **Concurrent Short Queries**: simple concurrency quotas of the workload queue
-  **Real-Time Concurrent Queries**: number of concurrent complex queries in a workload queue. Concurrent complex queries are controlled by the workload queue.
-  **Query Concurrency**: complex concurrency quotas of the workload queue
-  Operation

|image1|

Exception Handling Rules
------------------------

You can click the drop-down list of any workload queue to view the exception handling rule configured for the workload queue.

-  **Name**: type of the supported exception handling rule, including:

   -  **blocktime**: query blocking time. The unit is seconds.
   -  **elapsedtime**: execution duration of a query. The unit is seconds.
   -  **allcputime**: total CPU time spent in executing a query on all DNs. The unit is seconds.
   -  **cpuskewpercent**: CPU time skew of a query executed on DNs. The value depends on the setting of **qualificationtime**.
   -  **qualificationtime**: interval for checking the CPU skew. The unit is seconds. This parameter must be set together with **cpuskewpercent**.
   -  **spillsize**: amount of query data written to disks on DNs. The unit is MB.
   -  **broadcastsize**: size of broadcast operators of a query on DNs. The unit is MB.
   -  **mem_limit**: maximum memory used by a query on a single instance. The unit is MB.

-  **Type**: supported exception handling rule type. The options can be **abort** and **penalty**.
-  **Value**: rule value, which ranges from 0 to UINT_MAX.

|image2|

Waiting Queries
---------------

You can view the waiting queries in a workload queue in real time to identify the service pressure.

-  **User**: user name of a query statement
-  **Application**: application name of a query statement
-  **Database**: name of the database to which a query statement is connected
-  **Queuing Status**: queuing status of a query statement in a workload queue
-  **Wait Time**: waiting time before a query statement is executed, in ms
-  **Workload Queue**: workload queue to which a query statement belongs
-  **Query Statement**: details of a query statement submitted by a user

|image3|

Circuit Breaking Queries
------------------------

You can view the status of a triggered circuit breaking query in a workload queue.

-  **Query ID**: ID of a circuit breaking query
-  **Query Statement**: circuit breaking query statement
-  **Blocking Time (ms)**: blocking time of a circuit breaking statement, in ms
-  **Execution Time (ms)**: execution time of a circuit breaking statement, in ms
-  **CPU Time (ms)**: CPU time consumed by a circuit breaking statement, in ms
-  **CPU Skew (%)**: CPU skew of a circuit breaking statement on each DN
-  **Exception Handling**: exception handling method of a circuit breaking statement
-  **Status**: real-time status of a circuit breaking statement

|image4|

.. |image1| image:: /_static/images/en-us_image_0000001134401064.png
.. |image2| image:: /_static/images/en-us_image_0000001134401066.png
.. |image3| image:: /_static/images/en-us_image_0000001180440437.png
.. |image4| image:: /_static/images/en-us_image_0000001180440435.png
