:original_name: dws_04_0037.html

.. _dws_04_0037:

Creating and Managing DWS Partitioned Tables
============================================

Partitioning refers to splitting what is logically one large table into smaller physical pieces based on specific schemes. The table based on the logic is called a partition cable, and a physical piece is called a partition. Data is stored on these smaller physical pieces, namely, partitions, instead of the larger logical partitioned table. During conditional query, the system scans only the partitions that meet the conditions rather than scanning the entire table, improving query performance.

For details about the syntax for maintaining partitioned tables, see "ALTER TABLE PARTITION" in *SQL Syntax Reference*.

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

   Scanning small tables does not take much time, therefore the performance benefits of partitioning are not significant. Therefore, you are advised to partition only large tables. In column-store tables, each column is an independent file storage unit, and the minimum storage unit CU can store 60,000 rows of data. Therefore, for column-store partitioned tables, it is recommended that the data volume in each partition be greater than or equal to the number of DNs multiplied by 60,000.

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

Partition the table **web_returns_p2** by date and time, using time as the partition key.

::

   CREATE TABLE web_returns_p2
   (
      id integer,
      idle numeric,
      IO numeric,
      scope text,
      IP text,
      time timestamp
   )
    WITH (TTL='7 days',PERIOD='1 day')
   PARTITION BY RANGE(time)
    (
      PARTITION P1 VALUES LESS THAN('2022-01-05 16:32:45'),
      PARTITION P2 VALUES LESS THAN('2022-01-06 16:56:12')
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
   PARTITION province2_202201 VALUES (('202201', 'city3'), ('202201', 'city4'), ('202201', 'city5'))
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

Add a partition to a list partitioned table. For example, add the **province3_202201** partition to the **sales_info** table.

::

   ALTER TABLE sales_info ADD PARTITION province3_202201 VALUES (('202201', 'city6'), ('202201', 'city7'));

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

Setting Whether a Partitioned Index Is Available
------------------------------------------------

Create the local index **student_grade_index** for the partitioned table **customer_address** and set partition index names.

::

   CREATE INDEX customer_address_index ON customer_address(ca_address_id) LOCAL
   (
           PARTITION P1_index,
           PARTITION P2_index,
           PARTITION P3_index
   );

Rebuild all indexes on partition **P1** in the partitioned table **customer_address**.

::

   ALTER TABLE customer_address MODIFY PARTITION P1 REBUILD UNUSABLE LOCAL INDEXES;

Set all indexes in partition **P3** of the partitioned table **customer_address** to be unusable.

::

   ALTER TABLE customer_address MODIFY PARTITION P3 UNUSABLE LOCAL INDEXES;

Rebuilding Partition Indexes
----------------------------

For a partitioned table that has been running for a long time, indexes may generate fragments. Rebuilding indexes can improve query efficiency. The following describes how to rebuild indexes after data is inserted into partitions.

#. Creates a partitioned table.

   ::

      DROP TABLE IF EXISTS sales;
      CREATE TABLE sales (
          id          INT,
          sale_date   DATE,
          amount      DECIMAL(10,2)
      )
      PARTITION BY RANGE (sale_date)
      (
          PARTITION p202310 VALUES LESS THAN ('2023-11-01'),
          PARTITION p202311 VALUES LESS THAN ('2023-12-01')
      );

#. Create an index.

   ::

      CREATE INDEX idx_sale_date ON sales (sale_date)  LOCAL;

#. Insert partition data of October and November.

   ::

      INSERT INTO sales PARTITION (p202310)
      VALUES
      (1, '2023-10-05', 100.50),
      (2, '2023-10-10', 200.75);
      INSERT INTO sales PARTITION (p202311)
      VALUES
      (3, '2023-11-15', 300.00);

#. Rebuild indexes of the specific partition **p202310**.

   ::

      ALTER TABLE sales  REBUILD PARTITION p202310  WITHOUT UNUSABLE;

Exchanging Partitions
---------------------

The following example demonstrates how to migrate data from table **math_grade** to partition **math** in partitioned table **student_grade**.

#. Create the partitioned table **student_grade**.

   ::

      DROP TABLE IF EXISTS student_grade;
      CREATE TABLE student_grade (
              stu_name     char(5),
              stu_no       integer,
              grade        integer,
              subject      varchar(30)
      )
      PARTITION BY LIST(subject)
      (
              PARTITION gym VALUES('gymnastics'),
              PARTITION phys VALUES('physics'),
              PARTITION history VALUES('history'),
              PARTITION math VALUES('math')
      );

#. Add data to the partitioned table **student_grade**.

   ::

      INSERT INTO student_grade VALUES
              ('Ann', 20220101, 75, 'gymnastics'),
              ('Jack', 20220103, 60, 'math'),
              ('Anna', 20220108, 56, 'history'),
              ('John', 20220107, 82, 'physics'),
              ('Molly', 20220104, 91, 'physics'),
              ('Sam', 20220105, 72, 'math');

#. Query the records of partition **math** in **student_grade**.

   ::

      SELECT * FROM student_grade PARTITION (math);

   The query result is as follows:

   |image1|

#. Create an ordinary table **math_grade** that matches the definition of the partitioned table **student_grade**.

   ::

      DROP TABLE IF EXISTS math_grade;
      CREATE TABLE math_grade
      (
              stu_name     char(5),
              stu_no       integer,
              grade        integer,
              subject      varchar(30)
      );

#. Insert data to the **math_grade** table. The data in the **student_grade** partitioned table conforms to the partition rule of partition **math**.

   ::

      INSERT INTO math_grade VALUES
              ('Ann', 20220101, 75, 'math'),
              ('Jack', 20220103, 60, 'math'),
              ('Anna', 20220108, 56, 'math'),
              ('John', 20220107, 82, 'math');

#. Migrate data from the ordinary table **math_grade** to partition **math** in the partitioned table **student_grade**.

   ::

      ALTER TABLE student_grade EXCHANGE PARTITION (math) WITH TABLE math_grade;

#. Query the partitioned table **student_grade**. The result shows that the data in the table **math_grade** has been exchanged with the data in partition **math** of the partitioned table **student_grade**.

   ::

      SELECT * FROM student_grade PARTITION (math);

   |image2|

#. Query the **math_grade** table. The result shows that the data stored in the **math** partition of the **student_grade** partitioned table has been exchanged with the **math_grade** table.

   ::

      SELECT * FROM math_grade;

   |image3|

Locks Applied to Different Operations on Partitioned Tables
-----------------------------------------------------------

Different operations of partitioned tables are finally performed on the target partitions. The database applies table locks and partition locks of different levels to the partitioned table and the target partitions to control concurrent operations.

.. table:: **Table 1** Conflicts between lock modes

   +--------------------------+------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------+
   | Lock Mode                | Lock Level | Lock Usage                                                                                                                                                                                       | Conflict        |
   +==========================+============+==================================================================================================================================================================================================+=================+
   | AccessShareLock          | 1          | SELECT statement, allowing other transactions to read data.                                                                                                                                      | 8               |
   +--------------------------+------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------+
   | RowShareLock             | 2          | SELECT FOR UPDATE or FOR SHARE, allowing other transactions to read but preventing writes.                                                                                                       | 7|8             |
   +--------------------------+------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------+
   | RowExclusiveLock         | 3          | INSERT, UPDATE, and DELETE                                                                                                                                                                       | 5|6|7|8         |
   +--------------------------+------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------+
   | ShareUpdateExclusiveLock | 4          | VACUUM (non-FULL), ANALYZE, CREATE INDEX CONCURRENTLY, and COMMENT ON statements allows other transactions to read data but blocks writes.                                                       | 4|5|6|7|8       |
   +--------------------------+------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------+
   | ShareLock                | 5          | CREATE INDEX (non-CONCURRENTLY) allows other transactions to read data, but they cannot write data.                                                                                              | 3|4|6|7|8       |
   +--------------------------+------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------+
   | ShareRowExclusiveLock    | 6          | Similar to RowExclusiveLock, this lock lets you read data safely by using **ROW SELECT...FOR UPDATE**. But it permits RowShareLock, allowing others to read the row but not update or delete it. | 3|4|5|6|7|8     |
   +--------------------------+------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------+
   | ExclusiveLock            | 7          | This lock prevents **RowShareLock** or **SELECT... FOR UPDATE** operations.                                                                                                                      | 2|3|4|5|6|7|8   |
   +--------------------------+------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------+
   | AccessExclusiveLock      | 8          | The lock stops other transactions from reading and writing data during **ALTER TABLE**, **DROP TABLE**, or **VACUUM FULL** operations.                                                           | 1|2|3|4|5|6|7|8 |
   +--------------------------+------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------+

Partitioned tables use table locks and partition locks. Different levels of common locks are applied to tables and partitions to ensure behavior control during concurrent DQL, DML, and DDL operations. The following table lists the lock granularities of partitioned tables.

.. table:: **Table 2** Lock levels applied to different operations on partitioned tables

   +--------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Statement                                                          | Table Lock + Partition Lock                                                                                                                                  |
   +====================================================================+==============================================================================================================================================================+
   | SELECT full table                                                  | A level-1 lock on the CN primary table + Level-1 locks on all partitioned sub-tables (No sub-table lock is added for column-store tables.)                   |
   +--------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                                                                    | A level-1 lock on the DN primary table + Level-1 locks on all partitioned sub-tables                                                                         |
   +--------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | SELECT subpartitions                                               | A level-1 lock on the CN primary table + Level-1 locks on partitioned sub-tables                                                                             |
   +--------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                                                                    | A level-1 lock on the DN primary table + Level-1 locks on partitioned sub-tables                                                                             |
   +--------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | INSERT/UPDATE/DELETE primary table                                 | A level-3 lock on the CN primary table                                                                                                                       |
   +--------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                                                                    | A level-3 lock on the primary table + Level-3 locks on partitioned sub-tables                                                                                |
   +--------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | INSERT/UPDATE/DELETE sub-tables                                    | A level-3 lock on the CN primary table + Level-3 locks on partitioned sub-tables                                                                             |
   +--------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                                                                    | A level-3 lock on the primary table + Level-3 locks on partitioned sub-tables                                                                                |
   +--------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Light Runtime Analyze triggered by access to partition sub-tables  | A level-1 lock on the CN primary table + Level-1 locks on partitioned sub-tables                                                                             |
   +--------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                                                                    | A level-1 lock on the primary table + Level-1 locks on all partitioned sub-tables (other sub-partitions and locks are released immediately after being used) |
   +--------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Normal Runtime Analyze triggered by access to partition sub-tables | A level-4 lock on the CN primary table                                                                                                                       |
   +--------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                                                                    | A level-4 lock on the primary table + Level-4 locks on all partitioned sub-tables                                                                            |
   +--------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | ADD PARTITION                                                      | A level-3 lock on the CN primary table + Level-8 locks on partitioned sub-tables + Cluster lock                                                              |
   +--------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                                                                    | A level-3 lock on the primary table + Level-8 locks on partitioned sub-tables                                                                                |
   +--------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | SPLIT/MERGE/DROP PARTITION                                         | A level-8 lock on the primary table + Level-8 locks on partitioned sub-tables + a cluster lock                                                               |
   +--------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                                                                    | A level-8 lock on the primary table + Level-8 locks on partitioned sub-tables                                                                                |
   +--------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | TRUNCATE PARTITION                                                 | A level-3 lock on the CN primary table + Level-8 locks on partitioned sub-tables                                                                             |
   +--------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                                                                    | A level-3 lock on the primary table + Level-8 locks on partitioned sub-tables                                                                                |
   +--------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | EXCHANGE PARTITION                                                 | A level-3 lock on the CN primary table + Level-8 locks on partitioned sub-tables                                                                             |
   +--------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                                                                    | A level-3 lock on the primary table + Level-8 locks on partitioned sub-tables                                                                                |
   +--------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. |image1| image:: /_static/images/en-us_image_0000002568591613.png
.. |image2| image:: /_static/images/en-us_image_0000002537513428.png
.. |image3| image:: /_static/images/en-us_image_0000002537673762.png
