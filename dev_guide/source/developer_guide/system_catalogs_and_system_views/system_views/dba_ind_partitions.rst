:original_name: dws_04_0665.html

.. _dws_04_0665:

DBA_IND_PARTITIONS
==================

**DBA_IND_PARTITIONS** displays information about all index partitions in the database. Each index partition of a partitioned table in the database, if present, has a row of records in **DBA_IND_PARTITIONS**. It is accessible only to users with system administrator rights.

.. table:: **Table 1** DBA_IND_PARTITIONS columns

   +------------------------+-----------------------+---------------------------------------------------------------------------------+
   | Name                   | Type                  | Description                                                                     |
   +========================+=======================+=================================================================================+
   | index_owner            | character varying(64) | Name of the owner of the partitioned index to which the index partition belongs |
   +------------------------+-----------------------+---------------------------------------------------------------------------------+
   | schema                 | character varying(64) | Schema of the partitioned index to which the index partition belongs            |
   +------------------------+-----------------------+---------------------------------------------------------------------------------+
   | index_name             | character varying(64) | Index name of the partitioned table to which the index partition belongs        |
   +------------------------+-----------------------+---------------------------------------------------------------------------------+
   | partition_name         | character varying(64) | Name of the index partition                                                     |
   +------------------------+-----------------------+---------------------------------------------------------------------------------+
   | index_partition_usable | boolean               | Whether the index partition is available                                        |
   +------------------------+-----------------------+---------------------------------------------------------------------------------+
   | high_value             | text                  | Upper boundary of the partition corresponding to the index partition            |
   +------------------------+-----------------------+---------------------------------------------------------------------------------+
   | def_tablespace_name    | name                  | Tablespace name of the index partition                                          |
   +------------------------+-----------------------+---------------------------------------------------------------------------------+
