:original_name: dws_16_0137.html

.. _dws_16_0137:

IF NOT EXISTS
=============

DSC supports the conversion of IF NOT EXISTS, which is reserved during the migration.

**Input**

.. code-block::

   drop table if exists `categories`;
   CREATE TABLE IF NOT EXISTS `categories`
   (
       `CategoryID` tinyint(5) unsigned NOT NULL PRIMARY KEY AUTO_INCREMENT,
       `CategoryName` varchar(15) CHARACTER SET utf8 COLLATE utf8_unicode_ci NOT NULL DEFAULT '' ,
       `Description` mediumtext CHARACTER SET utf8 COLLATE utf8_unicode_ci NOT NULL ,
       `Picture` varchar(50) CHARACTER SET utf8 COLLATE utf8_unicode_ci NOT NULL DEFAULT '',
       UNIQUE   (`CategoryID`)
   )ENGINE=InnoDB AUTO_INCREMENT=9 DEFAULT CHARSET=latin1;
   drop table if exists `categories`;

**Output**

.. code-block::

   DROP TABLE IF EXISTS "public"."categories";
   CREATE TABLE IF NOT EXISTS "public"."categories" (
     "categoryid" SMALLSERIAL NOT NULL PRIMARY KEY,
     "categoryname" VARCHAR(60) NOT NULL DEFAULT '',
     "description" TEXT NOT NULL,
     "picture" VARCHAR(200) NOT NULL DEFAULT ''
   ) WITH (ORIENTATION = ROW, COMPRESSION = NO) NOCOMPRESS DISTRIBUTE BY HASH ("categoryid");
   DROP TABLE IF EXISTS "public"."categories";
