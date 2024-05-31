:original_name: dws_16_0115.html

.. _dws_16_0115:

.. _en-us_topic_0000001819416197:

Binary Types
============

Overview
--------

-  In MySQL, the BIT data type is used to store bit values. A type of BIT(*M*) enables storage of *M*-bit values. *M* can range from 1 to 64.
-  MySQL BINARY and VARBINARY types are similar to CHAR and VARCHAR, except that they contain binary strings rather than non-binary strings.

Type Mapping
------------

.. table:: **Table 1** Binary type mapping

   ================= ============== ===================
   MySQL Binary Type MySQL INPUT    GaussDB(DWS) OUTPUT
   ================= ============== ===================
   BIT[(M)]          BIT[(M)]       BIT[(M)]
   BINARY[(M)]       BINARY[(M)]    BYTEA
   CHAR BYTE[(M)]    BINARY[(M)]    BYTEA
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

   CREATE TABLE IF NOT EXISTS "public"."runoob_datatype_test"
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

   CREATE TABLE IF NOT EXISTS "public"."runoob_datatype_test"
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
