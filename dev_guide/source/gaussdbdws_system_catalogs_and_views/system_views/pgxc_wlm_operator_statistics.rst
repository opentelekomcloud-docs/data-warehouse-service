:original_name: dws_04_0838.html

.. _dws_04_0838:

PGXC_WLM_OPERATOR_STATISTICS
============================

**PGXC_WLM_OPERATOR_STATISTICS** displays the operator information of jobs being executed on CNs. The system administrator can query job operator information of all users in the cluster, while common users can query only their own job operator information.

:ref:`Table 1 <en-us_topic_0000001811609837__tf1a716e87ca6430baa99c3fd40179ec4>` lists the columns in the **PGXC_WLM_OPERATOR_STATISTICS** view.

.. table:: **Table 1** GS_WLM_OPERATOR_STATISTICS columns

   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Column                | Type                     | Description                                                                                                                                                           |
   +=======================+==========================+=======================================================================================================================================================================+
   | queryid               | Bigint                   | Internal query_id used for statement execution                                                                                                                        |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | pid                   | Bigint                   | ID of the backend thread                                                                                                                                              |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | plan_node_id          | Integer                  | plan_node_id of the execution plan of a query                                                                                                                         |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | plan_node_name        | Text                     | Name of the operator corresponding to **plan_node_id**. The maximum length of the operator name is 127 characters (excluding format characters such as spaces).       |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | start_time            | Timestamp with time zone | Time when the operator starts to be executed for the first time.                                                                                                      |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | duration              | Bigint                   | Total execution time of the operator from the start to the end, in milliseconds.                                                                                      |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | status                | Text                     | Execution status of the current operator. The value can be **waiting**, **running**, or **finished**.                                                                 |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | query_dop             | Integer                  | DOP of the current operator                                                                                                                                           |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | estimated_rows        | Bigint                   | Number of rows estimated by the optimizer. If the number of returned estimated rows exceeds int64_max, int64_max is displayed.                                        |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | tuple_processed       | Bigint                   | Total number of elements returned by the current operator on all DNs. If the estimated number of returned rows exceeds int64_max, int64_max is displayed.             |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | min_peak_memory       | Integer                  | Minimum peak memory used by the current operator on all DNs. The unit is MB.                                                                                          |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | max_peak_memory       | Integer                  | Maximum peak memory used by the current operator on all DNs. The unit is MB.                                                                                          |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | average_peak_memory   | Integer                  | Average peak memory used by the current operator on all DNs. The unit is MB.                                                                                          |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | memory_skew_percent   | Integer                  | Memory usage skew of the current operator among DNs                                                                                                                   |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | min_spill_size        | Integer                  | Minimum logical spilled data among all DNs when a spill occurs, in MB. The default value is **0**.                                                                    |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | max_spill_size        | Integer                  | Maximum logical spilled data among all DNs when a spill occurs, in MB. The default value is **0**.                                                                    |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | average_spill_size    | Integer                  | Average logical spilled data among all DNs when a spill occurs, in MB. The default value is **0**.                                                                    |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | spill_skew_percent    | Integer                  | DN spill skew when a spill occurs                                                                                                                                     |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | min_cpu_time          | Bigint                   | Minimum execution time of the operator on all DNs. The unit is ms.                                                                                                    |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | max_cpu_time          | Bigint                   | Maximum execution time of the operator on all DNs. The unit is ms.                                                                                                    |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | total_cpu_time        | Bigint                   | Total execution time of the operator on all DNs. The unit is ms.                                                                                                      |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | cpu_skew_percent      | Integer                  | Skew of the execution time among DNs.                                                                                                                                 |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | warning               | Text                     | Warning. The following warnings are displayed:                                                                                                                        |
   |                       |                          |                                                                                                                                                                       |
   |                       |                          | #. Sort/SetOp/HashAgg/HashJoin spill                                                                                                                                  |
   |                       |                          | #. Spill file size large than 256MB                                                                                                                                   |
   |                       |                          | #. Broadcast size large than 100MB                                                                                                                                    |
   |                       |                          | #. Early spill                                                                                                                                                        |
   |                       |                          | #. Spill times is greater than 3                                                                                                                                      |
   |                       |                          | #. Spill on memory adaptive                                                                                                                                           |
   |                       |                          | #. Hash table conflict                                                                                                                                                |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | parent_id             | Integer                  | Parent node ID of the operator node.                                                                                                                                  |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | exec_count            | Integer                  | Maximum number of times that the operator node can be executed on all DNs.                                                                                            |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | progress              | Text                     | Progress information of the operator. For the first operator, it is the overall progress of the job. For other operators, it is the progress of the current operator. |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | min_net_size          | Bigint                   | Minimum network communication data volume (KB) of the operator on all DNs. It mainly applies to network operators.                                                    |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | max_net_size          | Bigint                   | Maximum network communication data volume (KB) of the operator on all DNs. It mainly applies to network operators.                                                    |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | total_net_size        | Bigint                   | Total network communication data volume (KB) of the operator on all DNs. It mainly applies to network operators.                                                      |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | min_read_bytes        | Bigint                   | Minimum amount of data read by the operator from disks on all DNs. The unit is KB.                                                                                    |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | max_read_bytes        | Bigint                   | Maximum amount of data read by the operator from disks on all DNs. The unit is KB.                                                                                    |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | total_read_bytes      | Bigint                   | Total amount of data read by the operator from disks on all DNs, in KB.                                                                                               |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | min_write_bytes       | Bigint                   | Minimum amount of data written by the operator to disks on all DNs. The unit is KB.                                                                                   |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | max_write_bytes       | Bigint                   | Maximum amount of data written by the operator to disks on all DNs. The unit is KB.                                                                                   |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | total_write_bytes     | Bigint                   | Total amount of data written by the operator to disks on all DNs, in KB.                                                                                              |
   +-----------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
