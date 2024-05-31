:original_name: dws_16_0162.html

.. _dws_16_0162:

.. _en-us_topic_0000001772696208:

SET|DROP COLUMN DEFAULT VALUE
=============================

In MySQL, the **COLUMN** keyword can be omitted when the **ALTER** statement is used to set the default value of a column. DSC will perform adaptation based on GaussDB(DWS) features during migration.

**Input**

.. code-block::

   CREATE TABLE IF NOT EXISTS `runoob_alter_test`(
       `dataType1` int NOT NULL AUTO_INCREMENT,
       `dataType2` FLOAT(10,2),
       `dataType3` DOUBLE(20,8),
       `dataType4` TEXT NOT NULL,
       `dataType5` YEAR NOT NULL DEFAULT '2018',
       `dataType6` DATETIME NOT NULL DEFAULT '2018-10-12 15:27:33.999999',
       `dataType7` CHAR NOT NULL DEFAULT '',
       `dataType8` VARCHAR(50),
       `dataType9` VARCHAR(50) NOT NULL DEFAULT '',
       `dataType10` TIME NOT NULL DEFAULT '10:20:59',
       PRIMARY KEY(`dataType1`)
   )ENGINE=InnoDB DEFAULT CHARSET=utf8;

   ALTER TABLE runoob_alter_test ALTER dataType2 SET DEFAULT 1;
   ALTER TABLE runoob_alter_test ALTER COLUMN dataType2 SET DEFAULT 3;
   ALTER TABLE runoob_alter_test ALTER dataType2 DROP DEFAULT;
   ALTER TABLE runoob_alter_test ALTER COLUMN dataType2 DROP DEFAULT;

**Output**

.. code-block::

   CREATE TABLE IF NOT EXISTS "public"."runoob_alter_test"
   (
     "datatype1" SERIAL NOT NULL,
     "datatype2" REAL,
     "datatype3" DOUBLE PRECISION,
     "datatype4" TEXT NOT NULL,
     "datatype5" SMALLINT NOT NULL DEFAULT '2018',
     "datatype6" TIMESTAMP WITHOUT TIME ZONE NOT NULL DEFAULT '2018-10-12 15:27:33.999999',
     "datatype7" CHAR(4) NOT NULL DEFAULT '',
     "datatype8" VARCHAR(200),
     "datatype9" VARCHAR(200) NOT NULL DEFAULT '',
     "datatype10" TIME WITHOUT TIME ZONE NOT NULL DEFAULT '10:20:59',
     PRIMARY KEY ("datatype1")
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype1");

   ALTER TABLE "public"."runoob_alter_test" ALTER COLUMN "datatype2" SET DEFAULT '1';
   ALTER TABLE "public"."runoob_alter_test" ALTER COLUMN "datatype2" SET DEFAULT '3';
   ALTER TABLE "public"."runoob_alter_test" ALTER COLUMN "datatype2" DROP DEFAULT;
   ALTER TABLE "public"."runoob_alter_test" ALTER COLUMN "datatype2" DROP DEFAULT;
