:original_name: dws_04_0410.html

.. _dws_04_0410:

.. _en-us_topic_0000001147351103:

Overview of the SQL Execution Plan
==================================

The SQL execution plan is a node tree, which displays detailed procedure when GaussDB(DWS) runs an SQL statement. A database operator indicates one step.

You can run the **EXPLAIN** command to view the execution plan generated for each query by an optimizer. Explain outputs a row of information for each execution node, showing the basic node type and the expense estimate that the optimizer makes for executing the node. See :ref:`Figure 1 <en-us_topic_0000001147351103__en-us_topic_0000001098974940_fea51e22e1a004029882c2b264b9075e4>`.

.. _en-us_topic_0000001147351103__en-us_topic_0000001098974940_fea51e22e1a004029882c2b264b9075e4:

.. figure:: /_static/images/en-us_image_0000001145695169.jpg
   :alt: **Figure 1** SQL execution plan example

   **Figure 1** SQL execution plan example

-  Nodes at the bottom level are scan nodes. They scan tables and return raw rows. The types of scan nodes (sequential scans and index scans) vary depending on the table access methods. Objects scanned by the bottom layer nodes may not be row-store data (not directly read from a table), such as VALUES clauses and functions that return rows, which have their own types of scan nodes.
-  If the query requires join, aggregation, sorting, or other operations on the raw rows, there will be other nodes above the scan nodes to perform these operations. In addition, there is more than one possible way to perform these operations, so different execution node types may be displayed here.
-  The first row (the upper-layer node) estimates the total execution cost of the execution plan. This value indicates the value that the optimizer tries to minimize.

Execution Plan Display Format
-----------------------------

GaussDB(DWS) provides four display formats: **normal**, **pretty**, **summary**, and **run**.

-  **normal** indicates that the default printing format is used. :ref:`Figure 1 <en-us_topic_0000001147351103__en-us_topic_0000001098974940_fea51e22e1a004029882c2b264b9075e4>` shows the display format.
-  **pretty** indicates that the optimized display mode of GaussDB(DWS) is used. A new format contains a plan node ID, directly and effectively analyzing performance. as shown in :ref:`Figure 2 <en-us_topic_0000001147351103__en-us_topic_0000001098974940_f38ec674913084bccbe6bad67c4bc8fee>`.
-  **summary** indicates that the analysis result based on such information is printed in addition to the printed information in the format specified by **pretty**.
-  **run** indicates that in addition to the printed information specified by **summary**, the database exports the information as a CSV file.

.. _en-us_topic_0000001147351103__en-us_topic_0000001098974940_f38ec674913084bccbe6bad67c4bc8fee:

.. figure:: /_static/images/en-us_image_0000001145815093.png
   :alt: **Figure 2** Example of an execution plan using the pretty format

   **Figure 2** Example of an execution plan using the pretty format

You can change the display format of execution plans by setting **explain_perf_mode**. Later examples use the pretty format by default.

Execution Plan Information
--------------------------

In addition to setting different display formats for an execution plan, you can use different **EXPLAIN** syntax to display execution plan information in details. The following lists the common **EXPLAIN** syntax. For details, see EXPLAIN.

-  EXPLAIN *statement*: only generates an execution plan and does not execute. The *statement* indicates SQL statements.
-  EXPLAIN ANALYZE *statement*: generates and executes an execution plan, and displays the execution summary. Then actual execution time statistics are added to the display, including the total elapsed time expended within each plan node (in milliseconds) and the total number of rows it actually returned.
-  EXPLAIN PERFORMANCE *statement*: generates and executes the execution plan, and displays all execution information.

To measure the run time cost of each node in the execution plan, the current execution of **EXPLAIN ANALYZE** or **EXPLAIN PERFORMANCE** adds profiling overhead to query execution. Running **EXPLAIN ANALYZE** or **PERFORMANCE** on a query sometimes takes longer time than executing the query normally. The amount of overhead depends on the nature of the query, as well as the platform being used.

Therefore, if an SQL statement is not finished after being running for a long time, run the **EXPLAIN** statement to view the execution plan and then locate the fault. If the SQL statement has been properly executed, run the **EXPLAIN ANALYZE** or **EXPLAIN PERFORMANCE** statement to check the execution plan and information to locate the fault.

The **EXPLAIN PERFORMANCE** lightweight execution is consistent with **EXPLAIN PERFORMANCE** but greatly reduces the time spent on performance analysis.
