:original_name: dws_16_0108.html

.. _dws_16_0108:

.. _en-us_topic_0000001813438780:

Numeric Types
=============

Overview
--------

A data type is a basic data attribute. Occupied storage space and allowed operations vary according to data types. In a database, data is stored in tables, in which a data type is specified for each column. Data in the column must be of its allowed data type. The following table lists an example of converting MySQL numeric types to GaussDB(DWS) numeric types.

Type Comparison
---------------

.. table:: **Table 1** Numeric type mapping

   +-----------------------+--------------------------------------------------+-----------------------+
   | MySQL Numeric Type    | MySQL INPUT                                      | GaussDB(DWS) Output   |
   +=======================+==================================================+=======================+
   | DEC                   | DEC                                              | DECIMAL               |
   |                       |                                                  |                       |
   |                       | DEC[(M[,D])] [UNSIGNED] [ZEROFILL]               | DECIMAL[(M[,D])]      |
   +-----------------------+--------------------------------------------------+-----------------------+
   | DECIMAL               | DECIMAL[(M[,D])] [UNSIGNED] [ZEROFILL]           | DECIMAL[(M[,D])]      |
   +-----------------------+--------------------------------------------------+-----------------------+
   | DOUBLE PRECISION      | DOUBLE PRECISION                                 | DOUBLE PRECISION      |
   |                       |                                                  |                       |
   |                       | DOUBLE PRECISION [(M[,D])] [UNSIGNED] [ZEROFILL] | DOUBLE PRECISION      |
   +-----------------------+--------------------------------------------------+-----------------------+
   | DOUBLE                | DOUBLE[(M[,D])] [UNSIGNED] [ZEROFILL]            | DOUBLE PRECISION      |
   +-----------------------+--------------------------------------------------+-----------------------+
   | FIXED                 | FIXED                                            | DECIMAL               |
   |                       |                                                  |                       |
   |                       | FIXED[(M[,D])] [UNSIGNED] [ZEROFILL]             | DECIMAL[(M[,D])]      |
   +-----------------------+--------------------------------------------------+-----------------------+
   | FLOAT                 | FLOAT                                            | REAL                  |
   |                       |                                                  |                       |
   |                       | FLOAT [(M[,D])] [UNSIGNED] [ZEROFILL]            | REAL                  |
   |                       |                                                  |                       |
   |                       | FLOAT(p) [UNSIGNED] [ZEROFILL]                   | REAL                  |
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
   | REAL                  | REAL[(M[,D])]                                    | REAL/DOUBLE PRECISION |
   +-----------------------+--------------------------------------------------+-----------------------+
   | SMALLINT              | SMALLINT                                         | SMALLINT              |
   |                       |                                                  |                       |
   |                       | SMALLINT(p) [UNSIGNED] [ZEROFILL]                |                       |
   +-----------------------+--------------------------------------------------+-----------------------+
   | TINYINT               | TINYINT                                          | SMALLINT              |
   |                       |                                                  |                       |
   |                       | TINYINT(n)                                       | SMALLINT              |
   |                       |                                                  |                       |
   |                       | TINYINT(n) ZEROFILL                              | SMALLINT              |
   |                       |                                                  |                       |
   |                       | TINYINT(n) UNSIGNED ZEROFILL                     | TINYINT               |
   +-----------------------+--------------------------------------------------+-----------------------+

.. note::

   -  When the TINYINT type is converted, if it is unsigned (UNSIGNED), it is converted to TINYINT. Otherwise, it is converted to SMALLINT.
   -  The REAL type is converted to DOUBLE PRECISION by default. If **table.database.realAsFlag** in the **features-mysql.properties** configuration file is set to **true** (**false** by default), the type is converted to REAL.

**Input: TINYINT**

::

   CREATE TABLE IF NOT EXISTS `runoob_dataType_test`(
       `dataType_1` TINYINT,
       `dataType_2` TINYINT(0),
       `dataType_3` TINYINT(255),
       `dataType_4` TINYINT(255) UNSIGNED ZEROFILL,
       `dataType_5` TINYINT(255) ZEROFILL
   );

**Output**

::

   CREATE TABLE IF NOT EXISTS "public"."runoob_datatype_test"
   (
     "datatype_1" SMALLINT,
     "datatype_2" SMALLINT,
     "datatype_3" SMALLINT,
     "datatype_4" TINYINT,
     "datatype_5" SMALLINT
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype_1");
