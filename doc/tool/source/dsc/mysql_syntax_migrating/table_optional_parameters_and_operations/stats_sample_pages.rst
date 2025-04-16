:original_name: dws_16_0151.html

.. _dws_16_0151:

.. _en-us_topic_0000001813598664:

STATS_SAMPLE_PAGES
==================

**STATS_SAMPLE_PAGES** specifies the number of index pages to sample when cardinality and other statistics for an indexed column are estimated. DSC will delete this attribute during migration.

**Input**

::

   CREATE TABLE `public`.`runoob_alter_test`(
       `dataType1` int NOT NULL AUTO_INCREMENT,
       `dataType2` DOUBLE(20,8),
       `dataType3` TEXT NOT NULL,
       PRIMARY KEY(`dataType1`)
   ) ENGINE=InnoDB,STATS_SAMPLE_PAGES=25;

   ALTER TABLE runoob_alter_test STATS_SAMPLE_PAGES 100;
   ALTER TABLE runoob_alter_test STATS_SAMPLE_PAGES=100;

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
