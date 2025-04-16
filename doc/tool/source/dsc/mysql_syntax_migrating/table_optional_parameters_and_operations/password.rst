:original_name: dws_16_0147.html

.. _dws_16_0147:

.. _en-us_topic_0000001860318933:

PASSWORD
========

In MySQL, **PASSWORD** indicates the user password. GaussDB(DWS) does not support this attribute, which will be deleted by DSC during migration.

**Input**

::

   CREATE TABLE `public`.`runoob_alter_test`(
       `dataType1` int NOT NULL AUTO_INCREMENT,
       `dataType2` DOUBLE(20,8),
       `dataType3` TEXT NOT NULL,
       PRIMARY KEY(`dataType1`)
   );
   ALTER TABLE runoob_alter_test PASSWORD 'HELLO';

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
