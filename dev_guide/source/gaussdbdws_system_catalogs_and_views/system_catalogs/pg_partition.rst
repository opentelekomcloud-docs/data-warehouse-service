:original_name: dws_04_0606.html

.. _dws_04_0606:

PG_PARTITION
============

**PG_PARTITION** records all partitioned tables, table partitions, toast tables on table partitions, and index partitions in the database. Partitioned index information is not stored in the **PG_PARTITION** system catalog.

.. table:: **Table 1** PG_PARTITION columns

   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | Column                | Type                  | Description                                                                                                                                      |
   +=======================+=======================+==================================================================================================================================================+
   | relname               | Name                  | Names of the partitioned tables, table partitions, TOAST tables on table partitions, and index partitions                                        |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | parttype              | Char                  | Object type                                                                                                                                      |
   |                       |                       |                                                                                                                                                  |
   |                       |                       | -  **r** indicates a partitioned table.                                                                                                          |
   |                       |                       | -  **p** indicates a table partition.                                                                                                            |
   |                       |                       | -  **x** indicates an index partition.                                                                                                           |
   |                       |                       | -  **t** indicates a TOAST table.                                                                                                                |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | parentid              | OID                   | OID of the partitioned table in **PG_CLASS** when the object is a partitioned table or table partition                                           |
   |                       |                       |                                                                                                                                                  |
   |                       |                       | OID of the partitioned index when the object is an index partition                                                                               |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | rangenum              | Integer               | Reserved field.                                                                                                                                  |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | intervalnum           | Integer               | Reserved field.                                                                                                                                  |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | partstrategy          | Char                  | Partition policy of the partitioned table. Only the following policies are supported:                                                            |
   |                       |                       |                                                                                                                                                  |
   |                       |                       | **r** indicates the range partition.                                                                                                             |
   |                       |                       |                                                                                                                                                  |
   |                       |                       | **v** indicates the numeric partition.                                                                                                           |
   |                       |                       |                                                                                                                                                  |
   |                       |                       | **l**: indicates the list partition.                                                                                                             |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | relfilenode           | OID                   | Physical storage locations of the table partition, index partition, and TOAST table on the table partition.                                      |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | reltablespace         | OID                   | OID of the tablespace containing the table partition, index partition, TOAST table on the table partition                                        |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | relpages              | Double precision      | Statistics: numbers of data pages of the table partition and index partition                                                                     |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | reltuples             | Double precision      | Statistics: numbers of tuples of the table partition and index partition                                                                         |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | relallvisible         | Integer               | Statistics: number of visible data pages of the table partition and index partition                                                              |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | reltoastrelid         | OID                   | OID of the TOAST table corresponding to the table partition                                                                                      |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | reltoastidxid         | OID                   | OID of the TOAST table index corresponding to the table partition                                                                                |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | indextblid            | OID                   | OID of the table partition corresponding to the index partition                                                                                  |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | indisusable           | boolean               | Whether the index partition is available                                                                                                         |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | reldeltarelid         | OID                   | OID of a Delta table                                                                                                                             |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | reldeltaidx           | OID                   | OID of the index for a Delta table                                                                                                               |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | relcudescrelid        | OID                   | OID of a CU description table                                                                                                                    |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | relcudescidx          | OID                   | OID of the index for a CU description table                                                                                                      |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | relfrozenxid          | xid32                 | Frozen transaction ID                                                                                                                            |
   |                       |                       |                                                                                                                                                  |
   |                       |                       | To ensure forward compatibility, this column is reserved. The **relfrozenxid64** column is added to record the information.                      |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | intspnum              | Integer               | Number of tablespaces that the interval partition belongs to                                                                                     |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | partkey               | int2vector            | Column number of the partition key                                                                                                               |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | intervaltablespace    | oidvector             | Tablespace that the interval partition belongs to. Interval partitions fall in the tablespaces in the round-robin manner.                        |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | interval              | Text[]                | Interval value of the interval partition                                                                                                         |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | boundaries            | Text[]                | Upper boundary of the range partition and interval partition                                                                                     |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | transit               | Text[]                | Transit of the interval partition                                                                                                                |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | reloptions            | Text[]                | Storage property of a partition used for collecting online scale-out information. Same as **pg_class.reloptions**, it is a keyword=value string. |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | relfrozenxid64        | Xid                   | Frozen transaction ID                                                                                                                            |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | boundexprs            | pg_node_tree          | Partition boundary expression.                                                                                                                   |
   |                       |                       |                                                                                                                                                  |
   |                       |                       | -  For range partitioning, it is the upper boundary expression of a partition.                                                                   |
   |                       |                       | -  For list partitioning, it is a collection of partition boundary enumeration values.                                                           |
   |                       |                       |                                                                                                                                                  |
   |                       |                       | The **pg_node_tree** data is not readable. You can use the expression **pg_get_expr** to translate the current column into readable information. |
   |                       |                       |                                                                                                                                                  |
   |                       |                       | ::                                                                                                                                               |
   |                       |                       |                                                                                                                                                  |
   |                       |                       |    SELECT pg_get_expr(boundexprs, 0) FROM pg_partition                                                                                           |
   |                       |                       |    WHERE relname = 'country_202201';                                                                                                             |
   |                       |                       |    pg_get_expr                                                                                                                                   |
   |                       |                       |    ---------------------------------------------------------------                                                                               |
   |                       |                       |    ROW(202201, 'city1'::text), ROW(202201, 'city2'::text)                                                                                        |
   |                       |                       |    (1 row)                                                                                                                                       |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
   | relmetaversion        | Xid                   | Metadata version information. This column is supported only by clusters of version 9.1.0 or later.                                               |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

Query the partition information of the partitioned table **web_returns_p2**.

::

   CREATE TABLE web_returns_p2
   (
       wr_returned_date_sk       integer,
       wr_returned_time_sk       integer,
       wr_item_sk                integer NOT NULL,
       wr_refunded_customer_sk   integer
   )
   WITH (orientation = column)
   DISTRIBUTE BY HASH (wr_item_sk)
   PARTITION BY RANGE(wr_returned_date_sk)
   (
       PARTITION p2016 START(20161231) END(20191231) EVERY(10000),
       PARTITION p0 END(maxvalue)
   );

   SELECT oid FROM pg_class WHERE relname ='web_returns_p2';
   OID
   -------
    97628

   SELECT relname,parttype,parentid,boundaries FROM pg_partition WHERE parentid = '97628';
       relname     | parttype | parentid | boundaries
   ----------------+----------+----------+------------
    web_returns_p2 | r        |    97628 |
    p2016_0        | p        |    97628 | {20161231}
    p2016_1        | p        |    97628 | {20171231}
    p2016_2        | p        |    97628 | {20181231}
    p2016_3        | p        |    97628 | {20191231}
    p0             | p        |    97628 | {NULL}
   (6 rows)
