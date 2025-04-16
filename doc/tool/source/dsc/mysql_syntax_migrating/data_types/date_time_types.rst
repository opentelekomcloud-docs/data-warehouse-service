:original_name: dws_16_0109.html

.. _dws_16_0109:

.. _en-us_topic_0000001813439228:

Date/Time Types
===============

Overview
--------

This section describes the following date and time types: DATETIME, TIME, TIMESTAMP, and YEAR. GaussDB(DWS) does not support these types, and DSC will convert them.

Type Mapping
------------

.. table:: **Table 1** Date/Time type mapping

   ==================== ================ ==================================
   MySQL Date/Time Type MySQL INPUT      GaussDB(DWS) OUTPUT
   ==================== ================ ==================================
   DATETIME             DATETIME[(fsp)]  TIMESTAMP[(fsp)] WITHOUT TIME ZONE
   TIME                 TIME[(fsp)]      TIME[(fsp)] WITHOUT TIME ZONE
   TIMESTAMP            TIMESTAMP[(fsp)] TIMESTAMP[(fsp)] WITH TIME ZONE
   YEAR                 YEAR[(4)]        SMALLINT(4)
   ==================== ================ ==================================

.. note::

   The value of *fsp* must be in the range [0, 6]. Value **0** indicates no decimal. If this parameter is omitted, the default precision will be 0.

**Input: DATETIME**

::

   CREATE TABLE IF NOT EXISTS `runoob_dataType_test`(
       `dataType_1` DATETIME,
       `dataType_2` DATETIME(0),
       `dataType_3` DATETIME(6),
       `dataType_4` DATETIME DEFAULT NULL,
       `dataType_5` DATETIME DEFAULT '2018-10-12 15:27:33.999999'
   );

**Output**

::

   CREATE TABLE IF NOT EXISTS "public"."runoob_datatype_test"
   (
     "datatype_1" TIMESTAMP WITHOUT TIME ZONE,
     "datatype_2" TIMESTAMP(0) WITHOUT TIME ZONE,
     "datatype_3" TIMESTAMP(6) WITHOUT TIME ZONE,
     "datatype_4" TIMESTAMP WITHOUT TIME ZONE DEFAULT NULL,
     "datatype_5" TIMESTAMP WITHOUT TIME ZONE DEFAULT '2018-10-12 15:27:33.999999'
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype_1");

**Input: TIME**

::

   CREATE TABLE IF NOT EXISTS `runoob_dataType_test`(
       `dataType_1` TIME DEFAULT '20:58:10',
       `dataType_2` TIME(3) DEFAULT '20:58:10',
       `dataType_3` TIME(6) DEFAULT '20:58:10',
       `dataType_4` TIME(6) DEFAULT '2018-10-11 20:00:00',
       `dataType_5` TIME(6) DEFAULT '20:58:10.01234',
       `dataType_6` TIME(6) DEFAULT '2018-10-11 20:00:00.01234',
       `dataType_7` TIME DEFAULT NULL,
       `dataType_8` TIME(6) DEFAULT NULL,
       PRIMARY KEY (dataType_1)
   );

**Output**

::

   CREATE TABLE IF NOT EXISTS "public"."runoob_datatype_test"
   (
     "datatype_1" TIME WITHOUT TIME ZONE DEFAULT '20:58:10',
     "datatype_2" TIME(3) WITHOUT TIME ZONE DEFAULT '20:58:10',
     "datatype_3" TIME(6) WITHOUT TIME ZONE DEFAULT '20:58:10',
     "datatype_4" TIME(6) WITHOUT TIME ZONE DEFAULT '2018-10-11 20:00:00',
     "datatype_5" TIME(6) WITHOUT TIME ZONE DEFAULT '20:58:10.01234',
     "datatype_6" TIME(6) WITHOUT TIME ZONE DEFAULT '2018-10-11 20:00:00.01234',
     "datatype_7" TIME WITHOUT TIME ZONE DEFAULT NULL,
     "datatype_8" TIME(6) WITHOUT TIME ZONE DEFAULT NULL,
     PRIMARY KEY ("datatype_1")
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype_1");

**Input: TIMESTAMP**

::

   CREATE TABLE IF NOT EXISTS `runoob_dataType_test`(
      `dataType_1` TIMESTAMP,
      `dateType_4` TIMESTAMP DEFAULT '2018-10-12 15:27:33',
      `dateType_5` TIMESTAMP DEFAULT '2018-10-12 15:27:33.999999',
      `dateType_6` TIMESTAMP DEFAULT '2018-10-12 15:27:33',
      `dateType_7` TIMESTAMP DEFAULT '2018-10-12 15:27:33',
      `dataType_8` TIMESTAMP(0) DEFAULT '2018-10-12 15:27:33',
      `dateType_9` TIMESTAMP(6) DEFAULT '2018-10-12 15:27:33'
   );

**Output**

::

   CREATE TABLE IF NOT EXISTS "public"."runoob_datatype_test"
   (
     "datatype_1" TIMESTAMP WITH TIME ZONE,
     "datetype_4" TIMESTAMP WITH TIME ZONE DEFAULT '2018-10-12 15:27:33',
     "datetype_5" TIMESTAMP WITH TIME ZONE DEFAULT '2018-10-12 15:27:33.999999',
     "datetype_6" TIMESTAMP WITH TIME ZONE DEFAULT '2018-10-12 15:27:33',
     "datetype_7" TIMESTAMP WITH TIME ZONE DEFAULT '2018-10-12 15:27:33',
     "datatype_8" TIMESTAMP(0) WITH TIME ZONE DEFAULT '2018-10-12 15:27:33',
     "datetype_9" TIMESTAMP(6) WITH TIME ZONE DEFAULT '2018-10-12 15:27:33'
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype_1");

**Input: YEAR**

::

   CREATE TABLE IF NOT EXISTS `runoob_dataType_test`(
       `dataType_1` YEAR,
       `dataType_2` YEAR(4),
       `dataType_3` YEAR DEFAULT '2018',
       `dataType_4` TIME DEFAULT NULL
   );

**Output**

::

   CREATE TABLE IF NOT EXISTS "public"."runoob_datatype_test"
   (
     "datatype_1" SMALLINT,
     "datatype_2" SMALLINT,
     "datatype_3" VARCHAR(4) DEFAULT '2018',
     "datatype_4" TIME WITHOUT TIME ZONE DEFAULT NULL
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype_1");
