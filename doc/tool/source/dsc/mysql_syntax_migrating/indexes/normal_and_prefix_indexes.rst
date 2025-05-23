:original_name: dws_16_0168.html

.. _dws_16_0168:

.. _en-us_topic_0000001860198649:

Normal and Prefix Indexes
=========================

GaussDB(DWS) does not support prefix indexes or inline normal indexes. DSC will replace these indexes with normal indexes based on GaussDB(DWS) features.

#. Inline normal/prefix indexes

   **Input**

   ::

      CREATE TABLE IF NOT EXISTS `public`.`runoob_dataType_test`
      (
        `id` INT PRIMARY KEY AUTO_INCREMENT,
        `name` VARCHAR(128) NOT NULL,
        INDEX index_single(name(10))
      );

   **Output**

   ::

      CREATE TABLE IF NOT EXISTS "public"."runoob_datatype_test"
      (
        "id" SERIAL PRIMARY KEY,
        "name" VARCHAR(512) NOT NULL
      )
        WITH ( ORIENTATION = ROW, COMPRESSION = NO )
        NOCOMPRESS
        DISTRIBUTE BY HASH ("id");
      CREATE INDEX "index_single" ON "public"."runoob_datatype_test" USING BTREE ("name");

#. Normal/Prefix index created by **ALTER TABLE**

   **Input**

   ::

      CREATE TABLE `public`.`test_create_table05` (
       `ID` INT(11) NOT NULL AUTO_INCREMENT,
       `USER_ID` INT(20) NOT NULL,
       `USER_NAME` CHAR(20) NULL DEFAULT NULL,
       `DETAIL` VARCHAR(100) NULL DEFAULT NULL,
       PRIMARY KEY (`ID`)
      );

      ALTER TABLE TEST_CREATE_TABLE05 ADD INDEX USER_NAME_INDEX_02(USER_NAME(10));

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

      CREATE INDEX "user_name_index_02" ON "public"."test_create_table05" ("user_name");

#. Normal/Prefix index created by **CREATE INDEX**

   **Input**

   ::

      CREATE TABLE IF NOT EXISTS `public`.`customer`(
          `name` varchar(64) primary key,
          id integer,
          id2 integer
      );

      CREATE INDEX part_of_name ON customer (name(10));

   **Output**

   ::

      CREATE TABLE IF NOT EXISTS "public"."customer"
      (
        "name" VARCHAR(256) PRIMARY KEY,
        "id" INTEGER,
        "id2" INTEGER
      )
        WITH ( ORIENTATION = ROW, COMPRESSION = NO )
        NOCOMPRESS
        DISTRIBUTE BY HASH ("name");

      CREATE INDEX "part_of_name" ON "public"."customer" USING BTREE ("name");
