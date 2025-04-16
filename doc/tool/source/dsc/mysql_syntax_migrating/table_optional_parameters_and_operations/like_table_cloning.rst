:original_name: dws_16_0157.html

.. _dws_16_0157:

.. _en-us_topic_0000001860198729:

LIKE (Table Cloning)
====================

In the MySQL database, you can use the **CREATE TABLE .. LIKE ..** method to clone the old table structure to create a table. It is also supported by GaussDB(DWS). DSC will add additional table attribute information during migration.

**Input**

::

   CREATE TABLE IF NOT EXISTS `public`.`runoob_tbl_old`(
       `dataType_1` YEAR,
       `dataType_2` YEAR(4),
       `dataType_3` YEAR DEFAULT '2018',
       `dataType_4` TIME DEFAULT NULL
   );

   CREATE TABLE `runoob_tbl` (like `runoob_tbl_old`);

**Output**

::

   CREATE TABLE IF NOT EXISTS "public"."runoob_tbl_old"
   (
     "datatype_1" SMALLINT,
     "datatype_2" SMALLINT,
     "datatype_3" SMALLINT DEFAULT '2018',
     "datatype_4" TIME WITHOUT TIME ZONE DEFAULT NULL
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype_1");

   CREATE TABLE "public"."runoob_tbl"( LIKE "public"."runoob_tbl_old"
      INCLUDING COMMENTS INCLUDING CONSTRAINTS INCLUDING DEFAULTS INCLUDING INDEXES INCLUDING STORAGE);
