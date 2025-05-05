:original_name: dws_16_0150.html

.. _dws_16_0150:

.. _en-us_topic_0000001813598704:

STATS_PERSISTENT
================

In MySQL, **STATS_PERSISTENT** specifies whether to enable persistence statistics for an InnoDB table. The **CREATE TABLE** and **ALTER TABLE** statements can be used to enable persistence statistics. DSC will delete this attribute during migration.

**Input**

::

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

::

   CREATE TABLE "public"."runoob_alter_test"
   (
     "datatype1" SERIAL NOT NULL,
     "datatype2" DOUBLE PRECISION,
     "datatype3" TEXT NOT NULL,
     PRIMARY KEY ("datatype1")
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype1");

   -- A.

   -- B.

   -- C.
