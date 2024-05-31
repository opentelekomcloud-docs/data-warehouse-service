:original_name: dws_16_0156.html

.. _dws_16_0156:

.. _en-us_topic_0000001819416241:

DROP (Table Deletion)
=====================

Both GaussDB(DWS) and MySQL support using the DROP statement to delete tables. However, GaussDB(DWS) does not support the **RESTRICT \| CASCADE** keyword in the DROP statement. DSC will delete the keywords during migration.

**Input**

.. code-block::

   CREATE TABLE IF NOT EXISTS `public`.`express_elb_server`(
      `runoob_id` VARCHAR(10),
      `runoob_title` VARCHAR(100) NOT NULL,
      `runoob_author` VARCHAR(40) NOT NULL,
      `submission_date` VARCHAR(10)
   )ENGINE=InnoDB DEFAULT CHARSET=utf8;
   DROP TABLE  `public`.`express_elb_server` RESTRICT;

**Output**

.. code-block::

   CREATE TABLE IF NOT EXISTS "public"."express_elb_server"
   (
     "runoob_id" VARCHAR(40),
     "runoob_title" VARCHAR(400) NOT NULL,
     "runoob_author" VARCHAR(160) NOT NULL,
     "submission_date" VARCHAR(40)
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("runoob_id");
   DROP TABLE "public"."express_elb_server";
