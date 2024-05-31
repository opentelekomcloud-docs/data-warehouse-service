:original_name: dws_16_0113.html

.. _dws_16_0113:

.. _en-us_topic_0000001772696160:

Set Types
=========

Overview
--------

#. In MySQL, an ENUM is a string object with a value chosen from a list of permitted values that are enumerated explicitly in the column specification at table creation.
#. A SET is a string object that can have zero or more values, each of which must be chosen from a list of permitted values specified when the table is created.

Type Mapping
------------

.. table:: **Table 1** Set type mapping

   ============== =========== ===================
   MySQL Set Type MySQL INPUT GaussDB(DWS) OUTPUT
   ============== =========== ===================
   ENUM           ENUM        VARCHAR
   SET            SET         VARCHAR
   ============== =========== ===================

.. note::

   -  The ENUM type is converted to the VARCHAR type. The precision is four times the length of the longest field in the enumerated values. Use the CHECK() function to ensure that the input enumerated values are correct.
   -  The SET type is converted to the VARCHAR type. The precision is four times the sum of the length of each enumerated value field and the number of separators.

**Input: ENUM**

.. code-block::

   CREATE TABLE IF NOT EXISTS `runoob_dataType_test`(
        id   int(2) PRIMARY KEY,
       `dataType_17` ENUM('dws-1', 'dws-2', 'dws-3')
   );

**Output**

.. code-block::

   CREATE TABLE IF NOT EXISTS "public"."runoob_datatype_test"
   (
     "id" INTEGER(2) PRIMARY KEY,
     "datatype_17" VARCHAR(20) CHECK (dataType_17 IN('dws-1','dws-2','dws-3','', null))
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

   CREATE TABLE IF NOT EXISTS "public"."runoob_tbl_test"
   (
     "datatype_18" VARCHAR(68)
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype_18");
