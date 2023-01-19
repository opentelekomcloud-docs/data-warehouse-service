:original_name: dws_04_0876.html

.. _dws_04_0876:

USER_TAB_PARTITIONS
===================

**USER_TAB_PARTITIONS** displays all table partitions accessible to the current user. Each partition of a partitioned table accessible to the current user has a piece of record in **USER_TAB_PARTITIONS**.

.. table:: **Table 1** USER_TAB_PARTITIONS columns

   +-----------------+-----------------------+-----------------------------------------------+
   | Name            | Type                  | Description                                   |
   +=================+=======================+===============================================+
   | table_owner     | character varying(64) | Name of the owner of the partitioned table    |
   +-----------------+-----------------------+-----------------------------------------------+
   | schema          | character varying(64) | Schema of the partitioned table               |
   +-----------------+-----------------------+-----------------------------------------------+
   | table_name      | character varying(64) | Name of the partitioned table                 |
   +-----------------+-----------------------+-----------------------------------------------+
   | partition_name  | character varying(64) | Name of the table partition                   |
   +-----------------+-----------------------+-----------------------------------------------+
   | high_value      | text                  | Upper boundary of the table partition         |
   +-----------------+-----------------------+-----------------------------------------------+
   | tablespace_name | name                  | Name of the tablespace of the table partition |
   +-----------------+-----------------------+-----------------------------------------------+
