:original_name: dws_16_0140.html

.. _dws_16_0140:

.. _en-us_topic_0000001860318505:

KEY_BLOCK_SIZE
==============

**KEY_BLOCK_SIZE** choices vary depending on the storage engine used for a table. For MyISAM tables, **KEY_BLOCK_SIZE** optionally specifies the size in bytes to be used for index key blocks. For InnoDB tables, **KEY_BLOCK_SIZE** specifies the page size in kilobytes to be used for compressed InnoDB tables. GaussDB(DWS) does not support this attribute, which will be deleted by DSC during migration.

**Input**

::

   CREATE TABLE `public`.`runoob_tbl_test`(
       `runoob_id` VARCHAR(30),
       `runoob_title` VARCHAR(100) NOT NULL,
       `runoob_author` VARCHAR(40) NOT NULL,
       `submission_date` VARCHAR(30)
   ) ENGINE=MyISAM KEY_BLOCK_SIZE=8;

   ALTER TABLE runoob_tbl_test ENGINE=InnoDB;
   ALTER TABLE runoob_tbl_test KEY_BLOCK_SIZE=0;

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
