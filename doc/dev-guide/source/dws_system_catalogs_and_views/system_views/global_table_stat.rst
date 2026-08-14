:original_name: dws_04_0962.html

.. _dws_04_0962:

GLOBAL_TABLE_STAT
=================

**GLOBAL_TABLE_STAT** displays statistics about all tables (excluding foreign tables) in the current database. The values of **live_tuples** and **dead_tuples** are real-time values, and the values of other statistical columns are accumulated values since the instance was started.

.. table:: **Table 1** GLOBAL_TABLE_STAT columns

   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Column                | Type                  | Description                                                                                                                                                         |
   +=======================+=======================+=====================================================================================================================================================================+
   | schemaname            | Name                  | Namespace of a table                                                                                                                                                |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | relname               | Name                  | Table name                                                                                                                                                          |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | distribute_mode       | Char                  | Distribution mode of a table. The meaning of this column is the same as that of the **pclocatortype** column in the **pgxc_class** system catalog.                  |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | seq_scan              | Bigint                | Number of sequential scans. For a partitioned table, the sum of the number of scans of each partition is displayed.                                                 |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | seq_tuple_read        | Bigint                | Number of rows scanned in sequence.                                                                                                                                 |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | index_scan            | Bigint                | Number of index scans.                                                                                                                                              |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | index_tuple_read      | Bigint                | Number of rows scanned by the index.                                                                                                                                |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | tuple_inserted        | Bigint                | Number of rows inserted. For a replication table, the maximum value of each node is displayed. For a distribution table, the sum of all nodes is displayed.         |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | tuple_updated         | Bigint                | Number of rows updated. For a replication table, the maximum value of each node is displayed. For a distribution table, the sum of all nodes is displayed.          |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | tuple_deleted         | Bigint                | Number of rows deleted. For a replication table, the maximum value of each node is displayed. For a distribution table, the sum of all nodes is displayed.          |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | tuple_hot_updated     | Bigint                | Number of rows with HOT updates. For a replication table, the maximum value of each node is displayed. For a distribution table, the sum of all nodes is displayed. |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | live_tuples           | Bigint                | Number of live tuples. The maximum value of each node is displayed. For a distribution table, the sum of all nodes is displayed.                                    |
   |                       |                       |                                                                                                                                                                     |
   |                       |                       | This indicator applies only to row-store tables.                                                                                                                    |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | dead_tuples           | Bigint                | Number of dead tuples. The maximum value of each node is displayed. For a distribution table, the sum of all nodes is displayed.                                    |
   |                       |                       |                                                                                                                                                                     |
   |                       |                       | This indicator applies only to row-store tables.                                                                                                                    |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
