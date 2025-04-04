:original_name: dws_03_0088.html

.. _dws_03_0088:

How Do I Delete Table Data Efficiently in GaussDB(DWS)?
=======================================================

Yes. **TRUNCATE** is more efficient than **DELETE** for deleting massive data.

Function
--------

TRUNCATE quickly removes all rows from a table. It has the same effect as an unqualified DELETE but since it does not actually scan the table it is faster. This is most effective on large tables.

Functions
---------

-  **TRUNCATE TABLE** works like a **DELETE** statement with no **WHERE** clause, that is, emptying a table.
-  **TRUNCATE TABLE** uses less system and transaction log resources.

   -  **DELETE** deletes a row each time, and records each deletion in the transaction log.
   -  **TRUNCATE TABLE** deletes all rows in a table by releasing the data page, and only records each releasing of the data page in the transaction log.

-  **TRUNCATE**, **DELETE**, and **DROP** are different in that:

   -  **TRUNCATE TABLE** deletes content, releases space, but does not delete definitions.
   -  **DELETE TABLE** deletes content, but does not delete definitions or release space.
   -  **DROP TABLE** deletes content and definitions, and releases space.

Examples
--------

-  Create a table.

   ::

      CREATE TABLE tpcds.reason_t1 AS TABLE tpcds.reason;

   Truncate the table.

   ::

      TRUNCATE TABLE tpcds.reason_t1;

   Delete the table.

   ::

      DROP TABLE tpcds.reason_t1;

-  Create a partitioned table.

   ::

      CREATE TABLE tpcds.reason_p
      (
        r_reason_sk integer,
        r_reason_id character(16),
        r_reason_desc character(100)
      )PARTITION BY RANGE (r_reason_sk)
      (
        partition p_05_before values less than (05),
        partition p_15 values less than (15),
        partition p_25 values less than (25),
        partition p_35 values less than (35),
        partition p_45_after values less than (MAXVALUE)
      );

   Insert data.

   ::

      INSERT INTO tpcds.reason_p SELECT * FROM tpcds.reason;

   Truncate the **p_05_before** partition.

   ::

      ALTER TABLE tpcds.reason_p TRUNCATE PARTITION p_05_before;

   Truncate the partition **p_15** where 13 is located.

   ::

      ALTER TABLE tpcds.reason_p TRUNCATE PARTITION for (13);

   Truncate the partitioned table.

   ::

      TRUNCATE TABLE tpcds.reason_p;

   Delete the table.

   ::

      DROP TABLE tpcds.reason_p;
