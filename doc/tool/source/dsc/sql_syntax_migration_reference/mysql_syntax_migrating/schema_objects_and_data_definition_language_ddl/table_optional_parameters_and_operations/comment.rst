:original_name: dws_16_0129.html

.. _dws_16_0129:

.. _en-us_topic_0000001772696176:

COMMENT
=======

In MySQL, **COMMENT** is a comment for a table. In GaussDB(DWS), this attribute can be used to modify table definition. During migration using DSC, additional table attribute information is added.

**Input**

.. code-block::

   CREATE TABLE `public`.`runoob_alter_test`(
       `dataType1` int NOT NULL AUTO_INCREMENT,
   `dataType2` FLOAT (10,2) COMMENT'dataType2 column',
       PRIMARY KEY(`dataType1`)
   ) comment='table comment';

   ALTER TABLE `public`.`runoob_alter_test` COMMENT 'table comment after modification';
   ALTER TABLE `public`.`runoob_alter_test` ADD INDEX age_index(dataType2) COMMENT' index';

**Output**

.. code-block::

   CREATE TABLE "public"."runoob_alter_test"
   (
     "datatype1" SERIAL NOT NULL,
   "datatype2" REAL COMMENT 'dataType2 column',
     PRIMARY KEY ("datatype1")
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype1")
   COMMENT 'Comment of the table';
   ALTER TABLE `public`.`runoob_alter_test` COMMENT 'table comment after modification';
   CREATE INDEX "age_index" ON "public"."runoob_alter_test" ("dataType2") COMMENT' index';
