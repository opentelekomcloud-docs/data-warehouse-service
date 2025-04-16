:original_name: dws_16_0120.html

.. _dws_16_0120:

ALGORITHM
=========

MySQL has extended the support for **ALTER TABLE... ALGORITHM=INSTANT**: Users can add columns instantly and delete columns instantly anywhere in a table, and evaluate row size limits when adding a column.

This is not supported by GaussDB(DWS) and is deleted by DSC during the migration.

**Input**

::

   ALTER TABLE runoob_alter_test ALGORITHM=DEFAULT;
   ALTER TABLE runoob_alter_test ALGORITHM=INPLACE;
   ALTER TABLE runoob_alter_test ALGORITHM=COPY;
   ALTER TABLE runoob_alter_test ADD COLUMN COL_18 VARCHAR(64)  DEFAULT '00', ALGORITHM=INSTANT;
   ALTER TABLE runoob_alter_test MODIFY COLUMN dataType7 BIGINT, ALGORITHM=COPY;
   ALTER TABLE `runoob_alter_test` ALGORITHM=DEFAULT, ALGORITHM=INPLACE, ALGORITHM=COPY;
   ALTER TABLE `runoob_alter_test` ADD COLUMN dataType11 INT, ALGORITHM=DEFAULT, ALGORITHM=INPLACE, ALGORITHM=COPY;
   ALTER TABLE runoob_alter_test CHANGE COLUMN dataType11 dataType12 SMALLINT ,ALGORITHM=INPLACE, ALGORITHM=COPY;
   ALTER TABLE runoob_alter_test ALGORITHM=INPLACE, ALGORITHM=COPY, DROP COLUMN dataType12;

**Output**

::

   ALTER TABLE
     "public"."runoob_alter_test"
   ADD
     COLUMN "col_18" VARCHAR(256) DEFAULT '00';
   ALTER TABLE
     "public"."runoob_alter_test" MODIFY "datatype7" BIGINT NULL DEFAULT NULL;
   ALTER TABLE
     "public"."runoob_alter_test"
   ADD
     COLUMN "datatype11" INTEGER;
   ALTER TABLE
     "public"."runoob_alter_test" CHANGE COLUMN "datatype11" "datatype12" SMALLINT NULL DEFAULT NULL;
   ALTER TABLE
     "public"."runoob_alter_test" DROP COLUMN "datatype12" RESTRICT;
   DROP TABLE IF EXISTS "public"."runoob_alter_test";
