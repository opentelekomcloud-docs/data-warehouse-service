:original_name: dws_04_0869.html

.. _dws_04_0869:

USER_PART_TABLES
================

**USER_PART_TABLES** displays information about partitioned tables accessible to the current user.

.. table:: **Table 1** USER_PART_TABLES columns

   +------------------------+-----------------------+---------------------------------------------------+
   | Name                   | Type                  | Description                                       |
   +========================+=======================+===================================================+
   | table_owner            | character varying(64) | Name of the owner of the partitioned table        |
   +------------------------+-----------------------+---------------------------------------------------+
   | schema                 | character varying(64) | Schema of the partitioned table                   |
   +------------------------+-----------------------+---------------------------------------------------+
   | table_name             | character varying(64) | Name of the partitioned table                     |
   +------------------------+-----------------------+---------------------------------------------------+
   | partitioning_type      | text                  | Partition policy of the partitioned table         |
   +------------------------+-----------------------+---------------------------------------------------+
   | partition_count        | bigint                | Number of partitions of the partitioned table     |
   +------------------------+-----------------------+---------------------------------------------------+
   | def_tablespace_name    | name                  | Name of the tablespace of the partitioned table   |
   +------------------------+-----------------------+---------------------------------------------------+
   | partitioning_key_count | integer               | Number of partition keys of the partitioned table |
   +------------------------+-----------------------+---------------------------------------------------+
