:original_name: dws_07_6833.html

.. _dws_07_6833:

Table Operations
================

This section contains the migration syntax for migrating MySQL table operation. The migration syntax decides how the supported keywords/features are migrated.

For details, see the following topics:

-  :ref:`LIKE (Table Cloning) <en-us_topic_0000001234200611__en-us_topic_0238518441_en-us_topic_0237362250_section135275238112>`
-  :ref:`ADD|DROP COLUMN <en-us_topic_0000001234200611__en-us_topic_0238518441_en-us_topic_0237362250_section1277716559165>`
-  :ref:`MODIFY (Column Modification) <en-us_topic_0000001234200611__en-us_topic_0238518441_en-us_topic_0237362250_section1625145611618>`
-  :ref:`CHANGE (Column Modification) <en-us_topic_0000001234200611__en-us_topic_0238518441_en-us_topic_0237362250_section185127566162>`
-  :ref:`SET|DROP COLUMN DEFAULT VALUE <en-us_topic_0000001234200611__en-us_topic_0238518441_en-us_topic_0237362250_section777435610162>`
-  :ref:`DROP (Table Deletion) <en-us_topic_0000001234200611__en-us_topic_0238518441_en-us_topic_0237362250_section179989561167>`
-  :ref:`TRUNCATE (Table Deletion) <en-us_topic_0000001234200611__en-us_topic_0238518441_en-us_topic_0237362250_section142431157181611>`
-  :ref:`LOCK <en-us_topic_0000001234200611__en-us_topic_0238518441_en-us_topic_0237362250_section11440115719160>`
-  :ref:`RENAME (Table Renaming) <en-us_topic_0000001234200611__en-us_topic_0238518441_en-us_topic_0237362250_section367216577163>`

.. _en-us_topic_0000001234200611__en-us_topic_0238518441_en-us_topic_0237362250_section135275238112:

LIKE (Table Cloning)
--------------------

MySQL databases support **CREATE TABLE**. **LIKE** is a method with which a table is created by cloning the old table structure, and this method is supported by GaussDB(DWS). DSC will add additional table attribute information during migration.

**Input**

.. code-block::

   CREATE TABLE IF NOT EXISTS `public`.`runoob_tbl_old`(
       `dataType_1` YEAR,
       `dataType_2` YEAR(4),
       `dataType_3` YEAR DEFAULT '2018',
       `dataType_4` TIME DEFAULT NULL
   );

   CREATE TABLE `runoob_tbl` (like `runoob_tbl_old`);

**Output**

.. code-block::

   CREATE TABLE "public"."runoob_tbl_old"
   (
     "datatype_1" VARCHAR(4),
     "datatype_2" VARCHAR(4),
     "datatype_3" VARCHAR(4) DEFAULT '2018',
     "datatype_4" TIME WITHOUT TIME ZONE DEFAULT NULL
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype_1");

   CREATE TABLE "public"."runoob_tbl"( LIKE "public"."runoob_tbl_old"
      INCLUDING COMMENTS INCLUDING CONSTRAINTS INCLUDING DEFAULTS INCLUDING INDEXES INCLUDING STORAGE);

.. _en-us_topic_0000001234200611__en-us_topic_0238518441_en-us_topic_0237362250_section1277716559165:

ADD|DROP COLUMN
---------------

MySQL and GaussDB(DWS) use different statements for adding and deleting columns. DSC will perform adaptation based on GaussDB features during migration.

.. caution::

   GaussDB does not support the update of sequence numbers in table definitions. Temporarily, DSC does not support the complete migration of the FIRST and AFTER features. So as a workaround, it simply deletes the keywords.

**Input**

.. code-block::

   CREATE TABLE IF NOT EXISTS `runoob_alter_test`(
       `dataType1` int NOT NULL AUTO_INCREMENT,
       `dataType2` FLOAT(10,2),
       `dataType3` DOUBLE(20,8),
       `dataType4` TEXT NOT NULL,
       `dataType5` YEAR NOT NULL DEFAULT '2018',
       `dataType6` DATETIME NOT NULL DEFAULT '2018-10-12 15:27:33.999999',
       `dataType7` CHAR NOT NULL DEFAULT '',
       `dataType8` VARCHAR(50),
       `dataType9` VARCHAR(50) NOT NULL DEFAULT '',
       `dataType10` TIME NOT NULL DEFAULT '10:20:59',
       PRIMARY KEY(`dataType1`)
   )ENGINE=InnoDB DEFAULT CHARSET=utf8;

   ## A.
   ALTER TABLE runoob_alter_test ADD dataType1_1 INT NOT NULL AFTER dataType1;
   ALTER TABLE runoob_alter_test DROP dataType1_1;

   ## B.
   ALTER TABLE runoob_alter_test ADD dataType1_1 INT NOT NULL FIRST;
   ALTER TABLE runoob_alter_test DROP dataType1_1;

   ## C.
   ALTER TABLE runoob_alter_test ADD COLUMN dataType1_1 INT NOT NULL AFTER dataType2;
   ALTER TABLE runoob_alter_test DROP COLUMN dataType1_1;

   ## D.
   ALTER TABLE runoob_alter_test ADD COLUMN dataType1_1 INT NOT NULL FIRST;
   ALTER TABLE runoob_alter_test DROP COLUMN dataType1_1;

   ## E.
   ALTER TABLE runoob_alter_test ADD COLUMN(dataType1_1 INT NOT NULL, dataType1_2 VARCHAR(200) NOT NULL);
   ALTER TABLE runoob_alter_test DROP COLUMN dataType1_1, DROP COLUMN dataType1_2;

**Output**

.. code-block::

   CREATE TABLE "public"."runoob_alter_test"
   (
     "datatype1" SERIAL NOT NULL,
     "datatype2" FLOAT(10),
     "datatype3" FLOAT(20),
     "datatype4" TEXT NOT NULL,
     "datatype5" VARCHAR(4) NOT NULL DEFAULT '2018',
     "datatype6" TIMESTAMP WITHOUT TIME ZONE NOT NULL DEFAULT '2018-10-12 15:27:33.999999',
     "datatype7" CHAR NOT NULL DEFAULT '',
     "datatype8" VARCHAR(50),
     "datatype9" VARCHAR(50) NOT NULL DEFAULT '',
     "datatype10" TIME WITHOUT TIME ZONE NOT NULL DEFAULT '10:20:59',
     PRIMARY KEY ("datatype1")
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype1");

   -- A.
   ALTER TABLE "public"."runoob_alter_test" ADD COLUMN "datatype1_1" INTEGER NOT NULL;
   ALTER TABLE "public"."runoob_alter_test" DROP COLUMN "datatype1_1" RESTRICT;

   -- B.
   ALTER TABLE "public"."runoob_alter_test" ADD COLUMN "datatype1_1" INTEGER NOT NULL;
   ALTER TABLE "public"."runoob_alter_test" DROP COLUMN "datatype1_1" RESTRICT;

   -- C.
   ALTER TABLE "public"."runoob_alter_test" ADD COLUMN "datatype1_1" INTEGER NOT NULL;
   ALTER TABLE "public"."runoob_alter_test" DROP COLUMN "datatype1_1" RESTRICT;

   -- D.
   ALTER TABLE "public"."runoob_alter_test" ADD COLUMN "datatype1_1" INTEGER NOT NULL;
   ALTER TABLE "public"."runoob_alter_test" DROP COLUMN "datatype1_1" RESTRICT;

   -- E.
   ALTER TABLE "public"."runoob_alter_test" ADD COLUMN "datatype1_1" VARCHAR(200) NOT NULL, ADD COLUMN "datatype1_2" VARCHAR(200) NOT NULL;
   ALTER TABLE "public"."runoob_alter_test" DROP COLUMN "datatype1_1" RESTRICT, DROP COLUMN "datatype1_2" RESTRICT;

.. _en-us_topic_0000001234200611__en-us_topic_0238518441_en-us_topic_0237362250_section1625145611618:

MODIFY (Column Modification)
----------------------------

MySQL uses the **MODIFY** keyword to change column data types and set **NOT NULL** constraints. DSC will perform adaptation based on GaussDB features during migration.

**Input**

.. code-block::

   CREATE TABLE IF NOT EXISTS `runoob_alter_test`(
       `dataType0` varchar(100),
       `dataType1` bigint,
       `dataType2` bigint,
       `dataType3` bigint
   )ENGINE=InnoDB DEFAULT CHARSET=utf8;

   ## A.
   ALTER TABLE runoob_alter_test MODIFY dataType1 smallint;

   ## B.
   ALTER TABLE runoob_alter_test MODIFY dataType1 smallint NOT NULL;

   ## C.
   ALTER TABLE runoob_alter_test MODIFY dataType1 smallint NOT NULL FIRST;

   ## D.
   ALTER TABLE runoob_alter_test MODIFY dataType1 smallint NOT NULL AFTER dataType3;

**Output**

.. code-block::

   CREATE TABLE "public"."runoob_alter_test"
   (
     "datatype0" VARCHAR(100),
     "datatype1" BIGINT,
     "datatype2" BIGINT,
     "datatype3" BIGINT
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype0");

   -- A.
   ALTER TABLE "public"."runoob_alter_test" ALTER COLUMN "datatype1" SET DATA TYPE SMALLINT;

   -- B.
   ALTER TABLE "public"."runoob_alter_test" ALTER COLUMN "datatype1" SET DATA TYPE SMALLINT, ALTER COLUMN "datatype1" SET NOT NULL;

   -- C.
   ALTER TABLE "public"."runoob_alter_test" ALTER COLUMN "datatype1" SET DATA TYPE SMALLINT, ALTER COLUMN "datatype1" SET NOT NULL;

   -- D.
   ALTER TABLE "public"."runoob_alter_test" ALTER COLUMN "datatype1" SET DATA TYPE SMALLINT, ALTER COLUMN "datatype1" SET NOT NULL;

.. _en-us_topic_0000001234200611__en-us_topic_0238518441_en-us_topic_0237362250_section185127566162:

CHANGE (Column Modification)
----------------------------

MySQL uses the **CHANGE** keyword to change column names and data types and set **NOT NULL** constraints. DSC will perform adaptation based on GaussDB features during migration.

**Input**

.. code-block::

   CREATE TABLE IF NOT EXISTS `runoob_alter_test`(
       `dataType0` varchar(128),
       `dataType1` bigint,
       `dataType2` bigint,
       `dataType3` bigint,
       `dataType4` bigint
   )ENGINE=InnoDB DEFAULT CHARSET=utf8;

   ## A.
   ALTER TABLE runoob_alter_test CHANGE dataType1 dataType1New VARCHAR(50);

   ## B.
   ALTER TABLE runoob_alter_test CHANGE dataType2 dataType2New VARCHAR(50) NOT NULL;

   ## C.
   ALTER TABLE runoob_alter_test CHANGE dataType3 dataType3New VARCHAR(100) FIRST;

   ## D.
   ALTER TABLE runoob_alter_test CHANGE dataType4 dataType4New VARCHAR(50) AFTER dataType1;

**Output**

.. code-block::

   CREATE TABLE "public"."runoob_alter_test"
   (
     "datatype0" VARCHAR(128),
     "datatype1" BIGINT,
     "datatype2" BIGINT,
     "datatype3" BIGINT,
     "datatype4" BIGINT
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype0");

   -- A.
   ALTER TABLE "public"."runoob_alter_test" RENAME COLUMN "datatype1" TO "datatype1new";
   ALTER TABLE "public"."runoob_alter_test" ALTER COLUMN "datatype1new" SET DATA TYPE VARCHAR(50);

   -- B.
   ALTER TABLE "public"."runoob_alter_test" RENAME COLUMN "datatype2" TO "datatype2new";
   ALTER TABLE "public"."runoob_alter_test" ALTER COLUMN "datatype2new" SET DATA TYPE VARCHAR(50), ALTER COLUMN "datatype2new" SET NOT NULL;

   -- C.
   ALTER TABLE "public"."runoob_alter_test" RENAME COLUMN "datatype3" TO "datatype3new";
   ALTER TABLE "public"."runoob_alter_test" ALTER COLUMN "datatype3new" SET DATA TYPE VARCHAR(100);

   -- D.
   ALTER TABLE "public"."runoob_alter_test" RENAME COLUMN "datatype4" TO "datatype4new";
   ALTER TABLE "public"."runoob_alter_test" ALTER COLUMN "datatype4new" SET DATA TYPE VARCHAR(50);

.. _en-us_topic_0000001234200611__en-us_topic_0238518441_en-us_topic_0237362250_section777435610162:

SET|DROP COLUMN DEFAULT VALUE
-----------------------------

In MySQL, the **COLUMN** keyword can be omitted when the **ALTER** statement is used to set the default value of a column. DSC will perform adaptation based on GaussDB features during migration.

**Input**

.. code-block::

   CREATE TABLE IF NOT EXISTS `runoob_alter_test`(
       `dataType1` int NOT NULL AUTO_INCREMENT,
       `dataType2` FLOAT(10,2),
       `dataType3` DOUBLE(20,8),
       `dataType4` TEXT NOT NULL,
       `dataType5` YEAR NOT NULL DEFAULT '2018',
       `dataType6` DATETIME NOT NULL DEFAULT '2018-10-12 15:27:33.999999',
       `dataType7` CHAR NOT NULL DEFAULT '',
       `dataType8` VARCHAR(50),
       `dataType9` VARCHAR(50) NOT NULL DEFAULT '',
       `dataType10` TIME NOT NULL DEFAULT '10:20:59',
       PRIMARY KEY(`dataType1`)
   )ENGINE=InnoDB DEFAULT CHARSET=utf8;

   ALTER TABLE runoob_alter_test ALTER dataType2 SET DEFAULT 1;
   ALTER TABLE runoob_alter_test ALTER COLUMN dataType2 SET DEFAULT 3;
   ALTER TABLE runoob_alter_test ALTER dataType2 DROP DEFAULT;
   ALTER TABLE runoob_alter_test ALTER COLUMN dataType2 DROP DEFAULT;

**Output**

.. code-block::

   CREATE TABLE "public"."runoob_alter_test"
   (
     "datatype1" SERIAL NOT NULL,
     "datatype2" FLOAT(10),
     "datatype3" FLOAT(20),
     "datatype4" TEXT NOT NULL,
     "datatype5" VARCHAR(4) NOT NULL DEFAULT '2018',
     "datatype6" TIMESTAMP WITHOUT TIME ZONE NOT NULL DEFAULT '2018-10-12 15:27:33.999999',
     "datatype7" CHAR NOT NULL DEFAULT '',
     "datatype8" VARCHAR(50),
     "datatype9" VARCHAR(50) NOT NULL DEFAULT '',
     "datatype10" TIME WITHOUT TIME ZONE NOT NULL DEFAULT '10:20:59',
     PRIMARY KEY ("datatype1")
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype1");

   ALTER TABLE "public"."runoob_alter_test" ALTER COLUMN "datatype2" SET DEFAULT 1;
   ALTER TABLE "public"."runoob_alter_test" ALTER COLUMN "datatype2" SET DEFAULT 3;
   ALTER TABLE "public"."runoob_alter_test" ALTER COLUMN "datatype2" DROP DEFAULT;
   ALTER TABLE "public"."runoob_alter_test" ALTER COLUMN "datatype2" DROP DEFAULT;

.. _en-us_topic_0000001234200611__en-us_topic_0238518441_en-us_topic_0237362250_section179989561167:

DROP (Table Deletion)
---------------------

Both GaussDB(DWS) and MySQL can use the **DROP** statement to delete tables, but GaussDB(DWS) does not support the **RESTRICT \| CASCADE** keyword in **DROP**. DSC will delete the keywords during migration.

**Input**

.. code-block::

   CREATE TABLE IF NOT EXISTS `public`.`express_elb_server`(
      `runoob_id` VARCHAR(10),
      `runoob_title` VARCHAR(100) NOT NULL,
      `runoob_author` VARCHAR(40) NOT NULL,
      `submission_date` VARCHAR(10)
   )ENGINE=InnoDB DEFAULT CHARSET=utf8;
   DROP TABLE  `public`.`express_elb_server` RESTRICT;

**Output**

.. code-block::

   CREATE TABLE "public"."express_elb_server"
   (
     "runoob_id" VARCHAR(10),
     "runoob_title" VARCHAR(100) NOT NULL,
     "runoob_author" VARCHAR(40) NOT NULL,
     "submission_date" VARCHAR(10)
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("runoob_id");
   DROP TABLE "public"."express_elb_server";

.. _en-us_topic_0000001234200611__en-us_topic_0238518441_en-us_topic_0237362250_section142431157181611:

TRUNCATE (Table Deletion)
-------------------------

In MySQL, the **TABLE** keyword can be omitted when the **TRUNCATE** statement is used to delete table data. GaussDB does not support this usage. In addition, DSC will add **CONTINUE IDENTITY RESTRICT** during **TRUNCATE** migration.

**Input**

.. code-block::

   TRUNCATE TABLE `public`.`test_create_table01`;
   TRUNCATE TEST_CREATE_TABLE01;

**Output**

.. code-block::

   TRUNCATE TABLE "public"."test_create_table01" CONTINUE IDENTITY RESTRICT;
   TRUNCATE TABLE "public"."test_create_table01" CONTINUE IDENTITY RESTRICT;

.. _en-us_topic_0000001234200611__en-us_topic_0238518441_en-us_topic_0237362250_section11440115719160:

LOCK
----

GaussDB(DWS) does not support the **ALTER TABLE** *tbName* **LOCK** statement of MySQL, which will be deleted by DSC during migration.

**Input**

.. code-block::

   CREATE TABLE IF NOT EXISTS `runoob_alter_test`(
       `dataType1` int NOT NULL AUTO_INCREMENT,
       `dataType2` FLOAT(10),
       `dataType4` TEXT NOT NULL,
       `dataType5` YEAR NOT NULL DEFAULT '2018',
       `dataType6` DATETIME NOT NULL,
       `dataType7` CHAR NOT NULL DEFAULT '',
       `dataType8` VARCHAR(50),
       `dataType9` VARCHAR(50) NOT NULL DEFAULT '',
       `dataType10` TIME NOT NULL DEFAULT '10:20:59',
       PRIMARY KEY(`dataType1`)
   )ENGINE=InnoDB DEFAULT CHARSET=utf8;

   ## A.
   ALTER TABLE runoob_alter_test LOCK DEFAULT;

   ## B.
   ALTER TABLE runoob_alter_test LOCK=DEFAULT;

   ## C.
   ALTER TABLE runoob_alter_test LOCK EXCLUSIVE;

   ## D.
   ALTER TABLE runoob_alter_test LOCK=EXCLUSIVE;

**Output**

.. code-block::

   CREATE TABLE "public"."runoob_alter_test"
   (
     "datatype1" SERIAL NOT NULL,
     "datatype2" FLOAT(10),
     "datatype4" TEXT NOT NULL,
     "datatype5" VARCHAR(4) NOT NULL DEFAULT '2018',
     "datatype6" TIMESTAMP WITHOUT TIME ZONE NOT NULL,
     "datatype7" CHAR NOT NULL DEFAULT '',
     "datatype8" VARCHAR(50),
     "datatype9" VARCHAR(50) NOT NULL DEFAULT '',
     "datatype10" TIME WITHOUT TIME ZONE NOT NULL DEFAULT '10:20:59',
     PRIMARY KEY ("datatype1")
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype1");

   -- A.

   -- B.

   -- C.

   -- D.

.. _en-us_topic_0000001234200611__en-us_topic_0238518441_en-us_topic_0237362250_section367216577163:

RENAME (Table Renaming)
-----------------------

The statement for renaming a table in MySQL is slightly different from that in GaussDB(DWS). DSC will perform adaptation based on GaussDB features during migration.

.. caution::

   Currently, DSC does not support original table names prefixed with **DATABASE/SCHEMA.**

#. MySQL uses the **RENAME TABLE** statement to change a table name.

   **Input**

   .. code-block::

      # Rename a single table.
      RENAME TABLE DEPARTMENT TO NEWDEPT;

       # Rename multiple tables.
      RENAME TABLE NEWDEPT TO NEWDEPT_02,PEOPLE TO PEOPLE_02;

   **Output**

   .. code-block::

      -- Rename a single table.
      ALTER TABLE "public"."department" RENAME TO "newdept";

      -- Rename multiple tables.
      ALTER TABLE "public"."newdept" RENAME TO "newdept_02";
      ALTER TABLE "public"."people" RENAME TO "people_02";

#. In MySQL, the **ALTER TABLE RENAME** statement is used to change a table name. When this statement is migrated by DSC, the keyword **AS** is converted to **TO**.

   **Input**

   .. code-block::

      ## A.
      ALTER TABLE runoob_alter_test RENAME TO runoob_alter_testnew;

      ## B.
      ALTER TABLE runoob_alter_testnew RENAME AS runoob_alter_testnewnew;

   **Output**

   .. code-block::

      -- A.
      ALTER TABLE "public"."runoob_alter_test" RENAME TO "runoob_alter_testnew";

      -- B.
      ALTER TABLE "public"."runoob_alter_testnew" RENAME TO "runoob_alter_testnewnew";
