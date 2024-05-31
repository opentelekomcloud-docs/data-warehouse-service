:original_name: dws_16_0159.html

.. _dws_16_0159:

.. _en-us_topic_0000001772536528:

TRUNCATE (Table Deletion)
=========================

In MySQL, the **TABLE** keyword can be omitted when the **TRUNCATE** statement is used to delete table data. GaussDB(DWS) does not support this usage. In addition, DSC will add **CONTINUE IDENTITY RESTRICT** during **TRUNCATE** migration.

**Input**

.. code-block::

   TRUNCATE TABLE `public`.`test_create_table01`;
   TRUNCATE TEST_CREATE_TABLE01;

**Output**

.. code-block::

   TRUNCATE TABLE "public"."test_create_table01" CONTINUE IDENTITY RESTRICT;
   TRUNCATE TABLE "public"."test_create_table01" CONTINUE IDENTITY RESTRICT;
