:original_name: dws_04_0676.html

.. _dws_04_0676:

DBA_TAB_PARTITIONS
==================

**DBA_TAB_PARTITIONS** displays information about all partitions in the database.

.. table:: **Table 1** DBA_TAB_PARTITIONS columns

   +-----------------+-----------------------+--------------------------------------------------------------+
   | Name            | Type                  | Description                                                  |
   +=================+=======================+==============================================================+
   | table_owner     | character varying(64) | Owner of the table that contains the partition               |
   +-----------------+-----------------------+--------------------------------------------------------------+
   | schema          | character varying(64) | Schema of the partitioned table                              |
   +-----------------+-----------------------+--------------------------------------------------------------+
   | table_name      | character varying(64) | Table name                                                   |
   +-----------------+-----------------------+--------------------------------------------------------------+
   | partition_name  | character varying(64) | Name of the partition                                        |
   +-----------------+-----------------------+--------------------------------------------------------------+
   | high_value      | text                  | Upper boundary of the range partition and interval partition |
   +-----------------+-----------------------+--------------------------------------------------------------+
   | tablespace_name | name                  | Name of the tablespace that contains the partition           |
   +-----------------+-----------------------+--------------------------------------------------------------+
