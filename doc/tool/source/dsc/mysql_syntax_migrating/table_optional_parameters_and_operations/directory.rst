:original_name: dws_16_0134.html

.. _dws_16_0134:

.. _en-us_topic_0000001813438832:

DIRECTORY
=========

**DIRECTORY** enables a tablespace to be created outside the data directory and index directory. It allows **DATA DIRECTORY** and **INDEX DIRECTORY**. GaussDB(DWS) does not support table definition modification using this attribute. DSC will delete the attribute during migration.

**Input**

::

   CREATE TABLE `public`.`runoob_tbl_test1` (
   `dataType1` int NOT NULL AUTO_INCREMENT,
   `dataType2` DOUBLE(20,8),
   PRIMARY KEY(`dataType1`)
   ) ENGINE=MYISAM DATA DIRECTORY = 'D:\\input' INDEX DIRECTORY= 'D:\\input';

   CREATE TABLE `public`.`runoob_tbl_test2` (
   `dataType1` int NOT NULL AUTO_INCREMENT,
   `dataType2` DOUBLE(20,8),
   PRIMARY KEY(`dataType1`)
   ) ENGINE=INNODB DATA DIRECTORY = 'D:\\input';

**Output**

::

   CREATE TABLE "public"."runoob_tbl_test1"
   (
     "datatype1" SERIAL NOT NULL,
     "datatype2" DOUBLE PRECISION,
     PRIMARY KEY ("datatype1")
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype1");

   CREATE TABLE "public"."runoob_tbl_test2"
   (
     "datatype1" SERIAL NOT NULL,
     "datatype2" DOUBLE PRECISION,
     PRIMARY KEY ("datatype1")
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype1");
