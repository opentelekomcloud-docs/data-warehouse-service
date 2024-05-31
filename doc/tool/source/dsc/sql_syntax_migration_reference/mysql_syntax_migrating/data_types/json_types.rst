:original_name: dws_16_0116.html

.. _dws_16_0116:

JSON Types
==========

Overview
--------

The JSON data type can be used to store JavaScript Object Notation (JSON) data. DSC supports the following type conversions:

Type Mapping
------------

**Input Example (JSON)**

.. code-block::

   CREATE TABLE IF NOT EXISTS `runoob_dataType_test`(
       `dataType_1` INT,
       `dataType_2` VARCHAR,
       `dataType_3` JSON
   );
   ALTER TABLE `runoob_dataType_test` ADD COLUMN `dataType_4` JSON NOT NULL;
   ALTER TABLE `runoob_dataType_test` CHANGE COLUMN `dataType_4` `dataType_5` JSON NOT NULL;

**Output**

.. code-block::

   CREATE TABLE IF NOT EXISTS "public"."runoob_datatype_test"
   (
     "datatype_1" INTEGER,
     "datatype_2" VARCHAR,
     "datatype_3" JSONB
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype_1");
   ALTER TABLE
     "public"."runoob_datatype_test"
   ADD
     COLUMN "datatype_4" JSONB;
   ALTER TABLE
    "public"."runoob_datatype_test" CHANGE COLUMN "datatype_4" "datatype_5" JSONB;
