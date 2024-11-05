:original_name: dws_04_0962.html

.. _dws_04_0962:

GLOBAL_TABLE_STAT
=================

**GLOBAL_TABLE_STAT** displays statistics about all tables (excluding foreign tables) in the current database. The values of **live_tuples** and **dead_tuples** are real-time values, and the values of other statistical columns are accumulated values since the instance was started.

.. table:: **Table 1** GLOBAL_TABLE_STAT columns

   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Name                  | Type                  | Description                                                                                                                                                         |
   +=======================+=======================+=====================================================================================================================================================================+
   | schemaname            | name                  | Namespace of a table                                                                                                                                                |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | relname               | name                  | Table name                                                                                                                                                          |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | distribute_mode       | char                  | Distribution mode of a table. The meaning of this column is the same as that of the **pclocatortype** column in the **pgxc_class** system catalog.                  |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | seq_scan              | bigint                | Number of sequential scans. It is counted only for row-store tables. For a partitioned table, the sum of the number of scans of each partition is displayed.        |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | seq_tuple_read        | bigint                | Number of rows scanned in sequence. It is counted only for row-store tables.                                                                                        |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | index_scan            | bigint                | Number of index scans. It is counted only for row-store tables.                                                                                                     |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | index_tuple_read      | bigint                | Number of rows scanned by the index. It is counted only for row-store tables.                                                                                       |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | tuple_inserted        | bigint                | Number of rows inserted. For a replication table, the maximum value of each node is displayed. For a distribution table, the sum of all nodes is displayed.         |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | tuple_updated         | bigint                | Number of rows updated. For a replication table, the maximum value of each node is displayed. For a distribution table, the sum of all nodes is displayed.          |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | tuple_deleted         | bigint                | Number of rows deleted. For a replication table, the maximum value of each node is displayed. For a distribution table, the sum of all nodes is displayed.          |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | tuple_hot_updated     | bigint                | Number of rows with HOT updates. For a replication table, the maximum value of each node is displayed. For a distribution table, the sum of all nodes is displayed. |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | live_tuples           | bigint                | Number of live tuples. The maximum value of each node is displayed. For a distribution table, the sum of all nodes is displayed.                                    |
   |                       |                       |                                                                                                                                                                     |
   |                       |                       | This indicator applies only to row-store tables.                                                                                                                    |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | dead_tuples           | bigint                | Number of dead tuples. The maximum value of each node is displayed. For a distribution table, the sum of all nodes is displayed.                                    |
   |                       |                       |                                                                                                                                                                     |
   |                       |                       | This indicator applies only to row-store tables.                                                                                                                    |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
