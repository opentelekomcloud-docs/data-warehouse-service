:original_name: dws_07_8311.html

.. _dws_07_8311:

Comments
========

To comment out a single line, MySQL uses # or --, and GaussDB(DWS) uses --. DSC will replace # with -- for commenting out a single line during migration.

**Input**

.. code-block::

   ## comment sample create a table
   CREATE TABLE IF NOT EXISTS `public`.`runoob_tbl`(
      `runoob_id` VARCHAR,
      `runoob_title` VARCHAR(100) NOT NULL,
      `runoob_author` VARCHAR(40) NOT NULL,
      `submission_date` VARCHAR
   )ENGINE=InnoDB DEFAULT CHARSET=utf8;

**Output**

.. code-block::

   -- comment sample create a table
   CREATE TABLE "public"."runoob_tbl"
   (
     "runoob_id" VARCHAR,
     "runoob_title" VARCHAR(100) NOT NULL,
     "runoob_author" VARCHAR(40) NOT NULL,
     "submission_date" VARCHAR
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("runoob_id");
