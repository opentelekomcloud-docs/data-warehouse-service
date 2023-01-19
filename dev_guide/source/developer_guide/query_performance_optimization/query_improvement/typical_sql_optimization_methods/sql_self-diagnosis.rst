:original_name: dws_04_0446.html

.. _dws_04_0446:

SQL Self-Diagnosis
==================

Performance issues may occur when you query data or run the **INSERT**, **DELETE**, **UPDATE**, or **CREATE TABLE AS** statement. You can query the **warning** column in the :ref:`GS_WLM_SESSION_STATISTICS <dws_04_0706>`, :ref:`GS_WLM_SESSION_HISTORY <dws_04_0705>`, and :ref:`GS_WLM_SESSION_INFO <dws_04_0566>` views to obtain performance diagnosis information for tuning.

Alarms that can trigger SQL self-diagnosis depend on the settings of :ref:`resource_track_level <en-us_topic_0000001145694507__section153571329142612>`. If **resource_track_level** is set to **query** or **perf**, you can diagnose alarms indicating that statistics of multiple columns or a single column are not collected or SQL statements are not pushed down. If **resource_track_level** is set to **operator**, all alarm scenarios can be diagnosed.

Whether a SQL plan will be diagnosed depends on the settings of :ref:`resource_track_cost <en-us_topic_0000001145694507__section1089022732713>`. A SQL plan will be diagnosed only if its execution cost is greater than **resource_track_cost**. You can use the **EXPLAIN** keyword to check the plan execution cost.

Alarms
------

Currently, the following performance alarms will be reported:

-  Some column statistics are not collected.

   If statistics of a single column or multiple columns are not collected, an alarm is reported. For details about the optimization, see :ref:`Updating Statistics <dws_04_0436>` and :ref:`Optimizing Statistics <dws_04_0449>`.

   If no statistics are collected for the OBS foreign table and HDFS foreign table in the query statement, an alarm indicating that statistics are not collected will be reported. Because the **ANALYZE** performance of the OBS foreign table and HDFS foreign table is poor, you are not advised to perform **ANALYZE** on these tables. Instead, you are advised to use the **ALTER FOREIGN TABLE** syntax to modify the **totalrows** attribute of the foreign table to correct the estimated number of rows.

   Example alarms:

   No statistics about a table are not collected.

   .. code-block::

      Statistic Not Collect:
          schema_test.t1

   The statistics about a single column are not collected.

   .. code-block::

      Statistic Not Collect:
          schema_test.t2(c1)

   The statistics about multiple columns are not collected.

   .. code-block::

      Statistic Not Collect:
          schema_test.t3((c1,c2))

   The statistics about a single column and multiple columns are not collected.

   .. code-block::

      Statistic Not Collect:
          schema_test.t4(c1)    schema_test.t5((c1,c2))

-  SQL statements are not pushed down.

   The cause details are displayed in the alarms. For details about the optimization, see :ref:`Optimizing Statement Pushdown <dws_04_0447>`.

   -  If the pushdown failure is caused by functions, the function names are displayed in the alarm.
   -  If the pushdown failure is caused by syntax, the alarm indicates that the syntax does not support pushdown. For example, the syntax containing the **With Recursive**, **Distinct On**, or **Row** expression, and syntax whose return value is of record type do not support pushdown.

   Example alarms:

   .. code-block::

      SQL is not plan-shipping, reason : ""enable_stream_operator" is off"
      SQL is not plan-shipping, reason : ""Distinct On" can not be shipped"
      SQL is not plan-shipping, reason : ""v_test_unshipping_log" is VIEW that will be treated as Record type can't be shipped"

-  In a hash join, the larger table is used as the inner table.

   An alarm will be reported if the number of rows in the inner table reaches or exceeds 10 times of that in the foreign table, more than 100,000 inner-table rows are processed on each DN in average, and data has been flushed to disks. You can view the **query_plan** column in :ref:`GS_WLM_SESSION_HISTORY <dws_04_0705>` to check whether hash joins are used. In this scenario, you need to adjust the sequence of the HashJoin internal and foreign tables. For details, see :ref:`Join Order Hints <dws_04_0456>`.

   Example alarm:

   .. code-block::

      PlanNode[7] Large Table is INNER in HashJoin "Vector Hash Aggregate"

   In the preceding command, **7** indicates the operator whose ID is **7** in the **query_plan** column.

-  **nestloop** is used in a large-table equivalent join.

   An alarm will be reported if nested loop is used in an equivalent join where more than 100,000 larger-table rows are processed on each DN in average. You can view the **query_plan** column of :ref:`GS_WLM_SESSION_HISTORY <dws_04_0705>` to check whether nested loop is used. In this scenario, you need to adjust the table join mode and disable the NestLoop join mode between the current internal and foreign tables. For details, see :ref:`Join Operation Hints <dws_04_0457>`.

   Example alarm:

   .. code-block::

      PlanNode[5] Large Table with Equal-Condition use Nestloop"Nested Loop"

-  A large table is broadcasted.

   An alarm will be reported if more than 100 thousand of rows are broadcasted on each DN in average. In this scenario, the broadcast operation of the BroadCast lower-layer operator needs to be disabled. For details about the optimization, see :ref:`Stream Operation Hints <dws_04_0459>`.

   Example alarm:

   .. code-block::

      PlanNode[5] Large Table in Broadcast "Streaming(type: BROADCAST dop: 1/2)"

-  Data skew occurs.

   An alarm will be reported if the number of rows processed on any DN exceeds 100 thousand, and the number of rows processed on a DN reaches or exceeds 10 times of that processed on another DN. Generally, this alarm is generated due to storage layer skew or computing layer skew. For details about the optimization, see :ref:`Optimizing Data Skew <dws_04_0451>`.

   Example alarm:

   .. code-block::

      PlanNode[6] DataSkew:"Seq Scan", min_dn_tuples:0, max_dn_tuples:524288

-  The index is improper.

   During base table scanning, an alarm is reported if the following conditions are met:

   -  For row-store tables:

      -  When the index scanning is used, the ratio of the number of output lines to the number of scanned lines is greater than 1/1000 and the number of output lines is greater than 10,000.
      -  When sequential scanning is used, the number of output lines to the number of scanned lines is less than 1/1000, the number of output lines is less than or equal to 10,000, and the number of scanned lines is greater than 10,000.

   -  For column-store tables:

      -  When the index scanning is used, the ratio of the number of output lines to the number of scanned lines is greater than 1/10000 and the number of output lines is greater than 100.
      -  When sequential scanning is used, the number of output lines to the number of scanned lines is less than 1/10,000, the number of output lines is less than or equal to 100, and the number of scanned lines is greater than 10,000.

   For details about the optimization, see :ref:`Optimizing Operators <dws_04_0450>`. You can also refer to :ref:`Case: Creating an Appropriate Index <dws_04_0476>` and :ref:`Case: Setting Partial Cluster Keys <dws_04_0490>`.

   Example alarms:

   .. code-block::

      PlanNode[4] Indexscan is not properly used:"Index Only Scan", output:524288, filtered:0, rate:1.00000
      PlanNode[5] Indexscan is ought to be used:"Seq Scan", output:1, filtered:524288, rate:0.00000

-  Estimation is inaccurate.

   An alarm will be reported if the maximum number or the estimated maximum number of rows processed on a DN is over 100,000, and the larger number reaches or exceeds 10 times of the smaller one. In this scenario, you can refer to :ref:`Rows Hints <dws_04_0458>` to correct the estimation on the number of rows, so that the optimizer can re-design the execution plan based on the correct number.

   Example alarm:

   .. code-block::

      PlanNode[5] Inaccurate Estimation-Rows: "Hash Join" A-Rows:0, E-Rows:52488

Restrictions
------------

#. An alarm contains a maximum of 2048 characters. If the length of an alarm exceeds this value (for example, a large number of long table names and column names are displayed in the alarm when their statistics are not collected), a warning instead of an alarm will be reported.

   .. code-block::

      WARNING, "Planner issue report is truncated, the rest of planner issues will be skipped"

#. If a query statement contains the **Limit** operator, alarms of operators lower than **Limit** will not be reported.

#. For alarms about data skew and inaccurate estimation, only alarms on the lower-layer nodes in a plan tree will be reported. This is because the same alarms on the upper-level nodes may be triggered by problems on the lower-layer nodes. For example, if data skew occurs on the **Scan** node, data skew may also occur in operators (for example, **Hashagg**) at the upper layer.
