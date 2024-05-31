:original_name: dws_16_0124.html

.. _dws_16_0124:

BLOCK_SIZE
==========

In ADB, this parameter specifies the number of values stored in each block in columnar storage, which is also the minimum I/O unit. GaussDB(DWS does not support table definition modification using this attribute. DSC will delete the keyword during migration.

**Input**

.. code-block::

   drop table if exists unsupport_parse_test;
   create table `unsupport_parse_test` (
       `username` int,
       `update` timestamp not null default current_timestamp on update current_timestamp ,
        clustered key clustered_key(shopid ASC, datetype ASC)
   )BLOCK_SIZE = 1024 index_ALL = 'y';
   drop table if exists unsupport_parse_test;

**Output**

.. code-block::

   DROP TABLE IF EXISTS "public"."unsupport_parse_test";
   CREATE TABLE "public"."unsupport_parse_test" (
     "username" INTEGER,
     "update" TIMESTAMP WITH TIME ZONE NOT NULL DEFAULT CURRENT_TIMESTAMP
   ) WITH (ORIENTATION = ROW, COMPRESSION = NO) NOCOMPRESS DISTRIBUTE BY HASH ("username");
   DROP TABLE IF EXISTS "public"."unsupport_parse_test";
