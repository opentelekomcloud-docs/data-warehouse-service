:original_name: dws_16_0128.html

.. _dws_16_0128:

.. _en-us_topic_0000001860318605:

COLLATE
=======

In MySQL, **COLLATE** specifies a default database sorting rule. GaussDB(DWS) does not support table definition modification using this attribute. DSC will delete the keyword during migration.

**Input**

::

   CREATE TABLE `public`.`runoob_tbl_test`(
       `runoob_id` VARCHAR(30),
       `runoob_title` VARCHAR(100) NOT NULL,
       `runoob_author` VARCHAR(40) NOT NULL,
       `submission_date` VARCHAR(30)
   ) COLLATE=utf8_general_ci;

   ALTER TABLE `public`.`runoob_tbl_test` COLLATE=utf8mb4_bin;

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
