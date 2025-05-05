:original_name: dws_16_0172.html

.. _dws_16_0172:

.. _en-us_topic_0000001813438916:

Full-Text Indexes
=================

GaussDB(DWS) does not support full-text indexes. DSC will perform adaptation based on GaussDB(DWS) features during migration.

#. Inline full-text index

   **Input**

   ::

      ## A.
      CREATE TABLE `public`.`test_create_table02` (
          `ID` INT(11) NOT NULL PRIMARY KEY,
          `TITLE` CHAR(255) NOT NULL,
          `CONTENT` TEXT NULL,
          `CREATE_TIME` DATETIME NULL DEFAULT NULL,
           FULLTEXT (`CONTENT`)
      );

      ## B.
      CREATE TABLE IF NOT EXISTS `public`.`runoob_dataType_test`
      (
        `id` INT PRIMARY KEY AUTO_INCREMENT,
        `name` VARCHAR(128) NOT NULL,
        FULLTEXT INDEX (name)
      );

      ## C.
      CREATE TABLE IF NOT EXISTS `public`.`runoob_dataType_test`
      (
        `id` INT PRIMARY KEY AUTO_INCREMENT,
        `name` VARCHAR(128) NOT NULL,
        FULLTEXT INDEX (name ASC)
      );

   **Output**

   ::

      -- A.
      CREATE TABLE "public"."test_create_table02"
      (
        "id" INTEGER NOT NULL PRIMARY KEY,
        "title" CHAR(1020) NOT NULL,
        "content" TEXT,
        "create_time" TIMESTAMP WITHOUT TIME ZONE DEFAULT NULL
      )
        WITH ( ORIENTATION = ROW, COMPRESSION = NO )
        NOCOMPRESS
        DISTRIBUTE BY HASH ("id");
      CREATE INDEX "idx_test_create_table02_content" ON "public"."test_create_table02" USING GIN(to_tsvector(coalesce("content",'')));

      -- B.
      CREATE TABLE IF NOT EXISTS "public"."runoob_datatype_test"
      (
        "id" SERIAL PRIMARY KEY,
        "name" VARCHAR(512) NOT NULL
      )
        WITH ( ORIENTATION = ROW, COMPRESSION = NO )
        NOCOMPRESS
        DISTRIBUTE BY HASH ("id");
      CREATE INDEX "idx_runoob_datatype_test_name" ON "public"."runoob_datatype_test" USING GIN(to_tsvector(coalesce("name",'')));

      -- C.
      CREATE TABLE IF NOT EXISTS "public"."runoob_datatype_test"
      (
        "id" SERIAL PRIMARY KEY,
        "name" VARCHAR(512) NOT NULL
      )
        WITH ( ORIENTATION = ROW, COMPRESSION = NO )
        NOCOMPRESS
        DISTRIBUTE BY HASH ("id");
      CREATE INDEX "idx_runoob_datatype_test_name" ON "public"."runoob_datatype_test" USING GIN(to_tsvector(coalesce("name",'')));

#. Full-text index created by **ALTER TABLE**

   **Input**

   ::

      CREATE TABLE `public`.`test_create_table05` (
       `ID` INT(11) NOT NULL AUTO_INCREMENT,
       `USER_ID` INT(20) NOT NULL,
       `USER_NAME` CHAR(20) NULL DEFAULT NULL,
       `DETAIL` VARCHAR(100) NULL DEFAULT NULL,
       PRIMARY KEY (`ID`)
      );
      ALTER TABLE TEST_CREATE_TABLE05 ADD FULLTEXT INDEX USER_ID_INDEX_02(USER_ID);
      ALTER TABLE TEST_CREATE_TABLE05 ADD FULLTEXT USER_NAME_INDEX_02(USER_NAME);

   **Output**

   ::

      CREATE TABLE "public"."test_create_table05"
      (
        "id" SERIAL NOT NULL,
        "user_id" INTEGER NOT NULL,
        "user_name" CHAR(80) DEFAULT NULL,
        "detail" VARCHAR(400) DEFAULT NULL,
        PRIMARY KEY ("id")
      )
        WITH ( ORIENTATION = ROW, COMPRESSION = NO )
        NOCOMPRESS
        DISTRIBUTE BY HASH ("id");
      CREATE INDEX "user_id_index_02" ON "public"."test_create_table05" USING GIN(to_tsvector(coalesce("user_id",'')));
      CREATE INDEX "user_name_index_02" ON "public"."test_create_table05" USING GIN(to_tsvector(coalesce("user_name",'')));

#. Full-text index created by **CREATE INDEX**

   **Input**

   ::

      CREATE TABLE `public`.`test_index_table02` (
          `ID` INT(11) NOT NULL PRIMARY KEY,
          `TITLE` CHAR(255) NOT NULL,
          `CONTENT` TEXT NULL,
          `CREATE_TIME` INT(10) NULL DEFAULT NULL
      );
      CREATE FULLTEXT INDEX CON_INDEX ON TEST_INDEX_TABLE02(CONTENT);

   **Output**

   ::

      CREATE TABLE "public"."test_index_table02"
      (
        "id" INTEGER NOT NULL PRIMARY KEY,
        "title" CHAR(1020) NOT NULL,
        "content" TEXT,
        "create_time" INTEGER DEFAULT NULL
      )
        WITH ( ORIENTATION = ROW, COMPRESSION = NO )
        NOCOMPRESS
        DISTRIBUTE BY HASH ("id");
      CREATE INDEX "con_index" ON "public"."test_index_table02" USING GIN(to_tsvector(coalesce("content",'')));
