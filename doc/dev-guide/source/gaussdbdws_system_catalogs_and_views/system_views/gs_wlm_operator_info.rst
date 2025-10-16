:original_name: dws_04_0701.html

.. _dws_04_0701:

GS_WLM_OPERATOR_INFO
====================

This view displays the execution information about operators in the query statements that have been executed on the current CN. The information comes from the system catalog **dbms_om**. :ref:`gs_wlm_operator_info <dws_04_0565>`.

.. important::

   -  The schema of the **GS_WLM_OPERATOR_INFO** view is **pg_catalog**.
   -  The **GS_WLM_OPERATOR_INFO** view can be queried only in the **postgres** database. If the view is queried in other databases, an error is reported.

.. table:: **Table 1** GS_WLM_OPERATOR_INFO columns

   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | Column                | Type                     | Description                                                                                         |
   +=======================+==========================+=====================================================================================================+
   | nodename              | Text                     | Name of the CN where the statement is executed                                                      |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | queryid               | Bigint                   | Internal query_id used for statement execution                                                      |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | pid                   | Bigint                   | Backend thread ID                                                                                   |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | plan_node_id          | Integer                  | plan_node_id of the execution plan of a query                                                       |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | plan_node_name        | Text                     | Name of the operator corresponding to plan_node_id                                                  |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | start_time            | Timestamp with time zone | Time when an operator starts to process the first data record                                       |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | duration              | Bigint                   | Total execution time of an operator. The unit is ms.                                                |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | query_dop             | Integer                  | Degree of parallelism (DOP) of the current operator                                                 |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | estimated_rows        | Bigint                   | Number of rows estimated by the optimizer                                                           |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | tuple_processed       | Bigint                   | Number of elements returned by the current operator                                                 |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | min_peak_memory       | Integer                  | Minimum peak memory used by the current operator on all DNs. The unit is MB.                        |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | max_peak_memory       | Integer                  | Maximum peak memory used by the current operator on all DNs. The unit is MB.                        |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | average_peak_memory   | Integer                  | Average peak memory used by the current operator on all DNs. The unit is MB.                        |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | memory_skew_percent   | Integer                  | Memory usage skew of the current operator among DNs                                                 |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | min_spill_size        | Integer                  | Minimum spilled data among all DNs when a spill occurs. The unit is MB. The default value is **0**. |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | max_spill_size        | Integer                  | Maximum spilled data among all DNs when a spill occurs. The unit is MB. The default value is **0**. |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | average_spill_size    | Integer                  | Average spilled data among all DNs when a spill occurs. The unit is MB. The default value is **0**. |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | spill_skew_percent    | Integer                  | DN spill skew when a spill occurs                                                                   |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | min_cpu_time          | Bigint                   | Minimum execution time of the operator on all DNs. The unit is ms.                                  |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | max_cpu_time          | Bigint                   | Maximum execution time of the operator on all DNs. The unit is ms.                                  |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | total_cpu_time        | Bigint                   | Total execution time of the operator on all DNs. The unit is ms.                                    |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | cpu_skew_percent      | Integer                  | Skew of the execution time among DNs.                                                               |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | warning               | Text                     | Warning. The following warnings are displayed:                                                      |
   |                       |                          |                                                                                                     |
   |                       |                          | #. Sort/SetOp/HashAgg/HashJoin spill                                                                |
   |                       |                          | #. Spill file size large than 256MB                                                                 |
   |                       |                          | #. Broadcast size large than 100MB                                                                  |
   |                       |                          | #. Early spill                                                                                      |
   |                       |                          | #. Spill times is greater than 3                                                                    |
   |                       |                          | #. Spill on memory adaptive                                                                         |
   |                       |                          | #. Hash table conflict                                                                              |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
