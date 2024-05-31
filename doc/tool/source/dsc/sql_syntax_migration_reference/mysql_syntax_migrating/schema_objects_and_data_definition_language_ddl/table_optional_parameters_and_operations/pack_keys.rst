:original_name: dws_16_0145.html

.. _dws_16_0145:

.. _en-us_topic_0000001819336201:

PACK_KEYS
=========

In MySQL, **PACK_KEYS** specifies the index compression mode in the MyISAM storage engine. GaussDB(DWS) does not support this attribute, which will be deleted by DSC during migration.

**Input**

.. code-block::

   CREATE TABLE `public`.`runoob_alter_test`(
       `dataType1` int NOT NULL AUTO_INCREMENT,
       `dataType2` DOUBLE(20,8),
       `dataType3` TEXT NOT NULL,
       PRIMARY KEY(`dataType1`)
   ) ENGINE=MyISAM PACK_KEYS=1;

   ##A
   ALTER TABLE runoob_alter_test PACK_KEYS 0;
   ALTER TABLE runoob_alter_test PACK_KEYS=0;

   ##B
   ALTER TABLE runoob_alter_test PACK_KEYS 1;
   ALTER TABLE runoob_alter_test PACK_KEYS=1;

   ##C
   ALTER TABLE runoob_alter_test PACK_KEYS DEFAULT;
   ALTER TABLE runoob_alter_test PACK_KEYS=DEFAULT;

**Output**

.. code-block::

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

   --A

   --B

   --C
