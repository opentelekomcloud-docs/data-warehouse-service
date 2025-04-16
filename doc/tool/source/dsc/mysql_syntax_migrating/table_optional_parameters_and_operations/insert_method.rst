:original_name: dws_16_0139.html

.. _dws_16_0139:

.. _en-us_topic_0000001813599012:

INSERT_METHOD
=============

**INSERT_METHOD** specifies the table into which the row should be inserted. Use a value of **FIRST** or **LAST** to have inserts go to the first or last table, or a value of **NO** to prevent inserts. DSC will delete this attribute during migration.

**Input**

::

   CREATE TABLE `public`.`runoob_alter_test`(
       `dataType1` int NOT NULL AUTO_INCREMENT,
       `dataType2` DOUBLE(20,8),
       `dataType3` TEXT NOT NULL,
       PRIMARY KEY(`dataType1`)
   ) INSERT_METHOD=LAST;

   ALTER TABLE runoob_alter_test INSERT_METHOD NO;
   ALTER TABLE runoob_alter_test INSERT_METHOD=NO;
   ALTER TABLE runoob_alter_test INSERT_METHOD FIRST;
   ALTER TABLE runoob_alter_test INSERT_METHOD=FIRST;
   ALTER TABLE runoob_alter_test INSERT_METHOD LAST;
   ALTER TABLE runoob_alter_test INSERT_METHOD=LAST;

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
