:original_name: dws_07_6836.html

.. _dws_07_6836:

Hash Indexes
============

GaussDB(DWS) does not support hash indexes. DSC will replace these indexes with normal indexes based on GaussDB features.

#. Inline hash index

   **Input**

   .. code-block::

      CREATE TABLE `public`.`test_create_table03` (
          `DEMAND_ID` INT(11) NOT NULL AUTO_INCREMENT,
          `DEMAND_NAME` CHAR(100) NOT NULL,
          `THEME` VARCHAR(200) NULL DEFAULT NULL,
          `SEND_ID` INT(11) NULL DEFAULT NULL,
          `SEND_NAME` CHAR(20) NULL DEFAULT NULL,
          `SEND_TIME` DATETIME NULL DEFAULT NULL,
          `DEMAND_CONTENT` TEXT NOT NULL,
          PRIMARY KEY(`DEMAND_ID`),
          INDEX CON_INDEX(DEMAND_CONTENT(100)) USING HASH ,
          INDEX SEND_INFO_INDEX USING HASH (SEND_ID,SEND_NAME(10),SEND_TIME)
      );

   **Output**

   .. code-block::

      CREATE TABLE "public"."test_create_table03"
      (
        "demand_id" SERIAL NOT NULL,
        "demand_name" CHAR(100) NOT NULL,
        "theme" VARCHAR(200) DEFAULT NULL,
        "send_id" INTEGER(11) DEFAULT NULL,
        "send_name" CHAR(20) DEFAULT NULL,
        "send_time" TIMESTAMP WITHOUT TIME ZONE DEFAULT NULL,
        "demand_content" TEXT NOT NULL,
        PRIMARY KEY ("demand_id")
      )
        WITH ( ORIENTATION = ROW, COMPRESSION = NO )
        NOCOMPRESS
        DISTRIBUTE BY HASH ("demand_id");
      CREATE INDEX "con_index" ON "public"."test_create_table03" ("demand_content");
      CREATE INDEX "send_info_index" ON "public"."test_create_table03" ("send_id","send_name","send_time");

#. Hash index created by **ALTER TABLE**

   **Input**

   .. code-block::

      CREATE TABLE IF NOT EXISTS `public`.`runoob_alter_test`(
          `dataType1` int NOT NULL AUTO_INCREMENT,
          `dataType2` FLOAT(10,2),
          PRIMARY KEY(`dataType1`)
      )ENGINE=InnoDB DEFAULT CHARSET=utf8;

      ALTER TABLE runoob_alter_test ADD KEY alterTable_addKey_indexType(dataType1) USING HASH;

   **Output**

   .. code-block::

      CREATE TABLE "public"."runoob_alter_test"
      (
        "datatype1" SERIAL NOT NULL,
        "datatype2" FLOAT(10),
        PRIMARY KEY ("datatype1")
      )
        WITH ( ORIENTATION = ROW, COMPRESSION = NO )
        NOCOMPRESS
        DISTRIBUTE BY HASH ("datatype1");

      CREATE INDEX "altertable_addkey_indextype" ON "public"."runoob_alter_test" ("datatype1");

#. Hash index created by **CREATE INDEX**

   **Input**

   .. code-block::

      CREATE TABLE `public`.`test_index_table06` (
          `ID` INT(11) NOT NULL AUTO_INCREMENT,
          `FNAME` VARCHAR(30) NOT NULL,
          `INAME` VARCHAR(30) NOT NULL,
          PRIMARY KEY (`ID`)
      );
      CREATE INDEX FNAME_INDEX ON TEST_INDEX_TABLE06(FNAME(10)) USING HASH;
      CREATE INDEX NAME_01 ON TEST_INDEX_TABLE06(FNAME(10),INAME(10)) USING HASH;

   **Output**

   .. code-block::

      CREATE TABLE "public"."test_index_table06"
      (
        "id" SERIAL NOT NULL,
        "fname" VARCHAR(30) NOT NULL,
        "iname" VARCHAR(30) NOT NULL,
        PRIMARY KEY ("id")
      )
        WITH ( ORIENTATION = ROW, COMPRESSION = NO )
        NOCOMPRESS
        DISTRIBUTE BY HASH ("id");
      CREATE INDEX "fname_index" ON "public"."test_index_table06" ("fname");
      CREATE INDEX "name_01" ON "public"."test_index_table06" ("fname","iname");
