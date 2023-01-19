:original_name: dws_04_0868.html

.. _dws_04_0868:

USER_PART_INDEXES
=================

**USER_PART_INDEXES** displays information about partitioned table indexes accessible to the current user.

.. table:: **Table 1** USER_PART_INDEXES columns

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
   +------------------------+-----------------------+----------------------------------------------------------------------------+
   | partition_count        | bigint                | Number of index partitions of the partitioned table index                  |
   +------------------------+-----------------------+----------------------------------------------------------------------------+
   | def_tablespace_name    | name                  | Name of the tablespace of the partitioned table index                      |
   +------------------------+-----------------------+----------------------------------------------------------------------------+
   | partitioning_key_count | integer               | Number of partition keys of the partitioned table                          |
   +------------------------+-----------------------+----------------------------------------------------------------------------+
