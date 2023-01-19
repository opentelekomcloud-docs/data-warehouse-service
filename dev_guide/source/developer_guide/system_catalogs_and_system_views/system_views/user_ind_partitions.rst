:original_name: dws_04_0865.html

.. _dws_04_0865:

USER_IND_PARTITIONS
===================

**USER_IND_PARTITIONS** displays information about index partitions accessible to the current user.

.. table:: **Table 1** USER_IND_PARTITIONS columns

   +------------------------+-----------------------+---------------------------------------------------------------------------------------+
   | Name                   | Type                  | Description                                                                           |
   +========================+=======================+=======================================================================================+
   | index_owner            | character varying(64) | Name of the owner of the partitioned table index to which the index partition belongs |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------+
   | schema                 | character varying(64) | Schema of the partitioned table index to which the index partition belongs            |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------+
   | index_name             | character varying(64) | Name of the partitioned table index to which the index partition belongs              |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------+
   | partition_name         | character varying(64) | Name of the index partition                                                           |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------+
   | index_partition_usable | boolean               | Whether the index partition is available                                              |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------+
   | high_value             | text                  | Upper limit of the partition corresponding to the index partition                     |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------+
   | def_tablespace_name    | name                  | Name of the tablespace of the index partition                                         |
   +------------------------+-----------------------+---------------------------------------------------------------------------------------+
