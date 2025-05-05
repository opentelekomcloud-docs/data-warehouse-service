:original_name: dws_16_0127.html

.. _dws_16_0127:

CLUSTERED KEY
=============

It is a clustered index in ADB that defines the sorting sequence in a table. The logical sequence of key values in a clustered index determines the physical sequence of rows in the table. GaussDB(DWS) does not support table definition modification using this attribute. DSC will delete the keyword during migration.

**Input**

::

   DROP TABLE IF EXISTS unsupport_parse_test;
   CREATE TABLE `unsupport_parse_test` (
       `username` int,
        clustered key clustered_key(shopid ASC, datetype ASC)
   );
   DROP TABLE IF EXISTS unsupport_parse_test;

**Output**

::

   DROP TABLE IF EXISTS "public"."unsupport_parse_test";
   CREATE TABLE "public"."unsupport_parse_test" (
     "username" INTEGER
   ) WITH (ORIENTATION = ROW, COMPRESSION = NO) NOCOMPRESS DISTRIBUTE BY HASH ("username");
   DROP TABLE IF EXISTS "public"."unsupport_parse_test";
