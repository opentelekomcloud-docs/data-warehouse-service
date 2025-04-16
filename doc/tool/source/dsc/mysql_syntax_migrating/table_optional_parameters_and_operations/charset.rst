:original_name: dws_16_0125.html

.. _dws_16_0125:

.. _en-us_topic_0000001813439112:

CHARSET
=======

**CHARSET** specifies the default character set for a table. GaussDB(DWS) does not support table definition modification using this attribute. DSC will delete the keyword during migration.

**Input**

::

   CREATE TABLE `public`.`runoob_tbl_test`(
       `runoob_id` VARCHAR(30),
       `runoob_title` VARCHAR(100) NOT NULL,
       `runoob_author` VARCHAR(40) NOT NULL,
       `submission_date` VARCHAR(30)
   )DEFAULT CHARSET=utf8;

**Output**

::

   CREATE TABLE "public"."runoob_tbl_test"
   (
     "runoob_id" VARCHAR(120),
     "runoob_title" VARCHAR(400) NOT NULL,
     "runoob_author" VARCHAR(160) NOT NULL,
     "submission_date" VARCHAR(120)
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("runoob_id");
