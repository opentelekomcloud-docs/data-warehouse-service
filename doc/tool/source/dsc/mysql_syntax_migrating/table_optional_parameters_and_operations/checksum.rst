:original_name: dws_16_0126.html

.. _dws_16_0126:

.. _en-us_topic_0000001813438824:

CHECKSUM
========

In MySQL, **CHECKSUM** maintains a live checksum for all rows. GaussDB(DWS) does not support table definition modification using this attribute. DSC will delete the keyword during migration.

**Input**

::

   CREATE TABLE `public`.`runoob_alter_test`(
       `dataType1` int NOT NULL AUTO_INCREMENT,
       `dataType2` FLOAT(10,2),
       `dataType3` DOUBLE(20,8),
       PRIMARY KEY(`dataType1`)
   ) CHECKSUM=1;

   ALTER TABLE runoob_alter_test CHECKSUM 0;
   ALTER TABLE runoob_alter_test CHECKSUM=0;

   ALTER TABLE runoob_alter_test CHECKSUM 1;
   ALTER TABLE runoob_alter_test CHECKSUM=1;

**Output**

::

   CREATE TABLE "public"."runoob_alter_test"
   (
     "datatype1" SERIAL NOT NULL,
     "datatype2" REAL,
     "datatype3" DOUBLE PRECISION,
     PRIMARY KEY ("datatype1")
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype1");
