:original_name: dws_16_0143.html

.. _dws_16_0143:

.. _en-us_topic_0000001813438728:

MIN_ROWS
========

**MIN_ROWS** indicates the minimum number of rows that can be stored in a table. This attribute will be deleted during migration using DSC.

**Input**

::

   CREATE TABLE `public`.`runoob_alter_test`(
       `dataType1` int NOT NULL AUTO_INCREMENT,
       `dataType2` DOUBLE(20,8),
       `dataType3` TEXT NOT NULL,
       PRIMARY KEY(`dataType1`)
   );
   ALTER TABLE runoob_alter_test MIN_ROWS 10000;
   ALTER TABLE runoob_alter_test MIN_ROWS=10000;

**Output**

::

   CREATE TABLE "public"."runoob_alter_test"
   (
     "datatype1" SERIAL NOT NULL,
     "datatype2" DOUBLE PRECISION,
     "datatype3" TEXT NOT NULL,
     PRIMARY KEY ("datatype1")
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype1");
