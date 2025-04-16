:original_name: dws_16_0149.html

.. _dws_16_0149:

.. _en-us_topic_0000001813439120:

STATS_AUTO_RECALC
=================

**STATS_AUTO_RECALC** specifies whether to automatically recalculate persistent statistics for an InnoDB table. GaussDB(DWS) does not support this attribute, which will be deleted by DSC during migration.

**Input**

::

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

::

   CREATE TABLE "public"."runoob_alter_test"
   (
     "runoob_id" VARCHAR(120),
     "runoob_title" VARCHAR(400) NOT NULL,
     "runoob_author" VARCHAR(160) NOT NULL,
     "submission_date" VARCHAR(120)
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("runoob_id");

   -- A.

   -- B.

   -- C.
