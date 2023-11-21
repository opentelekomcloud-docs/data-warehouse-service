:original_name: dws_07_6834.html

.. _dws_07_6834:

Unique Indexes
==============

.. caution::

   If MySQL unique indexes (constraints) and primary key constraints are used together during migration, OLAP distribution keys may become unavailable. Therefore, this scenario is not supported by DSC.

#. If there is an inline unique index, DSC will delete it directly.

   **Input**

   .. code-block::

      CREATE TABLE IF NOT EXISTS `public`.`runoob_dataType_test`
      (
        `id` INT PRIMARY KEY AUTO_INCREMENT,
        `name` VARCHAR(128) NOT NULL,
        UNIQUE (id ASC)
      );

   **Output**

   .. code-block::

      CREATE TABLE "public"."runoob_datatype_test"
      (
        "id" SERIAL PRIMARY KEY,
        "name" VARCHAR(128) NOT NULL
      )
        WITH ( ORIENTATION = ROW, COMPRESSION = NO )
        NOCOMPRESS
        DISTRIBUTE BY HASH ("id");

#. If there is a unique index created by **ALTER TABLE**, DSC will create a normal index based on GaussDB features.

   **Input**

   .. code-block::

      CREATE TABLE IF NOT EXISTS `public`.`runoob_alter_test`(
          `dataType1` int,
          `dataType2` FLOAT(10,2),
          `dataType3` DOUBLE(20,8)
      )ENGINE=InnoDB DEFAULT CHARSET=utf8;

      ALTER TABLE runoob_alter_test ADD UNIQUE idx_runoob_alter_test_datatype1(dataType1);
      ALTER TABLE runoob_alter_test ADD UNIQUE INDEX idx_runoob_alter_test_datatype1(dataType2);
      ALTER TABLE runoob_alter_test ADD UNIQUE KEY idx_runoob_alter_test_datatype1(dataType3);

      CREATE TABLE IF NOT EXISTS `public`.`runoob_alter_test`(
          `dataType1` int,
          `dataType2` FLOAT(10,2),
          `dataType3` DOUBLE(20,8),
          `dataType4` TEXT NOT NULL,
          `dataType5` YEAR NOT NULL DEFAULT '2018',
          `dataType6` DATETIME NOT NULL DEFAULT '2018-10-12 15:27:33.999999'
      )ENGINE=InnoDB DEFAULT CHARSET=utf8;

      ALTER TABLE runoob_alter_test ADD CONSTRAINT UNIQUE idx_runoob_alter_test_datatype1(dataType1);
      ALTER TABLE runoob_alter_test ADD CONSTRAINT UNIQUE INDEX idx_runoob_alter_test_datatype2(dataType2);
      ALTER TABLE runoob_alter_test ADD CONSTRAINT UNIQUE KEY idx_runoob_alter_test_datatype3(dataType3);
      ALTER TABLE runoob_alter_test ADD CONSTRAINT constraint_dataType UNIQUE idx_runoob_alter_test_datatype4(dataType4);
      ALTER TABLE runoob_alter_test ADD CONSTRAINT constraint_dataType UNIQUE INDEX idx_runoob_alter_test_datatype5(dataType5);
      ALTER TABLE runoob_alter_test ADD CONSTRAINT constraint_dataType UNIQUE KEY idx_runoob_alter_test_datatype6(dataType6);

   **Output**

   .. code-block::

      CREATE TABLE "public"."runoob_alter_test"
      (
        "datatype1" INTEGER,
        "datatype2" FLOAT(10),
        "datatype3" FLOAT(20)
      )
        WITH ( ORIENTATION = ROW, COMPRESSION = NO )
        NOCOMPRESS
        DISTRIBUTE BY HASH ("datatype1");

      CREATE INDEX "idx_runoob_alter_test_datatype1" ON "public"."runoob_alter_test" ("datatype1");
      CREATE INDEX "idx_runoob_alter_test_datatype1" ON "public"."runoob_alter_test" ("datatype2");
      CREATE INDEX "idx_runoob_alter_test_datatype1" ON "public"."runoob_alter_test" ("datatype3");

      CREATE TABLE "public"."runoob_alter_test"
      (
        "datatype1" INTEGER,
        "datatype2" FLOAT(10),
        "datatype3" FLOAT(20),
        "datatype4" TEXT NOT NULL,
        "datatype5" VARCHAR(4) NOT NULL DEFAULT '2018',
        "datatype6" TIMESTAMP WITHOUT TIME ZONE NOT NULL DEFAULT '2018-10-12 15:27:33.999999'
      )
        WITH ( ORIENTATION = ROW, COMPRESSION = NO )
        NOCOMPRESS
        DISTRIBUTE BY HASH ("datatype1");

      CREATE INDEX "idx_runoob_alter_test_datatype1" ON "public"."runoob_alter_test" ("datatype1");
      CREATE INDEX "idx_runoob_alter_test_datatype2" ON "public"."runoob_alter_test" ("datatype2");
      CREATE INDEX "idx_runoob_alter_test_datatype3" ON "public"."runoob_alter_test" ("datatype3");
      CREATE INDEX "idx_runoob_alter_test_datatype4" ON "public"."runoob_alter_test" ("datatype4");
      CREATE INDEX "idx_runoob_alter_test_datatype5" ON "public"."runoob_alter_test" ("datatype5");
      CREATE INDEX "idx_runoob_alter_test_datatype6" ON "public"."runoob_alter_test" ("datatype6");

#. If there is a unique index created by **CREATE INDEX**, DSC will create a normal index based on GaussDB features.

   **Input**

   .. code-block::

      CREATE TABLE `public`.`test_index_table01` (
          `TABLE01_ID` INT(11) NOT NULL,
          `TABLE01_THEME` VARCHAR(100) NULL DEFAULT NULL,
          `AUTHOR_NAME` CHAR(10) NULL DEFAULT NULL,
          `AUTHOR_ID` INT(11) NULL DEFAULT NULL,
          `CREATE_TIME` INT NULL DEFAULT NULL,
          PRIMARY KEY(`TABLE01_ID`)
      );
      CREATE UNIQUE INDEX AUTHOR_INDEX ON `test_index_table01`(AUTHOR_ID);

   **Output**

   .. code-block::

      CREATE TABLE "public"."test_index_table01"
      (
        "table01_id" INTEGER(11) NOT NULL,
        "table01_theme" VARCHAR(100) DEFAULT NULL,
        "author_name" CHAR(10) DEFAULT NULL,
        "author_id" INTEGER(11) DEFAULT NULL,
        "create_time" INTEGER DEFAULT NULL,
        PRIMARY KEY ("table01_id")
      )
        WITH ( ORIENTATION = ROW, COMPRESSION = NO )
        NOCOMPRESS
        DISTRIBUTE BY HASH ("table01_id");
      CREATE INDEX "author_index" ON "public"."test_index_table01" ("author_id");
