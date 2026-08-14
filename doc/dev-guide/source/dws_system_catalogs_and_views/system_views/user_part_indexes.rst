:original_name: dws_04_0868.html

.. _dws_04_0868:

USER_PART_INDEXES
=================

**USER_PART_INDEXES** displays information about partitioned table indexes accessible to the current user.

+------------------------+-----------------------+----------------------------------------------------------------------------+
| Column                 | Type                  | Description                                                                |
+========================+=======================+============================================================================+
| index_owner            | character varying(64) | Name of the owner of the partitioned table index                           |
+------------------------+-----------------------+----------------------------------------------------------------------------+
| schema                 | character varying(64) | Schema of the partitioned table index                                      |
+------------------------+-----------------------+----------------------------------------------------------------------------+
| index_name             | character varying(64) | Name of the partitioned table index                                        |
+------------------------+-----------------------+----------------------------------------------------------------------------+
| table_name             | character varying(64) | Name of the partitioned table to which the partitioned table index belongs |
+------------------------+-----------------------+----------------------------------------------------------------------------+
| partitioning_type      | Text                  | Partition policy of the partitioned table                                  |
|                        |                       |                                                                            |
|                        |                       | .. note::                                                                  |
|                        |                       |                                                                            |
|                        |                       |    Currently, only range partitioning and list partitioning are supported. |
+------------------------+-----------------------+----------------------------------------------------------------------------+
| partition_count        | Bigint                | Number of index partitions of the partitioned table index                  |
+------------------------+-----------------------+----------------------------------------------------------------------------+
| def_tablespace_name    | Name                  | Tablespace name of the partitioned table index                             |
+------------------------+-----------------------+----------------------------------------------------------------------------+
| partitioning_key_count | Integer               | Number of partition keys of the partitioned table                          |
+------------------------+-----------------------+----------------------------------------------------------------------------+
