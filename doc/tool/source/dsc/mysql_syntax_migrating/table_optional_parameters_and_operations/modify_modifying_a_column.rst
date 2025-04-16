:original_name: dws_16_0158.html

.. _dws_16_0158:

.. _en-us_topic_0000001860318461:

MODIFY (Modifying a Column)
===========================

MySQL uses the **MODIFY** keyword to change column data types and set **NOT NULL** constraints. DSC will perform adaptation based on GaussDB(DWS) features during migration.

**Input**

::

   CREATE TABLE IF NOT EXISTS `runoob_alter_test`(
       `dataType0` varchar(100),
       `dataType1` bigint,
       `dataType2` bigint,
       `dataType3` bigint
   )ENGINE=InnoDB DEFAULT CHARSET=utf8;

   ## A.
   ALTER TABLE runoob_alter_test MODIFY dataType1 smallint;

   ## B.
   ALTER TABLE runoob_alter_test MODIFY dataType1 smallint NOT NULL;

   ## C.
   ALTER TABLE runoob_alter_test MODIFY dataType1 smallint NOT NULL FIRST;

   ## D.
   ALTER TABLE runoob_alter_test MODIFY dataType1 smallint NOT NULL AFTER dataType3;

**Output**

::

   CREATE TABLE "public"."runoob_alter_test"
   (
     "datatype0" VARCHAR(400),
     "datatype1" BIGINT,
     "datatype2" BIGINT,
     "datatype3" BIGINT
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype0");

   -- A.
   ALTER TABLE "public"."runoob_alter_test" MODIFY "datatype1" SMALLINT NULL DEFAULT NULL;

   -- B.
   ALTER TABLE "public"."runoob_alter_test" MODIFY "datatype1" SMALLINT NOT NULL;

   -- C.
   ALTER TABLE "public"."runoob_alter_test" MODIFY "datatype1" SMALLINT NOT NULL;

   -- D.
   ALTER TABLE "public"."runoob_alter_test" MODIFY "datatype1" SMALLINT NOT NULL;
