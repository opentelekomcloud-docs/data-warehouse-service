:original_name: dws_04_0669.html

.. _dws_04_0669:

DBA_PART_TABLES
===============

**DBA_PART_TABLES** displays information about all partitioned tables in the database. It is accessible only to users with system administrator rights.

.. table:: **Table 1** DBA_PART_TABLES columns

   +------------------------+-----------------------+-----------------------------------------------------+
   | Name                   | Type                  | Description                                         |
   +========================+=======================+=====================================================+
   | table_owner            | character varying(64) | Name of the owner of the partitioned table          |
   +------------------------+-----------------------+-----------------------------------------------------+
   | schema                 | character varying(64) | Schema of the partitioned table                     |
   +------------------------+-----------------------+-----------------------------------------------------+
   | table_name             | character varying(64) | Name of the partitioned table                       |
   +------------------------+-----------------------+-----------------------------------------------------+
   | partitioning_type      | text                  | Partition policy of the partitioned table           |
   |                        |                       |                                                     |
   |                        |                       | .. note::                                           |
   |                        |                       |                                                     |
   |                        |                       |    Currently, only range partitioning is supported. |
   +------------------------+-----------------------+-----------------------------------------------------+
   | partition_count        | bigint                | Number of partitions of the partitioned table       |
   +------------------------+-----------------------+-----------------------------------------------------+
   | def_tablespace_name    | name                  | Tablespace name of the partitioned table            |
   +------------------------+-----------------------+-----------------------------------------------------+
   | partitioning_key_count | integer               | Number of partition keys of the partitioned table   |
   +------------------------+-----------------------+-----------------------------------------------------+
