:original_name: dws_16_0123.html

.. _dws_16_0123:

.. _en-us_topic_0000001813438948:

AVG_ROW_LENGTH
==============

In MySQL, **AVG_ROW_LENGTH** indicates the average length of each row. This is not supported by GaussDB(DWS) and is deleted by DSC during the migration.

**Input**

::

   CREATE TABLE `public`.`runoob_tbl_test`(
       `runoob_id` VARCHAR(30),
       `runoob_title` VARCHAR(100) NOT NULL,
       `runoob_author` VARCHAR(40) NOT NULL,
       `submission_date` VARCHAR(30)
   )AVG_ROW_LENGTH=10000;

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
