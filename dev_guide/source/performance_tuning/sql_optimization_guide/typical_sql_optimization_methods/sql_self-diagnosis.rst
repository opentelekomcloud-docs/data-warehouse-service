:original_name: dws_04_0446.html

.. _dws_04_0446:

SQL Self-Diagnosis
==================

Performance problems may occur when you run the **INSERT/UPDATE/DELETE/SELECT/MERGE INTO** or **CREATE TABLE AS** statement. The product integrates the automatic performance diagnosis function and saves related diagnosis information to the . When **enable_resource_track** is set to **on**, the diagnosis information is dumped to the . You can query the warning field in the views :ref:`GS_WLM_SESSION_STATISTICS <dws_04_0706>`, :ref:`GS_WLM_SESSION_HISTORY <dws_04_0705>`, :ref:`GS_WLM_SESSION_INFO <dws_04_0704>` in *Developer Guide* to obtain the corresponding performance diagnosis information for performance optimization.

Alarms that can trigger SQL self-diagnosis depend on the settings of :ref:`resource_track_level <en-us_topic_0000001510522653__section153571329142612>`. When **resource_track_level** is set to **query**, you can diagnose alarms such as uncollected multi-column/single-column statistics, partitions not pruned, and failure of pushing down SQL statements. When **resource_track_level** is set to **perf** or **operator**, all alarms can be diagnosed.

Whether a SQL plan will be diagnosed depends on the settings of :ref:`resource_track_cost <en-us_topic_0000001510522653__section1089022732713>`. A SQL plan will be diagnosed only if its execution cost is greater than **resource_track_cost**. You can use the **EXPLAIN** keyword to check the plan execution cost.

When **EXPLAIN PERFORMANCE** or **EXPLAIN VERBOSE** is executed, SQL self-diagnosis information, except that without multi-column statistics, will be generated. For details, see :ref:`SQL Execution Plan <dws_04_0432>`.

Alarms
------

Currently, the following performance alarms will be reported:

-  Statistics of a single column or multiple columns are not collected.

   If statistics of a single column or multiple columns are not collected, an alarm is reported. To handle this alarm, you are advised to perform **ANALYZE** on related tables. For details, see :ref:`Updating Statistics <dws_04_0436>` and :ref:`Optimizing Statistics <dws_04_0449>`.

   If no statistics are collected for the OBS foreign table and HDFS foreign table in the query statement, an alarm indicating that statistics are not collected will be reported. Because the **ANALYZE** performance of the OBS foreign table and HDFS foreign table is poor, you are not advised to perform **ANALYZE** on these tables. Instead, you are advised to use the **ALTER FOREIGN TABLE** syntax to modify the **totalrows** attribute of the foreign table to correct the estimated number of rows.

   Example alarms:

   The statistics about a table are not collected.

   .. code-block::

      Statistic Not Collect
              schema_test.t1

   The statistics about a single column are not collected.

   .. code-block::

      Statistic Not Collect
              schema_test.t2(c1)

   The statistics about multiple columns are not collected.

   .. code-block::

      Statistic Not Collect
              schema_test.t3((c1,c2))

   The statistics about a single column and multiple columns are not collected.

   .. code-block::

      Statistic Not Collect
              schema_test.t4(c1)
              schema_test.t5((c1,c2))

-  Partitions are not pruned (supported by 8.1.2 and later versions).

   When a partitioned table is queried, the partition is pruned based on the constraints on the partition key to improve the query performance. However, the partition table may not be pruned due to improper constraints, deteriorating the query performance. For details, see :ref:`Case: Rewriting SQL Statements and Eliminating Prune Interference <dws_04_0488>`.

-  SQL statements are not pushed down.

   The cause details are displayed in the alarms. For details, see :ref:`Optimizing Statement Pushdown <dws_04_0447>`.

   The potential causes for the pushdown failure are as follows:

   -  Caused by functions

      The function name is displayed in the diagnosis information. Function pushdown is determined by the **shippable** attribute of the function. For details, see the **CREATE FUNCTION** syntax.

   -  Caused by syntax

      The diagnosis information displays the syntax that causes the pushdown failure. For example, if the statement contains the **With Recursive**, **Distinct On**, or **row** expression and the return value is of the record type, an alarm is reported, indicating that the syntax does not support pushdown.

   Example alarms:

   .. code-block::

      SQL is not plan-shipping
              "enable_stream_operator" is off

      SQL is not plan-shipping
              "Distinct On" can not be shipped

      SQL is not plan-shipping
              "v_test_unshipping_log" is VIEW that will be treated as Record type can't be shipped

-  Vectorized plans are not supported.

   For SQL statements that cannot use vectorized plans, report the detailed reasons why vectorized plans cannot be used.

   Common reasons are as follows:

   -  The target column contains functions whose return type is set.
   -  The target column or query condition, the distribution key of the Stream operator, and the **Limit** and **Offset** clauses contain expressions that cannot be vectorized (such as geospatial types, array expressions, Row expressions, XML expressions, and functions whose parameters or return values contain the refcursor type).
   -  The **Group By** clause contains an array equivalent judgment expression.
   -  **GC_FDW** and **LOG_FDW** do not support vectorization.
   -  The plan contains operators such as Cte Scan, Recursive Union, Merge Append and Lock Rows.

   Example alarms:

   .. code-block::

      SQL is un-vectorized
               Function regexp_split_to_table that returns set is un-vectorized

      SQL is un-vectorized
               Array expression is un-vectorized

      SQL is un-vectorized
               Function array_agg is un-vectorized

      SQL is un-vectorized
               RecursiveUnion is un-vectorized

-  In a hash join, the larger table is used as the inner table.

   An alarm will be reported if the number of rows in the inner table reaches or exceeds 10 times of that in the foreign table, more than 100,000 inner-table rows are processed on each DN in average, and data has been flushed to disks. You can view the **query_plan** column in :ref:`GS_WLM_SESSION_HISTORY <dws_04_0705>` to check whether hash joins are used. In this scenario, you need to adjust the sequence of the HashJoin internal and foreign tables. For details, see :ref:`Join Order Hints <dws_04_0456>`.

   Example alarm:

   .. code-block::

      Execute diagnostic information
      PlanNode[7] Large Table is INNER in HashJoin "Vector Hash Aggregate"

   In the preceding command, **7** indicates the operator whose ID is **7** in the **query_plan** column.

-  **nestloop** is used in a large-table equivalent join.

   An alarm will be reported if nested loop is used in an equivalent join where more than 100,000 larger-table rows are processed on each DN in average. You can view the **query_plan** column of :ref:`GS_WLM_SESSION_HISTORY <dws_04_0705>` to check whether nested loop is used. In this scenario, you need to adjust the table join mode and disable the NestLoop join mode between the current internal and foreign tables. For details, see :ref:`Join Operation Hints <dws_04_0457>`.

   Example alarm:

   .. code-block::

      Execute diagnostic information
              PlanNode[5] Large Table with Equal-Condition use Nestloop"Nested Loop"

-  A large table is broadcasted.

   An alarm will be reported if more than 100 thousand of rows are broadcasted on each DN in average. In this scenario, the broadcast operation of the BroadCast lower-layer operator needs to be disabled. For details, see :ref:`Stream Operation Hints <dws_04_0459>`.

   Example alarm:

   .. code-block::

      Execute diagnostic information
              PlanNode[5] Large Table in Broadcast "Streaming(type: BROADCAST dop: 1/2)"

-  Data skew occurs.

   An alarm will be reported if the number of rows processed on any DN exceeds 100 thousand, and the number of rows processed on a DN reaches or exceeds 10 times of that processed on another DN. Generally, this alarm is generated due to storage layer skew or computing layer skew. For details, see :ref:`Optimizing Data Skew <dws_04_0451>`.

   Example alarm:

   .. code-block::

      Execute diagnostic information
             PlanNode[6] DataSkew:"Seq Scan", min_dn_tuples:0, max_dn_tuples:524288

-  The index is improper.

   During base table scanning, an alarm is reported if the following conditions are met:

   -  For row-store tables:

      -  When the index scanning is used, the ratio of the number of output lines to the number of scanned lines is greater than 1/1000 and the number of output lines is greater than 10,000.
      -  When sequential scanning is used, the number of output lines to the number of scanned lines is less than 1/1000, the number of output lines is less than or equal to 10,000, and the number of scanned lines is greater than 10,000.

   -  For column-store tables:

      -  When the index scanning is used, the ratio of the number of output lines to the number of scanned lines is greater than 1/10000 and the number of output lines is greater than 100.
      -  When sequential scanning is used, the number of output lines to the number of scanned lines is less than 1/10,000, the number of output lines is less than or equal to 100, and the number of scanned lines is greater than 10,000.

   For details, see :ref:`Optimizing Operators <dws_04_0450>`. You can also refer to :ref:`Case: Creating an Appropriate Index <dws_04_0476>` and :ref:`Case: Setting Partial Cluster Keys <dws_04_0490>`.

   Example alarms:

   .. code-block::

      Execute diagnostic information
              PlanNode[4] Indexscan is not properly used:"Index Only Scan", output:524288, filtered:0, rate:1.00000
              PlanNode[5] Indexscan is ought to be used:"Seq Scan", output:1, filtered:524288, rate:0.00000

   The diagnosis result is only a suggestion for the current SQL statement. You are advised to create an index only for frequently used filter criteria.

-  Estimation is inaccurate.

   An alarm will be reported if the maximum number or the estimated maximum number of rows processed on a DN is over 100,000, and the larger number reaches or exceeds 10 times of the smaller one. In this scenario, you can refer to :ref:`Rows Hints <dws_04_0458>` to correct the estimation on the number of rows, so that the optimizer can re-design the execution plan based on the correct number.

   Example alarm:

   .. code-block::

      Execute diagnostic information
              PlanNode[5] Inaccurate Estimation-Rows: "Hash Join" A-Rows:0, E-Rows:52488

Restrictions
------------

#. An alarm contains a maximum of 2048 characters. If the length of an alarm exceeds this value (for example, a large number of long table names and column names are displayed in the alarm when their statistics are not collected), a warning instead of an alarm will be reported.

   .. code-block::

      WARNING, "Planner issue report is truncated, the rest of planner issues will be skipped"

#. If a query statement contains the **Limit** operator, alarms of operators lower than **Limit** will not be reported.

#. For alarms about data skew and inaccurate estimation, only alarms on the lower-layer nodes in a plan tree will be reported. This is because the same alarms on the upper-level nodes may be triggered by problems on the lower-layer nodes. For example, if data skew occurs on the **Scan** node, data skew may also occur in operators (for example, **Hashagg**) at the upper layer.
