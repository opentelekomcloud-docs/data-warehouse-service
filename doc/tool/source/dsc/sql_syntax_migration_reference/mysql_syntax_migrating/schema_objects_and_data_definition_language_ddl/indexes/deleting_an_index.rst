:original_name: dws_16_0173.html

.. _dws_16_0173:

.. _en-us_topic_0000001819336229:

Deleting an Index
=================

MySQL supports both **DROP INDEX** and **ALTER TABLE DROP INDEX** for deleting indexes. DSC will perform adaptation based on GaussDB(DWS) features during migration.

#. DROP INDEX

   **Input**

   .. code-block::

      CREATE TABLE `test_create_table03` (
          `DEMAND_ID` INT(11) NOT NULL,
          `DEMAND_NAME` CHAR(100) NOT NULL,
          `THEME` VARCHAR(200) NULL DEFAULT NULL,
          `SEND_ID` INT(11) NULL DEFAULT NULL,
          `SEND_NAME` CHAR(20) NULL DEFAULT NULL,
          `SEND_TIME` DATETIME NULL DEFAULT NULL,
          `DEMAND_CONTENT` TEXT NOT NULL
      )
      COLLATE='utf8_general_ci'
      ENGINE=InnoDB;

      CREATE UNIQUE INDEX DEMAND_NAME_INDEX ON TEST_CREATE_TABLE03(DEMAND_NAME);
      DROP INDEX DEMAND_NAME_INDEX ON TEST_CREATE_TABLE03;

      CREATE INDEX SEND_ID_INDEX ON TEST_CREATE_TABLE03(SEND_ID);
      DROP INDEX SEND_ID_INDEX ON TEST_CREATE_TABLE03;

   **Output**

   .. code-block::

      CREATE TABLE "public"."test_create_table03"
      (
        "demand_id" INTEGER NOT NULL,
        "demand_name" CHAR(400) NOT NULL,
        "theme" VARCHAR(800) DEFAULT NULL,
        "send_id" INTEGER DEFAULT NULL,
        "send_name" CHAR(80) DEFAULT NULL,
        "send_time" TIMESTAMP WITHOUT TIME ZONE DEFAULT NULL,
        "demand_content" TEXT NOT NULL
      )
        WITH ( ORIENTATION = ROW, COMPRESSION = NO )
        NOCOMPRESS
        DISTRIBUTE BY HASH ("demand_id");

      CREATE INDEX "demand_name_index" ON "public"."test_create_table03" ("demand_name");
      DROP INDEX "public"."demand_name_index" RESTRICT;

      CREATE INDEX "send_id_index" ON "public"."test_create_table03" USING BTREE ("send_id");
      DROP INDEX "public"."send_id_index" RESTRICT;

#. ALTER TABLE DROP INDEX

   **Input**

   .. code-block::

      CREATE TABLE `test_create_table03` (
          `DEMAND_ID` INT(11) NOT NULL,
          `DEMAND_NAME` CHAR(100) NOT NULL,
          `THEME` VARCHAR(200) NULL DEFAULT NULL,
          `SEND_ID` INT(11) NULL DEFAULT NULL,
          `SEND_NAME` CHAR(20) NULL DEFAULT NULL,
          `SEND_TIME` DATETIME NULL DEFAULT NULL,
          `DEMAND_CONTENT` TEXT NOT NULL
      )
      COLLATE='utf8_general_ci'
      ENGINE=InnoDB;

      ALTER TABLE TEST_CREATE_TABLE03 ADD UNIQUE INDEX TEST_CREATE_TABLE03_NAME_INDEX(DEMAND_NAME(50));
      ALTER TABLE TEST_CREATE_TABLE03 DROP INDEX TEST_CREATE_TABLE03_NAME_INDEX;

   **Output**

   .. code-block::

      CREATE TABLE "public"."test_create_table03"
      (
        "demand_id" INTEGER NOT NULL,
        "demand_name" CHAR(400) NOT NULL,
        "theme" VARCHAR(800) DEFAULT NULL,
        "send_id" INTEGER DEFAULT NULL,
        "send_name" CHAR(80) DEFAULT NULL,
        "send_time" TIMESTAMP WITHOUT TIME ZONE DEFAULT NULL,
        "demand_content" TEXT NOT NULL
      )
        WITH ( ORIENTATION = ROW, COMPRESSION = NO )
        NOCOMPRESS
        DISTRIBUTE BY HASH ("demand_id");

      CREATE INDEX "test_create_table03_name_index" ON "public"."test_create_table03" ("demand_name");
      DROP INDEX "public"."test_create_table03_name_index" RESTRICT;
