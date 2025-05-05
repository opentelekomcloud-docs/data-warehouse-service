:original_name: dws_16_0112.html

.. _dws_16_0112:

.. _en-us_topic_0000001860198993:

LOB Types
=========

Overview
--------

A BLOB is a binary large object that can hold a variable amount of data. The four BLOB types are TINYBLOB, BLOB, MEDIUMBLOB, and LONGBLOB. The only difference between these four types is the maximum length of the values they can contain. DSC supports the following type conversions:

.. note::

   The BLOB type can store images. Column storage does not support BLOB.

Type Mapping
------------

.. table:: **Table 1** LOB type mapping

   ============== =========== ===================
   MySQL LOB Type MySQL INPUT GaussDB(DWS) OUTPUT
   ============== =========== ===================
   TINYBLOB       TINYBLOB    BLOB
   BLOB           BLOB        BLOB
   MEDIUMBLOB     MEDIUMBLOB  BLOB
   LONGBLOB       LONGBLOB    BLOB
   ============== =========== ===================

**Input: [TINY|MEDIUM|LONG]BLOB**

::

   CREATE TABLE IF NOT EXISTS `runoob_dataType_test`(
       `dataType_1` BIGINT,
       `dataType_2` TINYBLOB,
       `dataType_3` BLOB,
       `dataType_4` MEDIUMBLOB,
       `dataType_5` LONGBLOB
   );

**Output**

::

   CREATE TABLE IF NOT EXISTS "public"."runoob_datatype_test"
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
