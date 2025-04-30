:original_name: dws_01_1334.html

.. _dws_01_1334:

Real-Time Queries
=================

Going to the Real-time Query Page
---------------------------------

#. Log in to the GaussDB(DWS) console.

#. Choose **Dedicated Clusters** > **Clusters** and locate the cluster to be monitored.

#. In the **Operation** column of the target cluster, click **Monitoring Panel**.

#. In the navigation pane, choose **Monitoring** > **Queries**.

   You can check the real-time information about all queries and sessions running in the cluster.

.. important::

   -  Real-time query is supported only in clusters of version 8.1.2 and later.
   -  To enable real-time query monitoring, choose **Settings** > **Monitoring**, click **Monitoring Collection**, and enable **Real-Time Query Monitoring**. For details, see :ref:`Monitoring Collection <en-us_topic_0000002203426701__en-us_topic_0000001076708691_section149871230683>`. Enabling real-time query may cause a large amount of data. Exercise caution when performing this operation.

Prerequisites
-------------

You need to set GUC parameters before viewing data on the monitoring page. If GUC parameters are not set, real-time or historical query may be unavailable. However, if this parameter is set, the cluster performance may deteriorate. Therefore, you need to balance the settings of related parameters. The following table lists the recommended GUC parameter settings. For how to modify parameters, see :ref:`Modifying GUC Parameters of the GaussDB(DWS) Cluster <dws_01_0152>`. For details about the parameters, see :ref:`Setting GUC Parameters <en-us_topic_0000002203312113__en-us_topic_0000001076708521_section3665174263916>`.

.. table:: **Table 1** Recommended GUC parameter settings

   ========================= ================ ================
   GUC Parameter             CN Configuration DN Configuration
   ========================= ================ ================
   max_active_statements     10               10
   enable_resource_track     on               on
   resource_track_level      query            query
   resource_track_cost       0                0
   resource_track_duration   10               10
   enable_resource_record    on               on
   session_statistics_memory 1,000 MB         1,000 MB
   ========================= ================ ================

Querying Information
--------------------

You can view the queries statistics, the number of sessions, average session duration (time of all session connections divided by the number of sessions), number of queries, average query duration, and average query waiting time.

|image1|

Checking Live Sessions
----------------------

On the **Sessions** page, you can browse the real-time information about all running queries. You can click the setting button in the upper right corner of the list to select the metrics to be displayed in the list. The metrics are as follows:

Session ID, username, session duration, application name, QueryBand, client IP address, access CN, session status, start time, lock mode, lock holding status, locked object, query SQL, lock wait, current query duration, and current query start time.

Here are the different session statuses:

-  **idle**: The backend is waiting for new client commands.
-  **active**: The backend is executing queries.
-  **idle in transaction**: The backend is in a transaction, but there is no statement being executed in the transaction.
-  **idle in transaction (aborted)**: The backend is in a transaction, but there are statements failed in the transaction.
-  **fastpath function call**: The backend is executing a **fast-path** function.

.. note::

   -  You can click a session ID to view the queries in the current session. For details, see :ref:`Viewing Real-time Query Monitoring Details <en-us_topic_0000002167905996__en-us_topic_0000001076579507_section5136123611463>`.
   -  To terminate a session, select the session, click **Terminate a Session**, and confirm your operation.
   -  If you want to terminate all idle sessions, click **Clear Idle Sessions**.
   -  The fine-grained permission control function is added. Only users with the operate permission are able to terminate sessions. For users with the read-only permission, the **Terminate a Session** button is grayed out.

Checking Real-time Queries
--------------------------

In the real-time query area, you can view details on all queries that are currently active in the cluster during a specific time period. To customize the metrics displayed in the list, click the settings button in the top right corner. The metrics are as follows:

Query ID, username, application name, database name, resource pool, submission time, blocking time (ms), execution time (ms), minimum CPU time (ms), maximum CPU time (ms), CPU time (ms), CPU time skew (%), DN spilling information, minimum spilled data among all DNs (MB), maximum spilled data among all DNs (MB), average spill to disk (MB), DN spill skew, query statement, access CN, client IP address, fast and slow lanes, query status, session ID, queuing status, job type, task name, task instance, TCP port, waiting or not, estimated total execution time (ms), estimated remaining time (ms), cgroup, minimum memory peak of DN (MB), maximum memory peak of DN (MB), average memory usage (MB), memory usage skew ratio across DNs, estimated memory usage (MB), minimum execution time of a statement across all DNs (ms), maximum execution time of a statement across all DNs (ms), average execution time of a statement on all DNs (ms), and execution time skew of a statement among DNs, alarms, average IOPS peak of a statement across all DNs (times/s for column-store tables and 10,000 times/s for row-store tables), I/O skew of a statement among DNs, statement status, and statement attributes.

Here are the different query statuses:

-  **idle**: The backend is waiting for new client commands.
-  **active**: The backend is executing queries.
-  **idle in transaction**: The backend is in a transaction, but there is no statement being executed in the transaction.
-  **idle in transaction (aborted)**: The backend is in a transaction, but there are statements failed in the transaction.
-  **fastpath function call**: The backend is executing a **fast-path** function.

   .. note::

      -  You can click a query ID to view the monitoring details. However, details cannot be displayed for queries whose ID is **0**. Query **0** indicates that an exception occurs during the query.
      -  To terminate a query, select the query, click **Terminate Query**, and confirm your operation.
      -  The fine-grained permission control function is added. Only users with the operate permission are able to terminate queries. For users with the read-only permission, the **Terminate Query** button is grayed out.
      -  The fast and slow lanes are selected based on the cost in the execution plan. If the optimizer estimates that the memory usage of a statement is greater than 32 MB, the statement enters the slow lane. Otherwise, the statement enters the fast lane.

.. _en-us_topic_0000002167905996__en-us_topic_0000001076579507_section5136123611463:

Viewing Real-time Query Monitoring Details
------------------------------------------

You can click a query ID to view the query details, including the basic information of query statements, real-time and historical resource consumption, SQL description, and query plan.

|image2|

.. |image1| image:: /_static/images/en-us_image_0000002168066100.png
.. |image2| image:: /_static/images/en-us_image_0000002203312625.png
