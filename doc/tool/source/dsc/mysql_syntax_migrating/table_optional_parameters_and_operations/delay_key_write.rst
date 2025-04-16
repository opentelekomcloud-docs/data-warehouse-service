:original_name: dws_16_0132.html

.. _dws_16_0132:

.. _en-us_topic_0000001860318729:

DELAY_KEY_WRITE
===============

**DELAY_KEY_WRITE** is valid only for MyISAM tables. It is used to delay updates until the table is closed. GaussDB(DWS) does not support table definition modification using this attribute. DSC will delete the attribute during migration.

**Input**

::

   CREATE TABLE `public`.`runoob_tbl_test`(
       `runoob_id` VARCHAR(30),
       `runoob_title` VARCHAR(100) NOT NULL,
       `runoob_author` VARCHAR(40) NOT NULL,
       `submission_date` VARCHAR(30)
   ) ENGINE=MyISAM, DELAY_KEY_WRITE=0;

   ALTER TABLE `public`.`runoob_tbl_test6` DELAY_KEY_WRITE=1;

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
