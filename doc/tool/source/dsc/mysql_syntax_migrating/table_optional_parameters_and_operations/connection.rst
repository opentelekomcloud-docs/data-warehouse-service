:original_name: dws_16_0130.html

.. _dws_16_0130:

.. _en-us_topic_0000001813438980:

CONNECTION
==========

GaussDB(DWS) does not support table definition modification using this attribute. DSC will delete the attribute during migration.

.. caution::

   In MySQL, the keyword **CONNECTION** is used as a referenced, external data source. Currently, DSC cannot completely migrate the feature of **CONNECTION**. So as a workaround, it simply deletes the keyword.

**Input**

::

   CREATE TABLE `public`.`runoob_alter_test`(
       `dataType1` int NOT NULL AUTO_INCREMENT,
       `dataType2` DOUBLE(20,8),
       `dataType3` TEXT NOT NULL,
       `dataType4` YEAR NOT NULL DEFAULT '2018',
       PRIMARY KEY(`dataType1`)
   );

   ALTER TABLE runoob_alter_test CONNECTION 'hello';
   ALTER TABLE runoob_alter_test CONNECTION='hello';

**Output**

::

   CREATE TABLE "public"."runoob_alter_test"
   (
     "datatype1" SERIAL NOT NULL,
     "datatype2" DOUBLE PRECISION,
     "datatype3" TEXT NOT NULL,
     "datatype4" SMALLINT NOT NULL DEFAULT '2018',
     PRIMARY KEY ("datatype1")
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype1");
