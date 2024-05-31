:original_name: dws_16_0114.html

.. _dws_16_0114:

.. _en-us_topic_0000001772536484:

Boolean
=======

Overview
--------

MySQL supports both BOOL and BOOLEAN. DSC supports the following type conversions:

Type Mapping
------------

**Input: BOOL/BOOLEAN**

.. code-block::

   CREATE TABLE IF NOT EXISTS `runoob_dataType_test`(
       `dataType_1` INT,
       `dataType_2` BOOL,
       `dataType_3` BOOLEAN
   );

**Output**

.. code-block::

   CREATE TABLE IF NOT EXISTS "public"."runoob_datatype_test"
   (
     "datatype_1" INTEGER,
     "datatype_2" BOOLEAN,
     "datatype_3" BOOLEAN
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype_1");
