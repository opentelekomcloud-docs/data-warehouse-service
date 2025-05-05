:original_name: dws_16_0164.html

.. _dws_16_0164:

Row-Store/Column-Store Table Compression
========================================

In GaussDB(DWS), only column-store tables can be compressed. Row-store tables cannot be compressed. The row-column storage compression mechanism is optimized. DSC performs adaptation based on GaussDB(DWS) features during migration.

Compression parameters:

:ref:`table.compress.mode. <en-us_topic_0000001860318481__en-us_topic_0000001434418777_li186211955102212>` If keyword **COMPRESS** is specified in **CREATE TABLE**, the compression feature will be triggered in the case of bulk **INSERT** operations. If this feature is enabled, a scan will be performed on all tuple data within the page to generate a dictionary and then the tuple data will be compressed and stored. If **NOCOMPRESS** is used, tables will not be compressed.

:ref:`table.compress.row <en-us_topic_0000001860318481__en-us_topic_0000001434418777_li7638164673410>` and :ref:`table.compress.column <en-us_topic_0000001860318481__en-us_topic_0000001434418777_li06731353153514>` determine the compression level, which determines the compression ratio and duration. Generally, the higher the level of compression, the higher the ratio, the longer the duration, vice versa. The actual compression ratio depends on the distribution mode of table data loaded.

:ref:`table.compress.level <en-us_topic_0000001860318481__en-us_topic_0000001434418777_li8585858112211>` determines the compression sublevel, which determines the table data compression ratio and duration. A compression level is divided into sublevels, providing more choices for the compression ratio and duration. As the value becomes larger, the compression ratio becomes higher and duration longer at the same compression level.

**Input example of a row-store table**

::

   DROP TABLE IF EXISTS `public`.`runoob_tbl`;
   CREATE TABLE IF NOT EXISTS `public`.`runoob_tbl`(
      `runoob_id` VARCHAR,
      `runoob_title` VARCHAR(100) NOT NULL,
      `runoob_author` VARCHAR(40) NOT NULL,
      `submission_date` VARCHAR
   )ENGINE=InnoDB DEFAULT CHARSET=utf8;

**Output example of a row-store table**

::

   DROP TABLE IF EXISTS "public"."runoob_tbl";
   CREATE TABLE IF NOT EXISTS "public"."runoob_tbl" (
     "runoob_id" VARCHAR,
     "runoob_title" VARCHAR(400) NOT NULL,
     "runoob_author" VARCHAR(160) NOT NULL,
     "submission_date" VARCHAR
   ) WITH (ORIENTATION = ROW, COMPRESSION = YES) COMPRESS DISTRIBUTE BY HASH ("runoob_id");

**Input example of a column-store table**

::

   DROP TABLE IF EXISTS `public`.`runoob_tbl`;
   CREATE TABLE IF NOT EXISTS `public`.`runoob_tbl`(
      `runoob_id` VARCHAR,
      `runoob_title` VARCHAR(100) NOT NULL,
      `runoob_author` VARCHAR(40) NOT NULL,
      `submission_date` VARCHAR
   )ENGINE=InnoDB DEFAULT CHARSET=utf8;

**Output example of a column-store table**

::

   DROP TABLE IF EXISTS "public"."runoob_tbl";
   CREATE TABLE IF NOT EXISTS "public"."runoob_tbl" (
     "runoob_id" VARCHAR,
     "runoob_title" VARCHAR(400) NOT NULL,
     "runoob_author" VARCHAR(160) NOT NULL,
     "submission_date" VARCHAR
   ) WITH (
     COMPRESSLEVEL = 1,
     ORIENTATION = COLUMN,
     COMPRESSION = LOW
   ) DISTRIBUTE BY HASH ("runoob_id");
