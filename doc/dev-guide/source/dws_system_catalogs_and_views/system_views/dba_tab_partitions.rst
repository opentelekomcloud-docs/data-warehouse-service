:original_name: dws_04_0676.html

.. _dws_04_0676:

DBA_TAB_PARTITIONS
==================

**DBA_TAB_PARTITIONS** displays information about all partitions in the database.

+-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Column                | Type                  | Description                                                                                                                                                                                                                           |
+=======================+=======================+=======================================================================================================================================================================================================================================+
| table_owner           | character varying(64) | Owner of the table that contains the partition                                                                                                                                                                                        |
+-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| schema                | character varying(64) | Schema of the partitioned table                                                                                                                                                                                                       |
+-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| table_name            | character varying(64) | Table name                                                                                                                                                                                                                            |
+-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| partition_name        | character varying(64) | Name of the partition                                                                                                                                                                                                                 |
+-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| high_value            | Text                  | Upper boundary of a range partition or boundary value set of a list partition                                                                                                                                                         |
|                       |                       |                                                                                                                                                                                                                                       |
|                       |                       | Reserved field for forward compatibility. The parameter **pretty_high_value** is added in version 8.1.3 to record the information.                                                                                                    |
+-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| pretty_high_value     | Text                  | Upper boundary of a range partition or boundary value set of a list partition                                                                                                                                                         |
|                       |                       |                                                                                                                                                                                                                                       |
|                       |                       | The query result is the instant decompilation output of the partition boundary expression. The output of this column contains more details than that of **high_value**. The output information can be collation and column data type. |
+-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| tablespace_name       | Name                  | Name of the tablespace that contains the partition                                                                                                                                                                                    |
+-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

View the partition information of a partitioned table:

::

   DROP TABLE IF EXISTS web_returns_p1;
   CREATE TABLE web_returns_p1
   (
       wr_returned_date_sk       integer,
       wr_returned_time_sk       integer,
       wr_item_sk                integer NOT NULL,
       wr_refunded_customer_sk   integer
   )
   WITH (orientation = column)
   DISTRIBUTE BY HASH (wr_item_sk)
   PARTITION BY RANGE (wr_returned_date_sk)
   (
       PARTITION p2016 VALUES LESS THAN(20161231),
       PARTITION p2017 VALUES LESS THAN(20171231),
       PARTITION p2018 VALUES LESS THAN(20181231),
       PARTITION p2019 VALUES LESS THAN(20191231),
       PARTITION p2020 VALUES LESS THAN(maxvalue)
   );

   SELECT * FROM dba_tab_partitions where table_name='web_returns_p1';

|image1|

.. |image1| image:: /_static/images/en-us_image_0000002591689546.png
