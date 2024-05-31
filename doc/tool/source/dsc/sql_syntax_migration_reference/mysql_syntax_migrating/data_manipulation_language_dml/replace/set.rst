:original_name: dws_16_0203.html

.. _dws_16_0203:

.. _en-us_topic_0000001772536572:

SET
===

MySQL REPLACE supports the use of SET settings, which the Migration tool will convert.

**Input**

.. code-block::

   replace INTO `runoob_datatype_test` VALUES (100, 100, 100, 0, 1);
   replace INTO `runoob_datatype_test` VALUES (100.23, 100.25, 100.26, 0.12,1.5);
   replace INTO `runoob_datatype_test` (dataType_numeric,dataType_numeric1) VALUES (100.23, 100.25);
   replace INTO `runoob_datatype_test` (dataType_numeric,dataType_numeric1,dataType_numeric2) VALUES (100.23, 100.25, 2.34);
   replace into runoob_datatype_test set dataType_numeric=23.1, dataType_numeric4 = 25.12 ;

**Output**

.. code-block::

   INSERT INTO "public"."runoob_datatype_test"  VALUES (100,100,100,0,1);
   INSERT INTO "public"."runoob_datatype_test"  VALUES (100.23,100.25,100.26,0.12,1.5);
   INSERT INTO "public"."runoob_datatype_test" ("datatype_numeric","datatype_numeric1") VALUES (100.23,100.25);
   INSERT INTO "public"."runoob_datatype_test" ("datatype_numeric","datatype_numeric1","datatype_numeric2") VALUES (100.23,100.25,2.34);
   INSERT INTO "public"."runoob_datatype_test" ("datatype_numeric","datatype_numeric4") VALUES (23.1,25.12);
