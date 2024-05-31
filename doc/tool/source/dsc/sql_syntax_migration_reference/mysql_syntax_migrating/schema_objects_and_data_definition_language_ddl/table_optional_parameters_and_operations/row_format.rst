:original_name: dws_16_0148.html

.. _dws_16_0148:

.. _en-us_topic_0000001819416233:

ROW_FORMAT
==========

**ROW_FORMAT** defines the physical format in which the rows are stored. Row format choices vary depending on the storage engine used for the table. If you specify a row format that is not supported by the storage engine that is used for the table, the table will be created using that storage engine's default row format. If **ROW_FORMAT** is set to **DEFAULT**, the value will be migrated to **SET NOCOMPRESS**. If **ROW_FORMAT** is set to **COMPRESSED**, the value will be migrated to **SET COMPRESS**. GaussDB(DWS) supports only the preceding two **ROW_FORMAT** values. If other values are used, they will be deleted by DSC.

**Input**

.. code-block::

   CREATE TABLE `public`.`runoob_alter_test`(
       `dataType1` int NOT NULL AUTO_INCREMENT,
       `dataType2` FLOAT(10,2),
       `dataType3` DOUBLE(20,8),
       `dataType4` TEXT NOT NULL,
       PRIMARY KEY(`dataType1`)
   ) ENGINE=InnoDB;

   ## A.
   ALTER TABLE runoob_alter_test ROW_FORMAT DEFAULT;
   ALTER TABLE runoob_alter_test ROW_FORMAT=DEFAULT;

   ## B.
   ALTER TABLE runoob_alter_test ROW_FORMAT DYNAMIC;
   ALTER TABLE runoob_alter_test ROW_FORMAT=DYNAMIC;

   ## C.
   ALTER TABLE runoob_alter_test ROW_FORMAT COMPRESSED;
   ALTER TABLE runoob_alter_test ROW_FORMAT=COMPRESSED;

   ## D.
   ALTER TABLE runoob_alter_test ROW_FORMAT REDUNDANT;
   ALTER TABLE runoob_alter_test ROW_FORMAT=REDUNDANT;

   ## E.
   ALTER TABLE runoob_alter_test ROW_FORMAT COMPACT;
   ALTER TABLE runoob_alter_test ROW_FORMAT=COMPACT;

**Output**

.. code-block::

   CREATE TABLE "public"."runoob_alter_test"
   (
     "datatype1" SERIAL NOT NULL,
     "datatype2" REAL,
     "datatype3" DOUBLE PRECISION,
     "datatype4" TEXT NOT NULL,
     PRIMARY KEY ("datatype1")
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype1");

   -- A.
   ALTER TABLE "public"."runoob_alter_test" SET NOCOMPRESS;
   ALTER TABLE "public"."runoob_alter_test" SET NOCOMPRESS;

   -- B.

   -- C.
   ALTER TABLE "public"."runoob_alter_test" SET COMPRESS;
   ALTER TABLE "public"."runoob_alter_test" SET COMPRESS;

   -- D.

   -- E.
