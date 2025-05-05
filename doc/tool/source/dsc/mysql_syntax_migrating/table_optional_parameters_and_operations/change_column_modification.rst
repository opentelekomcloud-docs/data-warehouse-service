:original_name: dws_16_0154.html

.. _dws_16_0154:

.. _en-us_topic_0000001860198661:

CHANGE (Column Modification)
============================

MySQL uses the **CHANGE** keyword to change column names and data types and set **NOT NULL** constraints. DSC will perform adaptation based on GaussDB(DWS) features during migration.

**Input**

::

   CREATE TABLE IF NOT EXISTS `runoob_alter_test`(
       `dataType0` varchar(128),
       `dataType1` bigint,
       `dataType2` bigint,
       `dataType3` bigint,
       `dataType4` bigint
   )ENGINE=InnoDB DEFAULT CHARSET=utf8;

   ## A.
   ALTER TABLE runoob_alter_test CHANGE dataType1 dataType1New VARCHAR(50);

   ## B.
   ALTER TABLE runoob_alter_test CHANGE dataType2 dataType2New VARCHAR(50) NOT NULL;

   ## C.
   ALTER TABLE runoob_alter_test CHANGE dataType3 dataType3New VARCHAR(100) FIRST;

   ## D.
   ALTER TABLE runoob_alter_test CHANGE dataType4 dataType4New VARCHAR(50) AFTER dataType1;

**Output**

::

   CREATE TABLE "public"."runoob_alter_test"
   (
     "datatype0" VARCHAR(512),
     "datatype1" BIGINT,
     "datatype2" BIGINT,
     "datatype3" BIGINT,
     "datatype4" BIGINT
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype0");

   -- A.
   ALTER TABLE "public"."runoob_alter_test" CHANGE COLUMN "datatype1" "datatype1new" VARCHAR(200) NULL DEFAULT NULL;

   -- B.
   ALTER TABLE "public"."runoob_alter_test" CHANGE COLUMN "datatype2" "datatype2new" VARCHAR(200) NOT NULL;

   -- C.
   ALTER TABLE "public"."runoob_alter_test" CHANGE COLUMN "datatype3" "datatype3new" VARCHAR(400) NULL DEFAULT NULL;

   -- D.
   ALTER TABLE "public"."runoob_alter_test" CHANGE COLUMN "datatype4" "datatype4new" VARCHAR(200) NULL DEFAULT NULL;
