:original_name: dws_16_0110.html

.. _dws_16_0110:

.. _en-us_topic_0000001772536480:

String Types
============

Overview
--------

MySQL interprets length specifications in character column definitions in character units. This applies to the CHAR, VARCHAR, and TEXT types. DSC supports the following type conversions:

Type Mapping
------------

.. table:: **Table 1** String type mapping

   +-----------------------+-----------------------+-----------------------+
   | MySQL String Type     | MySQL INPUT           | GaussDB(DWS) OUTPUT   |
   +=======================+=======================+=======================+
   | CHAR                  | CHAR[(0)]             | CHAR[(1)]             |
   |                       |                       |                       |
   |                       | CHAR[(n)]             | CHAR[(4n)]            |
   +-----------------------+-----------------------+-----------------------+
   | CHARACTER             | CHARACTER[(0)]        | CHAR[(1)]             |
   |                       |                       |                       |
   |                       | CHARACTER[(n)]        | CHAR[(4n)]            |
   +-----------------------+-----------------------+-----------------------+
   | NCHAR                 | NCHAR[(0)]            | CHAR[(1)]             |
   |                       |                       |                       |
   |                       | NCHAR[(n)]            | CHAR[(4n)]            |
   +-----------------------+-----------------------+-----------------------+
   | LONGTEXT              | LONGTEXT              | TEXT                  |
   +-----------------------+-----------------------+-----------------------+
   | MEDIUMTEXT            | MEDIUMTEXT            | TEXT                  |
   +-----------------------+-----------------------+-----------------------+
   | TEXT                  | TEXT                  | TEXT                  |
   +-----------------------+-----------------------+-----------------------+
   | TINYTEXT              | TINYTEXT              | TEXT                  |
   +-----------------------+-----------------------+-----------------------+
   | VARCHAR               | VARCHAR[(0)]          | VARCHAR[(1)]          |
   |                       |                       |                       |
   |                       | VARCHAR[(n)]          | VARCHAR[(4n)]         |
   +-----------------------+-----------------------+-----------------------+
   | NVARCHAR              | NVARCHAR[(0)]         | VARCHAR[(1)]          |
   |                       |                       |                       |
   |                       | NVARCHAR[(n)]         | VARCHAR[(4n)]         |
   +-----------------------+-----------------------+-----------------------+
   | CHARACTE VARYING      | CHARACTE VARYING      | VARCHAR               |
   +-----------------------+-----------------------+-----------------------+

.. note::

   -  During CHAR/CHARACTER/NCHAR conversion, if the precision is less than or equal to 0, it is converted to CHAR(1). If the precision is greater than 0, it is converted to a precision level four times that of the CHAR type.
   -  During VARCHAR/NVARCHAR conversion, if the precision is less than or equal to 0, it is converted to VARCHAR(1). If the precision is greater than 0, it is converted to a precision four times that of the VARCHAR type.

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

   CREATE TABLE IF NOT EXISTS "public"."runoob_datatype_test"
   (
     "datatype_1" CHAR NOT NULL,
     "datatype_2" CHAR(1) NOT NULL,
     "datatype_3" CHAR(1020) NOT NULL
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

   CREATE TABLE IF NOT EXISTS "public"."runoob_datatype_test"
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

   CREATE TABLE IF NOT EXISTS "public"."runoob_datatype_test"
   (
     "datatype_1" VARCHAR(1),
     "datatype_2" VARCHAR(7380)
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype_1");
