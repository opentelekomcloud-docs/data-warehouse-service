:original_name: dws_16_0170.html

.. _dws_16_0170:

.. _en-us_topic_0000001860199101:

B-tree Indexes
==============

GaussDB(DWS) supports B-tree indexes, but the position of the **USING BTREE** keyword in a statement is different from that in MySQL. DSC will perform adaptation based on GaussDB(DWS) features during migration.

#. Inline B-tree index

   **Input**

   ::

      CREATE TABLE `public`.`test_create_table03` (
          `DEMAND_ID` INT(11) NOT NULL AUTO_INCREMENT,
          `DEMAND_NAME` CHAR(100) NOT NULL,
          `THEME` VARCHAR(200) NULL DEFAULT NULL,
          `SEND_ID` INT(11) NULL DEFAULT NULL,
          `SEND_NAME` CHAR(20) NULL DEFAULT NULL,
          `SEND_TIME` DATETIME NULL DEFAULT NULL,
          `DEMAND_CONTENT` TEXT NOT NULL,
          PRIMARY KEY(`DEMAND_ID`),
          INDEX THEME_INDEX(THEME) USING BTREE,
          INDEX NAME_INDEX USING BTREE (SEND_NAME(10))
      );

   **Output**

   ::

      CREATE TABLE "public"."test_create_table03"
      (
        "demand_id" SERIAL NOT NULL,
        "demand_name" CHAR(400) NOT NULL,
        "theme" VARCHAR(800) DEFAULT NULL,
        "send_id" INTEGER DEFAULT NULL,
        "send_name" CHAR(80) DEFAULT NULL,
        "send_time" TIMESTAMP WITHOUT TIME ZONE DEFAULT NULL,
        "demand_content" TEXT NOT NULL,
        PRIMARY KEY ("demand_id")
      )
        WITH ( ORIENTATION = ROW, COMPRESSION = NO )
        NOCOMPRESS
        DISTRIBUTE BY HASH ("demand_id");
      CREATE INDEX "theme_index" ON "public"."test_create_table03" USING BTREE ("theme");
      CREATE INDEX "name_index" ON "public"."test_create_table03" USING BTREE ("send_name");

#. B-tree index created by **ALTER TABLE**

   **Input**

   ::

      CREATE TABLE IF NOT EXISTS `public`.`runoob_alter_test`(
          `dataType1` int NOT NULL AUTO_INCREMENT,
          `dataType2` FLOAT(10,2),
          PRIMARY KEY(`dataType1`)
      );

      ALTER TABLE runoob_alter_test ADD KEY alterTable_addKey_indexType (dataType1) USING BTREE;

   **Output**

   ::

      CREATE TABLE IF NOT EXISTS "public"."runoob_alter_test"
      (
        "datatype1" SERIAL NOT NULL,
        "datatype2" REAL,
        PRIMARY KEY ("datatype1")
      )
        WITH ( ORIENTATION = ROW, COMPRESSION = NO )
        NOCOMPRESS
        DISTRIBUTE BY HASH ("datatype1");

      CREATE INDEX "altertable_addkey_indextype" ON "public"."runoob_alter_test" ("datatype1");

#. B-tree index created by **CREATE INDEX**

   **Input**

   ::

      CREATE TABLE `public`.`test_index_table05` (
          `ID` INT(11) NOT NULL AUTO_INCREMENT,
          `USER_ID` INT(20) NOT NULL,
          `USER_NAME` CHAR(20) NULL DEFAULT NULL,
          `DETAIL` VARCHAR(100) NULL DEFAULT NULL,
          PRIMARY KEY (`ID`)
      );
      CREATE UNIQUE INDEX USER_ID_INDEX USING BTREE ON TEST_INDEX_TABLE05(USER_ID);
      CREATE INDEX USER_NAME_INDEX USING BTREE ON TEST_INDEX_TABLE05(USER_NAME(10));
      CREATE INDEX DETAIL_INDEX ON TEST_INDEX_TABLE05(DETAIL(50)) USING BTREE;
      CREATE INDEX USER_INFO_INDEX USING BTREE ON TEST_INDEX_TABLE05(USER_ID,USER_NAME(10));

   **Output**

   ::

      CREATE TABLE "public"."test_index_table05"
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
      CREATE INDEX "user_id_index" ON "public"."test_index_table05" ("user_id");
      CREATE INDEX "user_name_index" ON "public"."test_index_table05" USING BTREE ("user_name");
      CREATE INDEX "detail_index" ON "public"."test_index_table05" USING BTREE ("detail");
      CREATE INDEX "user_info_index" ON "public"."test_index_table05" USING BTREE ("user_id","user_name");
