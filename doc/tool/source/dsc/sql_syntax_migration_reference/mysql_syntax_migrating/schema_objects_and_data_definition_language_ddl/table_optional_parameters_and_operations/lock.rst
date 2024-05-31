:original_name: dws_16_0141.html

.. _dws_16_0141:

.. _en-us_topic_0000001772696188:

LOCK
====

GaussDB(DWS) does not support the **ALTER TABLE** *tbName* **LOCK** statement of MySQL, which will be deleted by DSC during migration.

**Input**

.. code-block::

   CREATE TABLE IF NOT EXISTS `runoob_alter_test`(
       `dataType1` int NOT NULL AUTO_INCREMENT,
       `dataType2` FLOAT(10),
       `dataType4` TEXT NOT NULL,
       `dataType5` YEAR NOT NULL DEFAULT '2018',
       `dataType6` DATETIME NOT NULL,
       `dataType7` CHAR NOT NULL DEFAULT '',
       `dataType8` VARCHAR(50),
       `dataType9` VARCHAR(50) NOT NULL DEFAULT '',
       `dataType10` TIME NOT NULL DEFAULT '10:20:59',
       PRIMARY KEY(`dataType1`)
   )ENGINE=InnoDB DEFAULT CHARSET=utf8;

   ## A.
   ALTER TABLE runoob_alter_test LOCK DEFAULT;

   ## B.
   ALTER TABLE runoob_alter_test LOCK=DEFAULT;

   ## C.
   ALTER TABLE runoob_alter_test LOCK EXCLUSIVE;

   ## D.
   ALTER TABLE runoob_alter_test LOCK=EXCLUSIVE;

**Output**

.. code-block::

   CREATE TABLE IF NOT EXISTS "public"."runoob_alter_test"
   (
     "datatype1" SERIAL NOT NULL,
     "datatype2" REAL,
     "datatype4" TEXT NOT NULL,
     "datatype5" SMALLINT NOT NULL DEFAULT '2018',
     "datatype6" TIMESTAMP WITHOUT TIME ZONE NOT NULL,
     "datatype7" CHAR(4) NOT NULL DEFAULT '',
     "datatype8" VARCHAR(200),
     "datatype9" VARCHAR(200) NOT NULL DEFAULT '',
     "datatype10" TIME WITHOUT TIME ZONE NOT NULL DEFAULT '10:20:59',
     PRIMARY KEY ("datatype1")
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype1");

   -- A.

   -- B.

   -- C.

   -- D.
