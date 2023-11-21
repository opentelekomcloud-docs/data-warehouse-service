:original_name: dws_07_6831.html

.. _dws_07_6831:

Basic Data Types
================

Overview
--------

MySQL supports a number of basic data types, including numeric, date/time, string (character), LOB, set, binary, and Boolean types. GaussDB(DWS) does not support some basic MySQL data types, precision settings of some MySQL data types, or some MySQL keywords such as **UNSIGNED** and **ZEROFILL**. DSC performs migration based on GaussDB support.

A data type is a basic data attribute. Occupied storage space and allowed operations vary according to data types. In a database, data is stored in tables, in which a data type is specified for each column. Data in the column must be of its allowed data type. For details of each types description, see :ref:`Table 1 <en-us_topic_0000001188681004__en-us_topic_0238518439_en-us_topic_0237362176_table1858310435332>`

.. _en-us_topic_0000001188681004__en-us_topic_0238518439_en-us_topic_0237362176_table1858310435332:

.. table:: **Table 1** Basic description of data types

   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Data Types                        | Basic Description                                                                                                                                                                                                                                                                                                                                                                  |
   +===================================+====================================================================================================================================================================================================================================================================================================================================================================================+
   | Numeric Types                     | For details, see :ref:`Numeric Types <en-us_topic_0000001188681004__en-us_topic_0238518439_en-us_topic_0237362176_section13238133084213>`.                                                                                                                                                                                                                                         |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Date/Time Types                   | This document describes the following **date/time** types: **DATETIME**, **TIME**, **TIMESTAMP**, and **YEAR**. GaussDB(DWS) does not support these types, and DSC will convert them. For details, see :ref:`Date/Time Types <en-us_topic_0000001188681004__en-us_topic_0238518439_en-us_topic_0237362176_section1756223318421>`.                                                  |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | String Types                      | MySQL interprets length specifications in character column definitions in character units. This applies to the CHAR, VARCHAR, and TEXT types. For details, see :ref:`String Types <en-us_topic_0000001188681004__en-us_topic_0238518439_en-us_topic_0237362176_section18212113419426>`.                                                                                            |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Spatial Data Types                | MySQL has spatial data types corresponding to the OpenGIS class. For details, see :ref:`Spatial Data Types <en-us_topic_0000001188681004__en-us_topic_0238518439_en-us_topic_0237362176_section2333153511421>`.                                                                                                                                                                    |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | LOB Types                         | A BLOB is a binary large object that can hold a variable amount of data. The four BLOB types are TINYBLOB, BLOB, MEDIUMBLOB, and LONGBLOB. The only difference between these four types is the maximum length of the values they can contain. For details, see :ref:`LOB Types <en-us_topic_0000001188681004__en-us_topic_0238518439_en-us_topic_0237362176_section135036144211>`. |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Set Types                         | #. In MySQL, an ENUM is a string object with a value chosen from a list of permitted values that are enumerated explicitly in the column specification at table creation.                                                                                                                                                                                                          |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                                    |
   |                                   | #. A SET is a string object that can have zero or more values, each of which must be chosen from a list of permitted values specified when the table is created.                                                                                                                                                                                                                   |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                                    |
   |                                   |    For details, see :ref:`Set Types <en-us_topic_0000001188681004__en-us_topic_0238518439_en-us_topic_0237362176_section1818193619424>`.                                                                                                                                                                                                                                           |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Boolean Types                     | MySQL supports both BOOL and BOOLEAN. For details, see :ref:`Boolean Types <en-us_topic_0000001188681004__en-us_topic_0238518439_en-us_topic_0237362176_section1540624204220>`.                                                                                                                                                                                                    |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Binary Types                      | #. In MySQL, the BIT data type is used to store bit values. A type of BIT(*M*) enables storage of *M*-bit values. *M* can range from 1 to 64.                                                                                                                                                                                                                                      |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                                    |
   |                                   | #. MySQL BINARY and VARBINARY types are similar to CHAR and VARCHAR, except that they contain binary strings rather than non-binary strings.                                                                                                                                                                                                                                       |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                                    |
   |                                   |    For details, see :ref:`Binary Types <en-us_topic_0000001188681004__en-us_topic_0238518439_en-us_topic_0237362176_section14212134405>`.                                                                                                                                                                                                                                          |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _en-us_topic_0000001188681004__en-us_topic_0238518439_en-us_topic_0237362176_section13238133084213:

Numeric Types
-------------

.. table:: **Table 2** Numeric type mapping

   +-----------------------+--------------------------------------------------+-----------------------+
   | MySQL Numeric Type    | MySQL Input                                      | GaussDB(DWS) Output   |
   +=======================+==================================================+=======================+
   | DEC                   | DEC                                              | DECIMAL               |
   |                       |                                                  |                       |
   |                       | DEC[(M[,D])] [UNSIGNED] [ZEROFILL]               | DECIMAL[(M[,D])]      |
   +-----------------------+--------------------------------------------------+-----------------------+
   | DECIMAL               | DECIMAL[(M[,D])] [UNSIGNED] [ZEROFILL]           | DECIMAL[(M[,D])]      |
   +-----------------------+--------------------------------------------------+-----------------------+
   | DOUBLE PRECISION      | DOUBLE PRECISION                                 | FLOAT                 |
   |                       |                                                  |                       |
   |                       | DOUBLE PRECISION [(M[,D])] [UNSIGNED] [ZEROFILL] | FLOAT[(M)]            |
   +-----------------------+--------------------------------------------------+-----------------------+
   | DOUBLE                | DOUBLE[(M[,D])] [UNSIGNED] [ZEROFILL]            | FLOAT[(M)]            |
   +-----------------------+--------------------------------------------------+-----------------------+
   | FIXED                 | FIXED                                            | DECIMAL               |
   |                       |                                                  |                       |
   |                       | FIXED[(M[,D])] [UNSIGNED] [ZEROFILL]             | DECIMAL[(M[,D])]      |
   +-----------------------+--------------------------------------------------+-----------------------+
   | FLOAT                 | FLOAT                                            | FLOAT                 |
   |                       |                                                  |                       |
   |                       | FLOAT [(M[,D])] [UNSIGNED] [ZEROFILL]            | FLOAT[(M)]            |
   |                       |                                                  |                       |
   |                       | FLOAT(p) [UNSIGNED] [ZEROFILL]                   | FLOAT(p)              |
   +-----------------------+--------------------------------------------------+-----------------------+
   | INT                   | INT                                              | INTEGER               |
   |                       |                                                  |                       |
   |                       | INT(p) [UNSIGNED] [ZEROFILL]                     | INTEGER(p)            |
   +-----------------------+--------------------------------------------------+-----------------------+
   | INTEGER               | INTEGER                                          | INTEGER               |
   |                       |                                                  |                       |
   |                       | INTEGER(p) [UNSIGNED] [ZEROFILL]                 | INTEGER(p)            |
   +-----------------------+--------------------------------------------------+-----------------------+
   | MEDIUMINT             | MEDIUMINT                                        | INTEGER               |
   |                       |                                                  |                       |
   |                       | MEDIUMINT(p) [UNSIGNED] [ZEROFILL]               | INTEGER(p)            |
   +-----------------------+--------------------------------------------------+-----------------------+
   | NUMERIC               | NUMERIC                                          | DECIMAL               |
   |                       |                                                  |                       |
   |                       | NUMERIC [(M[,D])] [UNSIGNED] [ZEROFILL]          | DECIMAL[(M[,D])]      |
   +-----------------------+--------------------------------------------------+-----------------------+
   | REAL                  | REAL[(M[,D])]                                    | FLOAT[(M)]            |
   +-----------------------+--------------------------------------------------+-----------------------+
   | SMALLINT              | SMALLINT                                         | SMALLINT              |
   |                       |                                                  |                       |
   |                       | SMALLINT(p) [UNSIGNED] [ZEROFILL]                |                       |
   +-----------------------+--------------------------------------------------+-----------------------+
   | TINYINT               | TINYINT                                          | TINYINT               |
   |                       |                                                  |                       |
   |                       | TINYINT(n)                                       | TINYINT               |
   |                       |                                                  |                       |
   |                       | TINYINT(n) ZEROFILL                              | TINYINT               |
   |                       |                                                  |                       |
   |                       | TINYINT(n) UNSIGNED ZEROFILL                     | SMALLINT              |
   +-----------------------+--------------------------------------------------+-----------------------+

**Input: TINYINT**

.. code-block::

   CREATE TABLE IF NOT EXISTS `runoob_dataType_test`(
       `dataType_1` TINYINT,
       `dataType_2` TINYINT(0),
       `dataType_3` TINYINT(255),
       `dataType_4` TINYINT(255) UNSIGNED ZEROFILL,
       `dataType_5` TINYINT(255) ZEROFILL
   );

**Output**

.. code-block::

   CREATE TABLE "public"."runoob_datatype_test"
   (
     "datatype_1" TINYINT,
     "datatype_2" TINYINT,
     "datatype_3" TINYINT,
     "datatype_4" SMALLINT,
     "datatype_5" TINYINT
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype_1");

.. _en-us_topic_0000001188681004__en-us_topic_0238518439_en-us_topic_0237362176_section1756223318421:

Date/Time Types
---------------

.. table:: **Table 3** Date/Time type mapping

   ==================== ================ ==================================
   MySQL Date/Time Type MySQL Input      GaussDB(DWS) Output
   ==================== ================ ==================================
   DATETIME             DATETIME[(fsp)]  TIMESTAMP[(fsp)] WITHOUT TIME ZONE
   TIME                 TIME[(fsp)]      TIME[(fsp)] WITHOUT TIME ZONE
   TIMESTAMP            TIMESTAMP[(fsp)] TIMESTAMP[(fsp)] WITH TIME ZONE
   YEAR                 YEAR[(4)]        VARCHAR(4)
   ==================== ================ ==================================

.. note::

   The value of *fsp* must be in the range [0, 6]. Value **0** indicates no decimal. If this parameter is omitted, the default precision will be 0.

**Input: DATETIME**

.. code-block::

   CREATE TABLE IF NOT EXISTS `runoob_dataType_test`(
       `dataType_1` DATETIME,
       `dataType_2` DATETIME(0),
       `dataType_3` DATETIME(6),
       `dataType_4` DATETIME DEFAULT NULL,
       `dataType_5` DATETIME DEFAULT '2018-10-12 15:27:33.999999'
   );

**Output**

.. code-block::

   CREATE TABLE "public"."runoob_datatype_test"
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

.. code-block::

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

.. code-block::

   CREATE TABLE "public"."runoob_datatype_test"
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

.. code-block::

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

.. code-block::

   CREATE TABLE "public"."runoob_datatype_test"
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

.. code-block::

   CREATE TABLE IF NOT EXISTS `runoob_dataType_test`(
       `dataType_1` YEAR,
       `dataType_2` YEAR(4),
       `dataType_3` YEAR DEFAULT '2018',
       `dataType_4` TIME DEFAULT NULL
   );

**Output**

.. code-block::

   CREATE TABLE "public"."runoob_datatype_test"
   (
     "datatype_1" VARCHAR(4),
     "datatype_2" VARCHAR(4),
     "datatype_3" VARCHAR(4) DEFAULT '2018',
     "datatype_4" TIME WITHOUT TIME ZONE DEFAULT NULL
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype_1");

.. _en-us_topic_0000001188681004__en-us_topic_0238518439_en-us_topic_0237362176_section18212113419426:

String Types
------------

.. table:: **Table 4** String type mapping

   ================= ============ ===================
   MySQL String Type MySQL Input  GaussDB(DWS) Output
   ================= ============ ===================
   CHAR              CHAR[(0)]    CHAR[(1)]
   LONGTEXT          LONGTEXT     TEXT
   MEDIUMTEXT        MEDIUMTEXT   TEXT
   TEXT              TEXT         TEXT
   TINYTEXT          TINYTEXT     TEXT
   VARCHAR           VARCHAR[(0)] VARCHAR[(1)]
   ================= ============ ===================

**Input: CHAR**

In MySQL, the length of a CHAR column is fixed to the length that you declare when you create the table. The length can be any value from 0 to 255. When CHAR values are stored, they are right-padded with spaces to the specified length.

.. code-block::

   CREATE TABLE IF NOT EXISTS `runoob_dataType_test`(
      `dataType_1` CHAR NOT NULL,
      `dataType_2` CHAR(0) NOT NULL,
      `dataType_3` CHAR(255) NOT NULL
   );

**Output**

.. code-block::

   CREATE TABLE "public"."runoob_datatype_test"
   (
     "datatype_1" CHAR NOT NULL,
     "datatype_2" CHAR(1) NOT NULL,
     "datatype_3" CHAR(255) NOT NULL
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype_1");

**Input: [LONG|MEDIUM|TINY]TEXT**

.. code-block::

   CREATE TABLE IF NOT EXISTS `runoob_dataType_test`(
       `dataType_1` LONGTEXT,
       `dataType_2` MEDIUMTEXT,
       `dataType_3` TEXT,
       `dataType_4` TINYTEXT
   );

**Output**

.. code-block::

   CREATE TABLE "public"."runoob_datatype_test"
   (
     "datatype_1" TEXT,
     "datatype_2" TEXT,
     "datatype_3" TEXT,
     "datatype_4" TEXT
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype_1");

**Input: VARCHAR**

In MySQL, values in VARCHAR columns are variable-length strings. The length can be any value from 0 to 65,535.

.. code-block::

   CREATE TABLE IF NOT EXISTS `runoob_dataType_test`(
       `dataType_1` VARCHAR(0),
       `dataType_2` VARCHAR(1845)
   );

**Output**

.. code-block::

   CREATE TABLE "public"."runoob_datatype_test"
   (
     "datatype_1" VARCHAR(1),
     "datatype_2" VARCHAR(1845)
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype_1");

.. _en-us_topic_0000001188681004__en-us_topic_0238518439_en-us_topic_0237362176_section2333153511421:

Spatial Data Types
------------------

.. table:: **Table 5** Spatial type mapping

   ================== ================== ===================
   MySQL Spatial Type MySQL Input        GaussDB(DWS) Output
   ================== ================== ===================
   GEOMETRY           GEOMETRY           CIRCLE
   POINT              POINT              POINT
   LINESTRING         LINESTRING         POLYGON
   POLYGON            POLYGON            POLYGON
   MULTIPOINT         MULTIPOINT         BOX
   MULTILINESTRING    MULTILINESTRING    BOX
   MULTIPOLYGON       MULTIPOLYGON       POLYGON
   GEOMETRYCOLLECTION GEOMETRYCOLLECTION CIRCLE
   ================== ================== ===================

-  GEOMETRY can store geometry values of any type. The other single-value types (POINT, LINESTRING, and POLYGON) restrict their values to a particular geometry type.

-  GEOMETRYCOLLECTION can store a collection of objects of any type. The other collection types (MULTIPOINT, MULTILINESTRING, MULTIPOLYGON, and GEOMETRYCOLLECTION) restrict collection members to those having a particular geometry type.

   **Input**

   .. code-block::

      CREATE TABLE `t_geo_test2`  (
        `id` int(11) NOT NULL,
        `name` varchar(255),
        `geometry_1` geometry NOT NULL,
        `point_1` point NOT NULL,
        `linestring_1` linestring NOT NULL,
        `polygon_1` polygon NOT NULL,
        `multipoint_1` multipoint NOT NULL,
        `multilinestring_1` multilinestring NOT NULL,
        `multipolygon_1` multipolygon NOT NULL,
        `geometrycollection_1` geometrycollection NOT NULL,
        PRIMARY KEY (`id`) USING BTREE
      ) ENGINE = InnoDB;

   **Output**

   .. code-block::

      CREATE TABLE "public"."t_geo_test2"
      (
        "id" INTEGER(11) NOT NULL,
        "name" VARCHAR(255),
        "geometry_1" CIRCLE NOT NULL,
        "point_1" POINT NOT NULL,
        "linestring_1" POLYGON NOT NULL,
        "polygon_1" POLYGON NOT NULL,
        "multipoint_1" BOX NOT NULL,
        "multilinestring_1" BOX NOT NULL,
        "multipolygon_1" POLYGON NOT NULL,
        "geometrycollection_1" CIRCLE NOT NULL,
        PRIMARY KEY ("id")
      )
        WITH ( ORIENTATION = ROW, COMPRESSION = NO )
        NOCOMPRESS
        DISTRIBUTE BY HASH ("id");

.. _en-us_topic_0000001188681004__en-us_topic_0238518439_en-us_topic_0237362176_section135036144211:

LOB Types
---------

.. table:: **Table 6** LOB type mapping

   ============== =========== ===================
   MySQL LOB Type MySQL Input GaussDB(DWS) Output
   ============== =========== ===================
   TINYBLOB       TINYBLOB    BLOB
   BLOB           BLOB        BLOB
   MEDIUMBLOB     MEDIUMBLOB  BLOB
   LONGBLOB       LONGBLOB    BLOB
   ============== =========== ===================

**Input: [TINY|MEDIUM|LONG]BLOB**

.. code-block::

   CREATE TABLE IF NOT EXISTS `runoob_dataType_test`(
       `dataType_1` BIGINT,
       `dataType_2` TINYBLOB,
       `dataType_3` BLOB,
       `dataType_4` MEDIUMBLOB,
       `dataType_5` LONGBLOB
   );

**Output**

.. code-block::

   CREATE TABLE "public"."runoob_datatype_test"
   (
     "datatype_1" BIGINT,
     "datatype_2" BLOB,
     "datatype_3" BLOB,
     "datatype_4" BLOB,
     "datatype_5" BLOB
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype_1");

.. _en-us_topic_0000001188681004__en-us_topic_0238518439_en-us_topic_0237362176_section1818193619424:

Set Types
---------

.. table:: **Table 7** Set type mapping

   ============== =========== ===================
   MySQL Set Type MySQL Input GaussDB(DWS) Output
   ============== =========== ===================
   ENUM           ENUM        VARCHAR(14)
   SET            SET         VARCHAR(14)
   ============== =========== ===================

**Input: ENUM**

.. code-block::

   CREATE TABLE IF NOT EXISTS `runoob_dataType_test`(
        id   int(2) PRIMARY KEY,
       `dataType_17` ENUM('dws-1', 'dws-2', 'dws-3')
   );

**Output**

.. code-block::

   CREATE TABLE "public"."runoob_datatype_test"
   (
     "id" INTEGER(2) PRIMARY KEY,
     "datatype_17" VARCHAR(14)
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("id");

**Input: SET**

.. code-block::

   CREATE TABLE IF NOT EXISTS `runoob_tbl_test`(
       `dataType_18` SET('dws-1', 'dws-2', 'dws-3')
   );

**Output**

.. code-block::

   CREATE TABLE "public"."runoob_tbl_test"
   (
     "datatype_18" VARCHAR(14)
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype_18");

.. _en-us_topic_0000001188681004__en-us_topic_0238518439_en-us_topic_0237362176_section1540624204220:

Boolean Types
-------------

**Input: BOOL/BOOLEAN**

.. code-block::

   CREATE TABLE IF NOT EXISTS `runoob_dataType_test`(
       `dataType_1` INT,
       `dataType_2` BOOL,
       `dataType_3` BOOLEAN
   );

**Output**

.. code-block::

   CREATE TABLE "public"."runoob_datatype_test"
   (
     "datatype_1" INTEGER,
     "datatype_2" BOOLEAN,
     "datatype_3" BOOLEAN
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype_1");

.. _en-us_topic_0000001188681004__en-us_topic_0238518439_en-us_topic_0237362176_section14212134405:

Binary Types
------------

.. table:: **Table 8** Binary type mapping

   ================= ============== ===================
   MySQL Binary Type MySQL Input    GaussDB(DWS) Output
   ================= ============== ===================
   BIT[(M)]          BIT[(M)]       BIT[(M)]
   BINARY[(M)]       BINARY[(M)]    BYTEA
   VARBINARY[(M)]    VARBINARY[(M)] BYTEA
   ================= ============== ===================

**Input: BIT**

.. code-block::

   CREATE TABLE IF NOT EXISTS `runoob_dataType_test`(
       `dataType_1` INT,
       `dataType_2` BIT(1),
       `dataType_3` BIT(64)
   );

**Output**

.. code-block::

   CREATE TABLE "public"."runoob_datatype_test"
   (
     "datatype_1" INTEGER,
     "datatype_2" BIT(1),
     "datatype_3" BIT(64)
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype_1");

**Input: [VAR]BINARY**

.. code-block::

   CREATE TABLE IF NOT EXISTS `runoob_dataType_test`(
       `dataType_1` INT,
       `dataType_2` BINARY,
       `dataType_3` BINARY(0),
       `dataType_4` BINARY(255),
       `dataType_5` VARBINARY(0),
       `dataType_6` VARBINARY(6553)
   );

**Output**

.. code-block::

   CREATE TABLE "public"."runoob_datatype_test"
   (
     "datatype_1" INTEGER,
     "datatype_2" BYTEA,
     "datatype_3" BYTEA,
     "datatype_4" BYTEA,
     "datatype_5" BYTEA,
     "datatype_6" BYTEA
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype_1");
