:original_name: dws_04_0668.html

.. _dws_04_0668:

DBA_PART_INDEXES
================

**DBA_PART_INDEXES** displays information about all partitioned table indexes in the database. It is accessible only to users with system administrator rights.

+------------------------+-----------------------+----------------------------------------------------------------------------+
| Name                   | Type                  | Description                                                                |
+========================+=======================+============================================================================+
| index_owner            | character varying(64) | Name of the owner of the partitioned table index                           |
+------------------------+-----------------------+----------------------------------------------------------------------------+
| schema                 | character varying(64) | Schema of the partitioned table index                                      |
+------------------------+-----------------------+----------------------------------------------------------------------------+
| index_name             | character varying(64) | Name of the partitioned table index                                        |
+------------------------+-----------------------+----------------------------------------------------------------------------+
| table_name             | character varying(64) | Name of the partitioned table to which the partitioned table index belongs |
+------------------------+-----------------------+----------------------------------------------------------------------------+
| partitioning_type      | text                  | Partition policy of the partitioned table                                  |
|                        |                       |                                                                            |
|                        |                       | .. note::                                                                  |
|                        |                       |                                                                            |
|                        |                       |    Currently, only range partitioning and list partitioning are supported. |
+------------------------+-----------------------+----------------------------------------------------------------------------+
| partition_count        | bigint                | Number of index partitions of the partitioned table index                  |
+------------------------+-----------------------+----------------------------------------------------------------------------+
| def_tablespace_name    | name                  | Tablespace name of the partitioned table index                             |
+------------------------+-----------------------+----------------------------------------------------------------------------+
| partitioning_key_count | integer               | Number of partition keys of the partitioned table                          |
+------------------------+-----------------------+----------------------------------------------------------------------------+
