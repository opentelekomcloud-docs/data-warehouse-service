:original_name: dws_16_0171.html

.. _dws_16_0171:

.. _en-us_topic_0000001860198745:

Spatial Indexes
===============

GaussDB(DWS) does not support spatial indexes. DSC will perform adaptation based on GaussDB(DWS) features during migration.

#. Inline spatial index

   **Input**

   ::

      CREATE TABLE `public`.`test_create_table04` (
          `ID` INT(11) NOT NULL AUTO_INCREMENT PRIMARY KEY,
          `A` POINT NOT NULL,
          `B` POLYGON NOT NULL,
          `C` GEOMETRYCOLLECTION NOT NULL,
          `D` LINESTRING NOT NULL,
          `E` MULTILINESTRING NOT NULL,
          `F` MULTIPOINT NOT NULL,
          `G` MULTIPOLYGON NOT NULL,
          SPATIAL INDEX A_INDEX(A),
          SPATIAL INDEX B_INDEX(B),
          SPATIAL INDEX C_INDEX(C),
          SPATIAL KEY D_INDEX(D),
          SPATIAL KEY E_INDEX(E),
          SPATIAL KEY F_INDEX(F),
          SPATIAL INDEX G_INDEX(G)
      );

   **Output**

   ::

      CREATE TABLE "public"."test_create_table04"
      (
        "id" SERIAL NOT NULL PRIMARY KEY,
        "a" POINT NOT NULL,
        "b" POLYGON NOT NULL,
        "c" GEOMETRYCOLLECTION NOT NULL,
        "d" POLYGON NOT NULL,
        "e" BOX NOT NULL,
        "f" BOX NOT NULL,
        "g" POLYGON NOT NULL
      )
        WITH ( ORIENTATION = ROW, COMPRESSION = NO )
        NOCOMPRESS
        DISTRIBUTE BY HASH ("id");
      CREATE INDEX "a_index" ON "public"."test_create_table04" USING GIST ("a");
      CREATE INDEX "b_index" ON "public"."test_create_table04" USING GIST ("b");
      CREATE INDEX "c_index" ON "public"."test_create_table04" USING GIST ("c");
      CREATE INDEX "d_index" ON "public"."test_create_table04" USING GIST ("d");
      CREATE INDEX "e_index" ON "public"."test_create_table04" USING GIST ("e");
      CREATE INDEX "f_index" ON "public"."test_create_table04" USING GIST ("f");
      CREATE INDEX "g_index" ON "public"."test_create_table04" USING GIST ("g");

#. Spatial index created by **ALTER TABLE**

   **Input**

   ::

      CREATE TABLE `public`.`test_create_table04` (
       `ID` INT(11) NOT NULL AUTO_INCREMENT PRIMARY KEY,
       `A` POINT NOT NULL,
       `B` POLYGON NOT NULL,
       `C` GEOMETRYCOLLECTION NOT NULL,
       `D` LINESTRING NOT NULL,
       `E` MULTILINESTRING NOT NULL,
       `F` MULTIPOINT NOT NULL,
       `G` MULTIPOLYGON NOT NULL
      );

      ALTER TABLE `test_create_table04` ADD SPATIAL INDEX A_INDEX(A);
      ALTER TABLE `test_create_table04` ADD SPATIAL INDEX E_INDEX(E) USING BTREE;

   **Output**

   ::

      CREATE TABLE "public"."test_create_table04"
      (
        "id" SERIAL NOT NULL PRIMARY KEY,
        "a" POINT NOT NULL,
        "b" POLYGON NOT NULL,
        "c" GEOMETRYCOLLECTION NOT NULL,
        "d" POLYGON NOT NULL,
        "e" BOX NOT NULL,
        "f" BOX NOT NULL,
        "g" POLYGON NOT NULL
      )
        WITH ( ORIENTATION = ROW, COMPRESSION = NO )
        NOCOMPRESS
        DISTRIBUTE BY HASH ("id");

      CREATE INDEX "a_index" ON "public"."test_create_table04" USING GIST ("a");
      CREATE INDEX "e_index" ON "public"."test_create_table04" USING GIST ("e");

#. Spatial index created by **CREATE INDEX**

   **Input**

   ::

      CREATE TABLE `public`.`test_create_table04` (
          `ID` INT(11) NOT NULL AUTO_INCREMENT PRIMARY KEY,
          `A` POINT NOT NULL,
          `B` POLYGON NOT NULL,
          `C` GEOMETRYCOLLECTION NOT NULL,
          `D` LINESTRING NOT NULL,
          `E` MULTILINESTRING NOT NULL,
          `F` MULTIPOINT NOT NULL,
          `G` MULTIPOLYGON NOT NULL
      );

      CREATE SPATIAL INDEX A_INDEX ON `test_create_table04`(A);

   **Output**

   ::

      CREATE TABLE "public"."test_create_table04"
      (
        "id" SERIAL NOT NULL PRIMARY KEY,
        "a" POINT NOT NULL,
        "b" POLYGON NOT NULL,
        "c" GEOMETRYCOLLECTION NOT NULL,
        "d" POLYGON NOT NULL,
        "e" BOX NOT NULL,
        "f" BOX NOT NULL,
        "g" POLYGON NOT NULL
      )
        WITH ( ORIENTATION = ROW, COMPRESSION = NO )
        NOCOMPRESS
        DISTRIBUTE BY HASH ("id");

      CREATE INDEX "a_index" ON "public"."test_create_table04" USING GIST ("a");
