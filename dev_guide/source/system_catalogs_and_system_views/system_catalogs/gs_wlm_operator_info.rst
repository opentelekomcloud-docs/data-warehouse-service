:original_name: dws_04_0565.html

.. _dws_04_0565:

GS_WLM_OPERATOR_INFO
====================

**GS_WLM_OPERATOR_INFO** records operators of completed jobs. The data is dumped from the kernel to a system catalog. If the GUC parameter **enable_resource_record** is set to **on**, the system imports records in :ref:`GS_WLM_OPERATOR_HISTORY <dws_04_0702>` to this system catalog every three minutes. You are not advised to enable this function because it occupies storage space and affects performance.

.. note::

   -  This system catalog's schema is **dbms_om**.
   -  The **pg_catalog** has the **GS_WLM_OPERATOR_INFO** view.

.. _en-us_topic_0000001188642146__ta2c1b3b6469742cd9a313f0843410206:

.. table:: **Table 1** GS_WLM_OPERATOR_INFO columns

   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | Name                  | Type                     | Description                                                                                         |
   +=======================+==========================+=====================================================================================================+
   | nodename              | text                     | Name of the CN where the statement is executed                                                      |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | queryid               | bigint                   | Internal query_id used for statement execution                                                      |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | pid                   | bigint                   | Thread ID of the backend                                                                            |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | plan_node_id          | integer                  | plan_node_id of the execution plan of a query                                                       |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | plan_node_name        | text                     | Name of the operator corresponding to plan_node_id                                                  |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | start_time            | timestamp with time zone | Time when an operator starts to process the first data record                                       |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | duration              | bigint                   | Total execution time of an operator. The unit is ms.                                                |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | query_dop             | integer                  | Degree of parallelism (DOP) of the current operator                                                 |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | estimated_rows        | bigint                   | Number of rows estimated by the optimizer                                                           |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | tuple_processed       | bigint                   | Number of elements returned by the current operator                                                 |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | min_peak_memory       | integer                  | Minimum peak memory used by the current operator on all DNs. The unit is MB.                        |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | max_peak_memory       | integer                  | Maximum peak memory used by the current operator on all DNs. The unit is MB.                        |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | average_peak_memory   | integer                  | Average peak memory used by the current operator on all DNs. The unit is MB.                        |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | memory_skew_percent   | integer                  | Memory usage skew of the current operator among DNs                                                 |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | min_spill_size        | integer                  | Minimum spilled data among all DNs when a spill occurs. The unit is MB. The default value is **0**. |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | max_spill_size        | integer                  | Maximum spilled data among all DNs when a spill occurs. The unit is MB. The default value is **0**. |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | average_spill_size    | integer                  | Average spilled data among all DNs when a spill occurs. The unit is MB. The default value is **0**. |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | spill_skew_percent    | integer                  | DN spill skew when a spill occurs                                                                   |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | min_cpu_time          | bigint                   | Minimum execution time of the operator on all DNs. The unit is ms.                                  |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | max_cpu_time          | bigint                   | Maximum execution time of the operator on all DNs. The unit is ms.                                  |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | total_cpu_time        | bigint                   | Total execution time of the operator on all DNs. The unit is ms.                                    |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | cpu_skew_percent      | integer                  | Skew of the execution time among DNs.                                                               |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
   | warning               | text                     | Warning. The following warnings are displayed:                                                      |
   |                       |                          |                                                                                                     |
   |                       |                          | #. Sort/SetOp/HashAgg/HashJoin spill                                                                |
   |                       |                          | #. Spill file size large than 256MB                                                                 |
   |                       |                          | #. Broadcast size large than 100MB                                                                  |
   |                       |                          | #. Early spill                                                                                      |
   |                       |                          | #. Spill times is greater than 3                                                                    |
   |                       |                          | #. Spill on memory adaptive                                                                         |
   |                       |                          | #. Hash table conflict                                                                              |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------+
