:original_name: dws_16_0146.html

.. _dws_16_0146:

PARTITION BY
============

In MySQL, PARTITION BY is used to create partitioned tables. Currently, GaussDB(DWS) supports only range and list partitions in MySQL.

.. caution::

   Hash partitions of PARTITION BY are not supported and deleted during migration. The deletion does not affect the current table functions, but may affect the performance slightly.

PARTITION BY RANGE
------------------

**Input**

.. code-block::

   CREATE TABLE IF NOT EXISTS `runoob_tbl_part_test`(
     `runoob_id` INT NOT NULL,
     `runoob_title` VARCHAR(100) NOT NULL,
     `runoob_author` VARCHAR(40) NOT NULL,
     `submission_date` VARCHAR(30)
   )ENGINE=InnoDB DEFAULT CHARSET=utf8
   PARTITION BY RANGE (`runoob_id`)(
       PARTITION p0 VALUES LESS THAN(100),
       PARTITION p1 VALUES LESS THAN(200),
       PARTITION p2 VALUES LESS THAN(300),
       PARTITION p3 VALUES LESS THAN(400),
       PARTITION p4 VALUES LESS THAN(500),
       PARTITION p5 VALUES LESS THAN (MAXVALUE)
   );

   ALTER TABLE `runoob_tbl_part_test` ADD PARTITION (PARTITION p6 VALUES LESS THAN (600));

   ALTER TABLE `runoob_tbl_part_test` ADD PARTITION (PARTITION p7 VALUES LESS THAN (700),PARTITION p8 VALUES LESS THAN (800));

**Output**

.. code-block::

   CREATE TABLE IF NOT EXISTS "public"."runoob_tbl_part_test" (
     "runoob_id" INTEGER NOT NULL,
     "runoob_title" VARCHAR(400) NOT NULL,
     "runoob_author" VARCHAR(160) NOT NULL,
     "submission_date" VARCHAR(120)
   ) WITH (ORIENTATION = ROW, COMPRESSION = NO) NOCOMPRESS DISTRIBUTE BY HASH ("runoob_id") PARTITION BY RANGE ("runoob_id") (
     PARTITION p0
     VALUES
       LESS THAN (100),
       PARTITION p1
     VALUES
       LESS THAN (200),
       PARTITION p2
     VALUES
       LESS THAN (300),
       PARTITION p3
     VALUES
       LESS THAN (400),
       PARTITION p4
     VALUES
       LESS THAN (500),
       PARTITION p5
     VALUES
       LESS THAN (MAXVALUE)
   );
   ALTER TABLE "public"."runoob_tbl_part_test" ADD PARTITION p6 VALUES LESS THAN (600);
   ALTER TABLE "public"."runoob_tbl_part_test" ADD PARTITION p7 VALUES LESS THAN (700), ADD PARTITION p8 VALUES LESS THAN (800);

PARTITION BY LIST
-----------------

**Input**

.. code-block::

   CREATE TABLE IF NOT EXISTS `runoob_tbl_part_test`(
       `runoob_id` INT NOT NULL,
       `runoob_title` VARCHAR(100) NOT NULL,
       `runoob_author` VARCHAR(40) NOT NULL,
       `submission_date` VARCHAR(30),
       PRIMARY KEY (`runoob_id`)
       )ENGINE=InnoDB DEFAULT CHARSET=utf8
       PARTITION BY LIST (runoob_id)(
       PARTITION r0 VALUES IN (1, 5, 9, 13, 17, 21),
       PARTITION r1 VALUES IN (2, 6, 10, 14, 18, 22),
       PARTITION r2 VALUES IN (3, 7, 11, 15, 19, 23),
       PARTITION r3 VALUES IN (4, 8, 12, 16, 20, 24)
       );
   ALTER TABLE `runoob_tbl_part_test` ADD PARTITION (PARTITION p5 VALUES IN(30, 40, 50, 60, 70, 80));

**Output**

.. code-block::

   CREATE TABLE IF NOT EXISTS "public"."runoob_tbl_part_test" (
     "runoob_id" INTEGER NOT NULL,
     "runoob_title" VARCHAR(400) NOT NULL,
     "runoob_author" VARCHAR(160) NOT NULL,
     "submission_date" VARCHAR(120),
     PRIMARY KEY ("runoob_id")
   ) WITH (ORIENTATION = ROW, COMPRESSION = NO) NOCOMPRESS DISTRIBUTE BY HASH ("runoob_id") PARTITION BY LIST (runoob_id) (
     PARTITION r0
     VALUES
       (1, 5, 9, 13, 17, 21),
       PARTITION r1
     VALUES
       (2, 6, 10, 14, 18, 22),
       PARTITION r2
     VALUES
       (3, 7, 11, 15, 19, 23),
       PARTITION r3
     VALUES
       (4, 8, 12, 16, 20, 24)
   );
   ALTER TABLE "public"."runoob_tbl_part_test" ADD PARTITION p5 VALUES (30, 40, 50, 60, 70, 80);
