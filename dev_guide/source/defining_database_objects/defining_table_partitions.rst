:original_name: dws_04_0037.html

.. _dws_04_0037:

Defining Table Partitions
=========================

Partitioning refers to splitting what is logically one large table into smaller physical pieces based on specific schemes. The table based on the logic is called a partition cable, and a physical piece is called a partition. Data is stored on these smaller physical pieces, namely, partitions, instead of the larger logical partitioned table. During conditional query, the system scans only the partitions that meet the conditions rather than scanning the entire table improving query performance.

Advantages of partitioned tables:

-  Improved query performance. You can search in specific partitions, improving the search efficiency.
-  Enhanced availability. If a partition is faulty, data in other partitions is still available.
-  Improved maintainability. For expired historical data that needs to be periodically deleted, you can quickly delete it by dropping or truncate partitions.

Supported Table Partition Types
-------------------------------

-  Range partitioning: partitions are created based on a numeric range, for example, by date or price range.
-  List partitioning: partitions are created based on a list of values, such as sales scope or product attribute. Only clusters of 8.1.3 and later versions support this function.

Choosing to Partition a Table
-----------------------------

You can choose to partition a table when the table has the following characteristics:

-  There are obvious ranges among the fields of the table.

   A table is partitioned based on obvious rangeable fields. Generally, columns such as date, area, and value are used for partitioning. The time column is most commonly used.

-  Queries to the table have obvious range characteristics.

   If the queried data fall into specific ranges, its better tables are partitioned so that through partition pruning, only the queried partition needs to be scanned, improving data scanning efficiency and reducing the I/O overhead of data scanning.

-  The table contains a large amount of data.

   Scanning small tables does not take much time, therefore the performance benefits of partitioning are not significant. Therefore, you are advised to partition only large tables. In column-store table, each column is an independent file storage unit, and the minimum storage unit CU can store 60,000 rows of data. Therefore, for column-store partitioned tables, it is recommended that the data volume in each partition be greater than or equal to the number of DNs multiplied by 60,000.

Creating a Range Partitioned Table
----------------------------------

Example: Create a table **web_returns_p1** partitioned by the range **wr_returned_date_sk**.

::

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
       PARTITION pxxxx VALUES LESS THAN(maxvalue)
   );

Create partitions in batches, with fixed partition ranges. The following example can be used:

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

Creating a List Partitioned Table
---------------------------------

A list partitioned table can use any column that allows value comparison as the partition key column. When creating a list partitioned table, you must declare the value partition for each partition.

Example: Create a list partitioned table **sales_info**.

::

   CREATE TABLE sales_info
   (
   sale_time  timestamptz,
   period     int,
   city       text,
   price      numeric(10,2),
   remark     varchar2(100)
   )
   DISTRIBUTE BY HASH(sale_time)
   PARTITION BY LIST (period, city)
   (
   PARTITION province1_202201 VALUES (('202201', 'city1'), ('202201', 'city2')),
   PARTITION province2_202201 VALUES (('202201', 'city3'), ('202201', 'city4'), ('202201', 'city5')),
   PARTITION rest VALUES (DEFAULT)
   );

Partitioning an Existing Table
------------------------------

A table can be partitioned only when it is created. If you want to partition a table, you must create a partitioned table, load the data in the original table to the partitioned table, delete the original table, and rename the partitioned table as the name of the original table. You must also re-grant permissions on the table to users. For example:

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

::

   INSERT INTO web_returns_p2 SELECT * FROM web_returns_p1;
   DROP TABLE web_returns_p1;
   ALTER TABLE web_returns_p2 RENAME TO web_returns_p1;
   GRANT ALL PRIVILEGES ON web_returns_p1 TO dbadmin;
   GRANT SELECT ON web_returns_p1 TO jack;

Adding a Partition
------------------

Run the **ALTER TABLE** statement to add a partition to a partitioned table. For example, to add partition **P2020** to the **web_returns_p1** table, run the following command:

::

   ALTER TABLE web_returns_p1 ADD PARTITION P2020 VALUES LESS THAN (20201231);

Splitting a Partition
---------------------

The syntax for splitting a partition varies between a range partitioned table and a list partitioned table.

-  Run the **ALTER TABLE** statement to split a partition in a range partitioned table. For example, the partition **pxxxx** of the table **web_returns_p1** is split into two partitions **p2020** and **p20xx** at the splitting point **20201231**.

   ::

      ALTER TABLE web_returns_p1 SPLIT PARTITION pxxxx AT(20201231) INTO (PARTITION p2020,PARTITION p20xx);

-  Run the **ALTER TABLE** statement to split a partition in a list partitioned table. For example, split the partition **province2_202201** of table **sales_inf** into two partitions **province3_202201** and **province4_202201**.

   ::

      ALTER TABLE sales_info SPLIT PARTITION province2_202201 VALUES(('202201', 'city5')) INTO (PARTITION province3_202201,PARTITION province4_202201);

Merging Partitions
------------------

Run the **ALTER TABLE** statement to merge two partitions in a partitioned table. For example, merge partitions **p2016** and **p2017** of table **web_returns_p1** into one partition **p20162017**.

::

   ALTER TABLE web_returns_p1 MERGE PARTITIONS p2016,p2017 INTO PARTITION p20162017;

Deleting a Partition
--------------------

Run the **ALTER TABLE** statement to delete a partition from a partitioned table. For example, run the following command to delete partition **P2020** from the **web_returns_p1** table:

::

   ALTER TABLE web_returns_p1 DROP PARTITION P2020;

Querying a Partition
--------------------

-  Query partition **p2019**.

   ::

      SELECT * FROM web_returns_p1 PARTITION (p2019);
      SELECT * FROM web_returns_p1 PARTITION FOR (20201231);

-  View partitioned tables using the system catalog **dba_tab_partitions**.

   ::

      SELECT * FROM dba_tab_partitions where table_name='web_returns_p1';

Deleting a Partitioned Table
----------------------------

Run the **DROP TABLE** statement to delete a partitioned table.

::

   DROP TABLE web_returns_p1;
