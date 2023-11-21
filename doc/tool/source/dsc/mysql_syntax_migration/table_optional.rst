:original_name: dws_07_6832.html

.. _dws_07_6832:

Table (Optional)
================

This section describes the migration syntax of MySQL tables (optional parameter). The migration syntax determines how the keywords and features are migrated. GaussDB(DWS) does not support table migration. Currently, all table migration methods are temporary.

AUTO_INCREMENT
--------------

In database application, unique numbers that increase automatically are needed to identify records. In MySQL, the **AUTO_INCREMENT** attribute of a data column can be used to automatically generate the numbers. When creating a table, you can use **AUTO_INCREMENT=**\ *n* to specify a start value. You can also use the **ALTER TABLE** *TABLE_NAME* **AUTO_INCREMENT=**\ *n* command to reset the start value. GaussDB(DWS) does not support this attribute, which will be converted to **SERIAL** and deleted by DSC during migration.

**Input**

.. code-block::

   CREATE TABLE `public`.`job_instance` (
     `job_sche_id` int(11) NOT NULL AUTO_INCREMENT,
     `task_name` varchar(100) NOT NULL DEFAULT '',
     PRIMARY KEY (`job_sche_id`)
   ) ENGINE=InnoDB AUTO_INCREMENT=219 DEFAULT CHARSET=utf8;

**Output**

.. code-block::

   CREATE TABLE "public"."job_instance"
   (
     "job_sche_id" SERIAL NOT NULL,
     "task_name" VARCHAR(100) NOT NULL DEFAULT '',
     PRIMARY KEY ("job_sche_id")
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("job_sche_id");

In addition, GaussDB(DWS) does not support table definition modification using the **AUTO_INCREMENT** attribute. DSC will delete this attribute during migration.

**Input**

.. code-block::

   CREATE TABLE IF NOT EXISTS `public`.`runoob_alter_test`(
       `dataType1` int NOT NULL AUTO_INCREMENT,
       `dataType2` FLOAT(10,2),
       PRIMARY KEY(`dataType1`)
   );

   ALTER TABLE runoob_alter_test AUTO_INCREMENT 100;
   ALTER TABLE runoob_alter_test AUTO_INCREMENT=100;

**Output**

.. code-block::

   CREATE TABLE "public"."runoob_alter_test"
   (
     "datatype1" SERIAL NOT NULL,
     "datatype2" FLOAT(10),
     PRIMARY KEY ("datatype1")
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype1");

AVG_ROW_LENGTH
--------------

**Input**

.. code-block::

   CREATE TABLE `public`.`runoob_tbl_test`(
       `runoob_id` VARCHAR(30),
       `runoob_title` VARCHAR(100) NOT NULL,
       `runoob_author` VARCHAR(40) NOT NULL,
       `submission_date` VARCHAR(30)
   )AVG_ROW_LENGTH=10000;

**Output**

.. code-block::

   CREATE TABLE "public"."runoob_tbl_test"
   (
     "runoob_id" VARCHAR(30),
     "runoob_title" VARCHAR(100) NOT NULL,
     "runoob_author" VARCHAR(40) NOT NULL,
     "submission_date" VARCHAR(30)
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("runoob_id");

In addition, GaussDB(DWS) does not allow using the **AUTO_INCREMENT** attribute to modify tables. DSC will delete the attribute after migration.

**Input**

.. code-block::

   CREATE TABLE IF NOT EXISTS `public`.`runoob_alter_test`(
       `dataType1` int NOT NULL AUTO_INCREMENT,
       `dataType2` FLOAT(10,2),
       PRIMARY KEY(`dataType1`)
   );

   ALTER TABLE runoob_alter_test AUTO_INCREMENT 100;
   ALTER TABLE runoob_alter_test AUTO_INCREMENT=100;

**Output**

.. code-block::

   CREATE TABLE "public"."runoob_alter_test"
   (
     "datatype1" SERIAL NOT NULL,
     "datatype2" FLOAT(10),
     PRIMARY KEY ("datatype1")
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype1");

CHARSET
-------

**CHARSET** specifies the default character set for a table. GaussDB(DWS) does not support table definition modification using this attribute. DSC will delete the keyword during migration.

**Input**

.. code-block::

   CREATE TABLE `public`.`runoob_tbl_test`(
       `runoob_id` VARCHAR(30),
       `runoob_title` VARCHAR(100) NOT NULL,
       `runoob_author` VARCHAR(40) NOT NULL,
       `submission_date` VARCHAR(30)
   )DEFAULT CHARSET=utf8;

**Output**

.. code-block::

   CREATE TABLE "public"."runoob_tbl_test"
   (
     "runoob_id" VARCHAR(30),
     "runoob_title" VARCHAR(100) NOT NULL,
     "runoob_author" VARCHAR(40) NOT NULL,
     "submission_date" VARCHAR(30)
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("runoob_id");

CHECKSUM
--------

In MySQL, **CHECKSUM** maintains a live checksum for all rows. GaussDB(DWS) does not support table definition modification using this attribute. DSC will delete the keyword during migration.

**Input**

.. code-block::

   CREATE TABLE `public`.`runoob_alter_test`(
       `dataType1` int NOT NULL AUTO_INCREMENT,
       `dataType2` FLOAT(10,2),
       `dataType3` DOUBLE(20,8),
       PRIMARY KEY(`dataType1`)
   ) CHECKSUM=1;

   ALTER TABLE runoob_alter_test CHECKSUM 0;
   ALTER TABLE runoob_alter_test CHECKSUM=0;

   ALTER TABLE runoob_alter_test CHECKSUM 1;
   ALTER TABLE runoob_alter_test CHECKSUM=1;

**Output**

.. code-block::

   CREATE TABLE "public"."runoob_alter_test"
   (
     "datatype1" SERIAL NOT NULL,
     "datatype2" FLOAT(10),
     "datatype3" FLOAT(20),
     PRIMARY KEY ("datatype1")
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype1");

COLLATE
-------

In MySQL, **COLLATE** specifies a default database sorting rule. GaussDB(DWS) does not support table definition modification using this attribute. DSC will delete the keyword during migration.

**Input**

.. code-block::

   CREATE TABLE `public`.`runoob_tbl_test`(
       `runoob_id` VARCHAR(30),
       `runoob_title` VARCHAR(100) NOT NULL,
       `runoob_author` VARCHAR(40) NOT NULL,
       `submission_date` VARCHAR(30)
   ) COLLATE=utf8_general_ci;

   ALTER TABLE `public`.`runoob_tbl_test` COLLATE=utf8mb4_bin;

**Output**

.. code-block::

   CREATE TABLE "public"."runoob_tbl_test"
   (
     "runoob_id" VARCHAR(30),
     "runoob_title" VARCHAR(100) NOT NULL,
     "runoob_author" VARCHAR(40) NOT NULL,
     "submission_date" VARCHAR(30)
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("runoob_id");

COMMENT
-------

In MySQL, **COMMENT** is a comment for a table. GaussDB(DWS) does not support table definition modification using this attribute. DSC will delete the attribute during migration.

**Input**

.. code-block::

   CREATE TABLE `public`.`runoob_alter_test`(
       `dataType1` int NOT NULL AUTO_INCREMENT,
       `dataType2` FLOAT(10,2),
       PRIMARY KEY(`dataType1`)
   ) comment='table comment';

   ALTER TABLE `public`.`runoob_alter_test` COMMENT 'table comment after modification';

**Output**

.. code-block::

   CREATE TABLE "public"."runoob_alter_test"
   (
     "datatype1" SERIAL NOT NULL,
     "datatype2" FLOAT(10),
     PRIMARY KEY ("datatype1")
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype1");

CONNECTION
----------

GaussDB(DWS) does not support table definition modification using this attribute. DSC will delete the attribute during migration.

.. caution::

   In MySQL, the keyword **CONNECTION** is used as a referenced, external data source. Currently, DSC cannot completely migrate the feature of **CONNECTION**. So as a workaround, it simply deletes the keyword.

**Input**

.. code-block::

   CREATE TABLE `public`.`runoob_alter_test`(
       `dataType1` int NOT NULL AUTO_INCREMENT,
       `dataType2` DOUBLE(20,8),
       `dataType3` TEXT NOT NULL,
       `dataType4` YEAR NOT NULL DEFAULT '2018',
       PRIMARY KEY(`dataType1`)
   );

   ALTER TABLE runoob_alter_test CONNECTION 'hello';
   ALTER TABLE runoob_alter_test CONNECTION='hello';

**Output**

.. code-block::

   CREATE TABLE "public"."runoob_alter_test"
   (
     "datatype1" SERIAL NOT NULL,
     "datatype2" FLOAT(20),
     "datatype3" TEXT NOT NULL,
     "datatype4" VARCHAR(4) NOT NULL DEFAULT '2018',
     PRIMARY KEY ("datatype1")
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype1");

DELAY_KEY_WRITE
---------------

**DELAY_KEY_WRITE** is valid only for MyISAM tables. It is used to delay updates until the table is closed. GaussDB(DWS) does not support table definition modification using this attribute. DSC will delete the attribute during migration.

**Input**

.. code-block::

   CREATE TABLE `public`.`runoob_tbl_test`(
       `runoob_id` VARCHAR(30),
       `runoob_title` VARCHAR(100) NOT NULL,
       `runoob_author` VARCHAR(40) NOT NULL,
       `submission_date` VARCHAR(30)
   ) ENGINE=MyISAM, DELAY_KEY_WRITE=0;

   ALTER TABLE `public`.`runoob_tbl_test6` DELAY_KEY_WRITE=1;

**Output**

.. code-block::

   CREATE TABLE "public"."runoob_tbl_test"
   (
     "runoob_id" VARCHAR(30),
     "runoob_title" VARCHAR(100) NOT NULL,
     "runoob_author" VARCHAR(40) NOT NULL,
     "submission_date" VARCHAR(30)
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("runoob_id");

DIRECTORY
---------

**DIRECTORY** enables a tablespace to be created outside the data directory and index directory. It allows **DATA DIRECTORY** and **INDEX DIRECTORY**. GaussDB(DWS) does not support table definition modification using this attribute. DSC will delete the attribute during migration.

**Input**

.. code-block::

   CREATE TABLE `public`.`runoob_tbl_test1` (
   `dataType1` int NOT NULL AUTO_INCREMENT,
   `dataType2` DOUBLE(20,8),
   PRIMARY KEY(`dataType1`)
   ) ENGINE=MYISAM DATA DIRECTORY = 'D:\\input' INDEX DIRECTORY= 'D:\\input';

   CREATE TABLE `public`.`runoob_tbl_test2` (
   `dataType1` int NOT NULL AUTO_INCREMENT,
   `dataType2` DOUBLE(20,8),
   PRIMARY KEY(`dataType1`)
   ) ENGINE=INNODB DATA DIRECTORY = 'D:\\input';

**Output**

.. code-block::

   CREATE TABLE "public"."runoob_tbl_test1"
   (
     "datatype1" SERIAL NOT NULL,
     "datatype2" FLOAT(20),
     PRIMARY KEY ("datatype1")
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype1");

   CREATE TABLE "public"."runoob_tbl_test2"
   (
     "datatype1" SERIAL NOT NULL,
     "datatype2" FLOAT(20),
     PRIMARY KEY ("datatype1")
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype1");

ENGINE
------

In MySQL, **ENGINE** specifies the storage engine for a table. When the storage engine is **ARCHIVE**, **BLACKHOLE**, **CSV**, **FEDERATED**, **INNODB**, **MYISAM**, **MEMORY**, **MRG_MYISAM**, **NDB**, **NDBCLUSTER**, or **PERFORMANCE_SCHEMA**, this attribute can be migrated and will be deleted during the migration.

**Input**

.. code-block::

   CREATE TABLE `public`.`runoob_alter_test`(
   `dataType1` int NOT NULL,
   `dataType2` DOUBLE(20,8),
   PRIMARY KEY(`dataType1`)
   )ENGINE=MYISAM;

   ## A.
   ALTER TABLE runoob_alter_test ENGINE INNODB;
   ALTER TABLE runoob_alter_test ENGINE=INNODB;

   ## B.
   ALTER TABLE runoob_alter_test ENGINE MYISAM;
   ALTER TABLE runoob_alter_test ENGINE=MYISAM;

   ## C.
   ALTER TABLE runoob_alter_test ENGINE MEMORY;
   ALTER TABLE runoob_alter_test ENGINE=MEMORY;

**Output**

.. code-block::

   CREATE TABLE "public"."runoob_alter_test"
   (
     "datatype1" INTEGER NOT NULL,
     "datatype2" FLOAT(20),
     PRIMARY KEY ("datatype1")
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype1");

   -- A.

   -- B.

   -- C.

INSERT_METHOD
-------------

**INSERT_METHOD** specifies the table into which the row should be inserted. Use a value of **FIRST** or **LAST** to have inserts go to the first or last table, or a value of **NO** to prevent inserts. DSC will delete this attribute during migration.

**Input**

.. code-block::

   CREATE TABLE `public`.`runoob_alter_test`(
       `dataType1` int NOT NULL AUTO_INCREMENT,
       `dataType2` DOUBLE(20,8),
       `dataType3` TEXT NOT NULL,
       PRIMARY KEY(`dataType1`)
   ) INSERT_METHOD=LAST;

   ALTER TABLE runoob_alter_test INSERT_METHOD NO;
   ALTER TABLE runoob_alter_test INSERT_METHOD=NO;
   ALTER TABLE runoob_alter_test INSERT_METHOD FIRST;
   ALTER TABLE runoob_alter_test INSERT_METHOD=FIRST;
   ALTER TABLE runoob_alter_test INSERT_METHOD LAST;
   ALTER TABLE runoob_alter_test INSERT_METHOD=LAST;

**Output**

.. code-block::

   CREATE TABLE "public"."runoob_alter_test"
   (
     "datatype1" SERIAL NOT NULL,
     "datatype2" FLOAT(20),
     "datatype3" TEXT NOT NULL,
     PRIMARY KEY ("datatype1")
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype1");

KEY_BLOCK_SIZE
--------------

**KEY_BLOCK_SIZE** choices vary depending on the storage engine used for a table. For MyISAM tables, **KEY_BLOCK_SIZE** optionally specifies the size in bytes to be used for index key blocks. For InnoDB tables, **KEY_BLOCK_SIZE** specifies the page size in kilobytes to be used for compressed InnoDB tables. GaussDB(DWS) does not support this attribute, which will be deleted by DSC during migration.

**Input**

.. code-block::

   CREATE TABLE `public`.`runoob_tbl_test`(
       `runoob_id` VARCHAR(30),
       `runoob_title` VARCHAR(100) NOT NULL,
       `runoob_author` VARCHAR(40) NOT NULL,
       `submission_date` VARCHAR(30)
   ) ENGINE=MyISAM KEY_BLOCK_SIZE=8;

   ALTER TABLE runoob_tbl_test ENGINE=InnoDB;
   ALTER TABLE runoob_tbl_test KEY_BLOCK_SIZE=0;

**Output**

.. code-block::

   CREATE TABLE "public"."runoob_tbl_test"
   (
     "runoob_id" VARCHAR(30),
     "runoob_title" VARCHAR(100) NOT NULL,
     "runoob_author" VARCHAR(40) NOT NULL,
     "submission_date" VARCHAR(30)
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("runoob_id");

MAX_ROWS
--------

In MySQL, **MAX_ROWS** indicates the maximum number of rows that can be stored in a table. This attribute will be deleted during migration using DSC.

**Input**

.. code-block::

   CREATE TABLE `public`.`runoob_alter_test`(
       `dataType1` int NOT NULL AUTO_INCREMENT,
       `dataType2` DOUBLE(20,8),
       `dataType3` TEXT NOT NULL,
       PRIMARY KEY(`dataType1`)
   );

   ALTER TABLE runoob_alter_test MAX_ROWS 100000;
   ALTER TABLE runoob_alter_test MAX_ROWS=100000;

**Output**

.. code-block::

   CREATE TABLE "public"."runoob_alter_test"
   (
     "datatype1" SERIAL NOT NULL,
     "datatype2" FLOAT(20),
     "datatype3" TEXT NOT NULL,
     PRIMARY KEY ("datatype1")
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype1");

MIN_ROWS
--------

**MIN_ROWS** indicates the minimum number of rows that can be stored in a table. This attribute will be deleted during migration using DSC.

**Input**

.. code-block::

   CREATE TABLE `public`.`runoob_alter_test`(
       `dataType1` int NOT NULL AUTO_INCREMENT,
       `dataType2` DOUBLE(20,8),
       `dataType3` TEXT NOT NULL,
       PRIMARY KEY(`dataType1`)
   );
   ALTER TABLE runoob_alter_test MIN_ROWS 10000;
   ALTER TABLE runoob_alter_test MIN_ROWS=10000;

**Output**

.. code-block::

   CREATE TABLE "public"."runoob_alter_test"
   (
     "datatype1" SERIAL NOT NULL,
     "datatype2" FLOAT(20),
     "datatype3" TEXT NOT NULL,
     PRIMARY KEY ("datatype1")
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype1");

PACK_KEYS
---------

In MySQL, **PACK_KEYS** specifies the index compression mode in the MyISAM storage engine. GaussDB(DWS) does not support this attribute, which will be deleted by DSC during migration.

**Input**

.. code-block::

   CREATE TABLE `public`.`runoob_alter_test`(
       `dataType1` int NOT NULL AUTO_INCREMENT,
       `dataType2` DOUBLE(20,8),
       `dataType3` TEXT NOT NULL,
       PRIMARY KEY(`dataType1`)
   ) ENGINE=MyISAM PACK_KEYS=1;

   ##A
   ALTER TABLE runoob_alter_test PACK_KEYS 0;
   ALTER TABLE runoob_alter_test PACK_KEYS=0;

   ##B
   ALTER TABLE runoob_alter_test PACK_KEYS 1;
   ALTER TABLE runoob_alter_test PACK_KEYS=1;

   ##C
   ALTER TABLE runoob_alter_test PACK_KEYS DEFAULT;
   ALTER TABLE runoob_alter_test PACK_KEYS=DEFAULT;

**Output**

.. code-block::

   CREATE TABLE "public"."runoob_alter_test"
   (
     "datatype1" SERIAL NOT NULL,
     "datatype2" FLOAT(10),
     "datatype3" FLOAT(20),
     "datatype4" TEXT NOT NULL,
     PRIMARY KEY ("datatype1")
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype1");

   --A

   --B

   --C

PASSWORD
--------

In MySQL, **PASSWORD** indicates the user password. GaussDB(DWS) does not support this attribute, which will be deleted by DSC during migration.

**Input**

.. code-block::

   CREATE TABLE `public`.`runoob_alter_test`(
       `dataType1` int NOT NULL AUTO_INCREMENT,
       `dataType2` DOUBLE(20,8),
       `dataType3` TEXT NOT NULL,
       PRIMARY KEY(`dataType1`)
   );
   ALTER TABLE runoob_alter_test PASSWORD 'HELLO';

**Output**

.. code-block::

   CREATE TABLE "public"."runoob_alter_test"
   (
     "datatype1" SERIAL NOT NULL,
     "datatype2" FLOAT(20),
     "datatype3" TEXT NOT NULL,
     PRIMARY KEY ("datatype1")
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype1");

ROW_FORMAT
----------

**ROW_FORMAT** defines the physical format in which the rows are stored. Row format choices vary depending on the storage engine used for the table. If you specify a row format that is not supported by the storage engine that is used for the table, the table will be created using that storage engine's default row format. If **ROW_FORMAT** is set to **DEFAULT**, the value will be migrated to **SET NOCOMPRESS**. If **ROW_FORMAT** is set to **COMPRESSED**, the value will be migrated to **SET COMPRESS**. GaussDB(DWS) supports only the preceding two **ROW_FORMAT** values. If other values are used, they will be deleted by DSC.

**Input**

.. code-block::

   CREATE TABLE `public`.`runoob_alter_test`(
       `dataType1` int NOT NULL AUTO_INCREMENT,
       `dataType2` FLOAT(10,2),
       `dataType3` DOUBLE(20,8),
       `dataType4` TEXT NOT NULL,
       PRIMARY KEY(`dataType1`)
   ) ENGINE=InnoDB;

   ## A.
   ALTER TABLE runoob_alter_test ROW_FORMAT DEFAULT;
   ALTER TABLE runoob_alter_test ROW_FORMAT=DEFAULT;

   ## B.
   ALTER TABLE runoob_alter_test ROW_FORMAT DYNAMIC;
   ALTER TABLE runoob_alter_test ROW_FORMAT=DYNAMIC;

   ## C.
   ALTER TABLE runoob_alter_test ROW_FORMAT COMPRESSED;
   ALTER TABLE runoob_alter_test ROW_FORMAT=COMPRESSED;

   ## D.
   ALTER TABLE runoob_alter_test ROW_FORMAT REDUNDANT;
   ALTER TABLE runoob_alter_test ROW_FORMAT=REDUNDANT;

   ## E.
   ALTER TABLE runoob_alter_test ROW_FORMAT COMPACT;
   ALTER TABLE runoob_alter_test ROW_FORMAT=COMPACT;

**Output**

.. code-block::

   CREATE TABLE "public"."runoob_alter_test"
   (
     "datatype1" SERIAL NOT NULL,
     "datatype2" FLOAT(10),
     "datatype3" FLOAT(20),
     "datatype4" TEXT NOT NULL,
     PRIMARY KEY ("datatype1")
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype1");

   -- A.
   ALTER TABLE "public"."runoob_alter_test" SET NOCOMPRESS;
   ALTER TABLE "public"."runoob_alter_test" SET NOCOMPRESS;

   -- B.

   -- C.
   ALTER TABLE "public"."runoob_alter_test" SET COMPRESS;
   ALTER TABLE "public"."runoob_alter_test" SET COMPRESS;

   -- D.

   -- E.

STATS_AUTO_RECALC
-----------------

**STATS_AUTO_RECALC** specifies whether to automatically recalculate persistent statistics for an InnoDB table. GaussDB(DWS) does not support this attribute, which will be deleted by DSC during migration.

**Input**

.. code-block::

   CREATE TABLE `public`.`runoob_alter_test`(
       `runoob_id` VARCHAR(30),
       `runoob_title` VARCHAR(100) NOT NULL,
       `runoob_author` VARCHAR(40) NOT NULL,
       `submission_date` VARCHAR(30)
   ) ENGINE=InnoDB, STATS_AUTO_RECALC=DEFAULT;

   ## A.
   ALTER TABLE runoob_alter_test STATS_AUTO_RECALC DEFAULT;
   ALTER TABLE runoob_alter_test STATS_AUTO_RECALC=DEFAULT;

   ## B.
   ALTER TABLE runoob_alter_test STATS_AUTO_RECALC 0;
   ALTER TABLE runoob_alter_test STATS_AUTO_RECALC=0;

   ## C.
   ALTER TABLE runoob_alter_test STATS_AUTO_RECALC 1;
   ALTER TABLE runoob_alter_test STATS_AUTO_RECALC=1;

**Output**

.. code-block::

   CREATE TABLE "public"."runoob_alter_test"
   (
     "runoob_id" VARCHAR(30),
     "runoob_title" VARCHAR(100) NOT NULL,
     "runoob_author" VARCHAR(40) NOT NULL,
     "submission_date" VARCHAR(30)
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("runoob_id");

   -- A.

   -- B.

   -- C.

STATS_PERSISTENT
----------------

In MySQL, **STATS_PERSISTENT** specifies whether to enable persistence statistics for an InnoDB table. The **CREATE TABLE** and **ALTER TABLE** statements can be used to enable persistence statistics. DSC will delete this attribute during migration.

**Input**

.. code-block::

   CREATE TABLE `public`.`runoob_alter_test`(
       `dataType1` int NOT NULL AUTO_INCREMENT,
       `dataType2` DOUBLE(20,8),
       `dataType3` TEXT NOT NULL,
       PRIMARY KEY(`dataType1`)
   ) ENGINE=InnoDB, STATS_PERSISTENT=0;

   ## A.
   ALTER TABLE runoob_alter_test STATS_PERSISTENT DEFAULT;
   ALTER TABLE runoob_alter_test STATS_PERSISTENT=DEFAULT;

   ## B.
   ALTER TABLE runoob_alter_test STATS_PERSISTENT 0;
   ALTER TABLE runoob_alter_test STATS_PERSISTENT=0;

   ## C.
   ALTER TABLE runoob_alter_test STATS_PERSISTENT 1;
   ALTER TABLE runoob_alter_test STATS_PERSISTENT=1;

**Output**

.. code-block::

   CREATE TABLE "public"."runoob_alter_test"
   (
     "datatype1" SERIAL NOT NULL,
     "datatype2" FLOAT(20),
     "datatype3" TEXT NOT NULL,
     PRIMARY KEY ("datatype1")
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype1");

   -- A.

   -- B.

   -- C.

STATS_SAMPLE_PAGES
------------------

**STATS_SAMPLE_PAGES** specifies the number of index pages to sample when cardinality and other statistics for an indexed column are estimated. DSC will delete this attribute during migration.

**Input**

.. code-block::

   CREATE TABLE `public`.`runoob_alter_test`(
       `dataType1` int NOT NULL AUTO_INCREMENT,
       `dataType2` DOUBLE(20,8),
       `dataType3` TEXT NOT NULL,
       PRIMARY KEY(`dataType1`)
   ) ENGINE=InnoDB,STATS_SAMPLE_PAGES=25;

   ALTER TABLE runoob_alter_test STATS_SAMPLE_PAGES 100;
   ALTER TABLE runoob_alter_test STATS_SAMPLE_PAGES=100;

**Output**

.. code-block::

   CREATE TABLE "public"."runoob_alter_test"
   (
     "datatype1" SERIAL NOT NULL,
     "datatype2" FLOAT(20),
     "datatype3" TEXT NOT NULL,
     PRIMARY KEY ("datatype1")
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype1");

UNION
-----

**UNION** is a table creation parameter of the MERGE storage engine. Creating a table using this keyword is similar to creating a common view. The created table logically combines the data of multiple tables that are specified by UNION. DSC migrates this feature to the view creation statement in GaussDB.

**Input**

.. code-block::

   CREATE TABLE t1 (
       a INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
       message CHAR(20)
   ) ENGINE=MyISAM;

   CREATE TABLE t2 (
       a INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
       message CHAR(20)
   ) ENGINE=MyISAM;

   CREATE TABLE total (
       a INT NOT NULL AUTO_INCREMENT,
       message CHAR(20))
   ENGINE=MERGE UNION=(t1,t2) INSERT_METHOD=LAST;

**Output**

.. code-block::

   CREATE TABLE t1 (
       a SERIAL NOT NULL PRIMARY KEY,
       message CHAR(20)
   )
   WITH ( ORIENTATION = ROW, COMPRESSION = NO )
   NOCOMPRESS
   DISTRIBUTE BY HASH ("a");

   CREATE TABLE t2 (
       a SERIAL NOT NULL PRIMARY KEY,
       message CHAR(20)
   )
   WITH ( ORIENTATION = ROW, COMPRESSION = NO )
   NOCOMPRESS
   DISTRIBUTE BY HASH ("a");

   CREATE VIEW total(a, message) AS
   SELECT * FROM t1
   UNION ALL
   SELECT * FROM t2;
