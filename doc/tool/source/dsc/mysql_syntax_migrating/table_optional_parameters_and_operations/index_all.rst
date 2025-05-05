:original_name: dws_16_0138.html

.. _dws_16_0138:

INDEX_ALL
=========

In the ADB, **index_all='Y'** is used to create the full-column index. GaussDB(DWS) does not support table definition modification using this attribute. DSC will delete the attribute during migration.

**Input**

::

   DROP TABLE IF EXISTS unsupport_parse_test;
   CREATE TABLE `unsupport_parse_test` (
       `username` int,
       `update` timestamp not null default current_timestamp on update current_timestamp ,
        clustered key clustered_key(shopid ASC, datetype ASC)
   )index_ALL = 'Y';
   DROP TABLE IF EXISTS unsupport_parse_test;

**Output**

::

   DROP TABLE IF EXISTS "public"."unsupport_parse_test";
   CREATE TABLE "public"."unsupport_parse_test" (
     "username" INTEGER,
     "update" TIMESTAMP WITH TIME ZONE NOT NULL DEFAULT CURRENT_TIMESTAMP
   ) WITH (ORIENTATION = ROW, COMPRESSION = NO) NOCOMPRESS DISTRIBUTE BY HASH ("username");
   DROP TABLE IF EXISTS "public"."unsupport_parse_test";
