:original_name: dws_16_0111.html

.. _dws_16_0111:

.. _en-us_topic_0000001813598864:

Spatial Data Types
==================

Overview
--------

MySQL has spatial data types corresponding to the OpenGIS class. DSC supports the following type conversions:

Type Mapping
------------

.. table:: **Table 1** Spatial type mapping

   ================== ================== ===================
   MySQL Spatial Type MySQL INPUT        GaussDB(DWS) OUTPUT
   ================== ================== ===================
   GEOMETRY           GEOMETRY           GEOMETRY
   POINT              POINT              POINT
   LINESTRING         LINESTRING         POLYGON
   POLYGON            POLYGON            POLYGON
   MULTIPOINT         MULTIPOINT         BOX
   MULTILINESTRING    MULTILINESTRING    BOX
   MULTIPOLYGON       MULTIPOLYGON       POLYGON
   GEOMETRYCOLLECTION GEOMETRYCOLLECTION GEOMETRYCOLLECTION
   ================== ================== ===================

.. note::

   -  GEOMETRY can store geometry values of any type. The other single-value types (POINT, LINESTRING, and POLYGON) restrict their values to a particular geometry type.
   -  GEOMETRYCOLLECTION can store a collection of objects of any type. The other collection types (**MULTIPOINT**, **MULTILINESTRING**, **MULTIPOLYGON**, and **GEOMETRYCOLLECTION**) restrict collection members to those having a particular geometry type.

-  **Input**

   ::

      CREATE TABLE `t_geo_test2`  (
        `id` int(11) NOT NULL,
        `name` varchar(255),
        `geometry_1` geometry NOT NULL,
        `point_1` point NOT NULL,
        `linestring_1` linestring NOT NULL,
        `polygon_1` polygon NOT NULL,
        `multipoint_1` multipoint NOT NULL,
        `multilinestring_1` multilinestring NOT NULL,
        `multipolygon_1` multipolygon NOT NULL,
        `geometrycollection_1` geometrycollection NOT NULL,
        PRIMARY KEY (`id`) USING BTREE
      ) ENGINE = InnoDB;

   **Output**

   ::

      CREATE TABLE "public"."t_geo_test2"
      (
        "id" INTEGER(11) NOT NULL,
        "name" VARCHAR(255),
        "geometry_1" GEOMETRY NOT NULL,
        "point_1" POINT NOT NULL,
        "linestring_1" POLYGON NOT NULL,
        "polygon_1" POLYGON NOT NULL,
        "multipoint_1" BOX NOT NULL,
        "multilinestring_1" BOX NOT NULL,
        "multipolygon_1" POLYGON NOT NULL,
        "geometrycollection_1" GEOMETRYCOLLECTION NOT NULL,
        PRIMARY KEY ("id")
      )
        WITH ( ORIENTATION = ROW, COMPRESSION = NO )
        NOCOMPRESS
        DISTRIBUTE BY HASH ("id");
