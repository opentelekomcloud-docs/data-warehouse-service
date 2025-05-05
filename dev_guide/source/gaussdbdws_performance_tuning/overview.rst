:original_name: dws_04_0401.html

.. _dws_04_0401:

Overview
========

Database performance tuning is the process of optimizing database system configuration and SQL queries to improve database performance and efficiency. The purpose includes eliminating performance bottlenecks, reducing response times, increasing throughput and resource utilization, cutting costs, and improving system stability.

This section provides comprehensive guidance for DBAs on performance diagnosis, system tuning, and SQL tuning, as well as practical examples of SQL tuning.

Precautions
-----------

-  Database performance tuning is a complex and intricate process. To achieve the optimal performance and efficiency, performance tuning must take into consideration multiple factors, such as hardware, software, queries, configuration, and data structures. Engineers performing the performance tuning must be familiar with how database systems work in great detail, including a deep understanding of the system software architecture, software and hardware configurations, database configuration parameters, concurrency control, query handling, and database applications.
-  Performance tuning sometimes requires a cluster restart, which may interrupt services. To avoid that, you are advised to schedule performance tuning tasks that require a cluster restart to occur during off-peak hours.

Performance Tuning Process
--------------------------

:ref:`Figure 1 <en-us_topic_0000002088892813__en-us_topic_0000001233883279_f64f52b18636244eb93a7de59c93fd086>` illustrates the performance tuning process.

.. _en-us_topic_0000002088892813__en-us_topic_0000001233883279_f64f52b18636244eb93a7de59c93fd086:

.. figure:: /_static/images/en-us_image_0000001188482356.png
   :alt: **Figure 1** GaussDB(DWS) performance tuning

   **Figure 1** GaussDB(DWS) performance tuning

:ref:`Table 1 <en-us_topic_0000002088892813__en-us_topic_0000001233883279_tc9d84325e32a4d07a7f18e42cea41baa>` gives a brief introduction to each phase of the performance tuning process.

.. _en-us_topic_0000002088892813__en-us_topic_0000001233883279_tc9d84325e32a4d07a7f18e42cea41baa:

.. table:: **Table 1** Phase-by-phase introduction to GaussDB(DWS) performance tuning

   +-------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Phase                                                       | Description                                                                                                                                                                                                                                                                                                              |
   +=============================================================+==========================================================================================================================================================================================================================================================================================================================+
   | :ref:`Performance Diagnosis <en-us_topic_0000002052655422>` | Obtain the CPU, memory, I/O, and network resource usage of each node to check whether these resources are fully utilized and whether any performance bottlenecks exist.                                                                                                                                                  |
   +-------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`System Optimization <en-us_topic_0000002088892829>`   | Perform OS and database system-level performance tuning to achieve better utilization of existing CPU, memory, I/O, and network resources, prevent resource conflicts, and improve query throughput.                                                                                                                     |
   +-------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`SQL Tuning <en-us_topic_0000002052655446>`            | Analyze the SQL statements used and determine whether any optimization can be performed. Analysis of SQL statements comprises:                                                                                                                                                                                           |
   |                                                             |                                                                                                                                                                                                                                                                                                                          |
   |                                                             | -  Generating table statistics using **ANALYZE**: The **ANALYZE** statement collects statistics about the database table content. Statistical results are stored in the system catalog **PG_STATISTIC**. The execution plan generator uses these statistics to determine which one is the most effective execution plan. |
   |                                                             | -  Analyzing the execution plan: The **EXPLAIN** statement displays the execution plan of SQL statements, and the **EXPLAIN PERFORMANCE** statement displays the execution time of each operator in SQL statements.                                                                                                      |
   |                                                             | -  Identifying the root causes of issues: Identify possible causes by analyzing the execution plan and perform specific optimization by modifying database-level SQL optimization parameters.                                                                                                                            |
   |                                                             | -  Compiling better SQL statements: Compile better SQL statements in the scenarios, such as cache of intermediate and temporary data for complex queries, result set cache, and result set combination.                                                                                                                  |
   +-------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
