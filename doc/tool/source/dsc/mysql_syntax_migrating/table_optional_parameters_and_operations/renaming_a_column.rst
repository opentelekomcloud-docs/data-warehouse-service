:original_name: dws_16_0163.html

.. _dws_16_0163:

Renaming a Column
=================

Using a Reserved Keyword As a Column Name
-----------------------------------------

In GaussDB(DWS), reserved keywords must be enclosed in double quotation marks when they are used as column names. The following reserved keywords are supported: desc, checksum, operator, and size.

**Input**

::

   CREATE TABLE `desc` (
       user char,
       checksum int,
       operator smallint,
       desc char,
       size bigint
   );

**Output**

::

   CREATE TABLE "public"."desc" (
     "user" CHAR(4),
     "checksum" INTEGER,
     "operator" SMALLINT,
     "desc" CHAR(4),
     "size" BIGINT
   ) WITH (ORIENTATION = ROW, COMPRESSION = NO) NOCOMPRESS DISTRIBUTE BY HASH ("user");

Using the Name of a Hidden Column of a System Catalog As a Column Name
----------------------------------------------------------------------

If the defined column name is the same as that of a hidden column in a GaussDB(DWS) system catalog ( including **xc_node_id** and **tableoid**, **cmax**, **xmax**, **cmin**, **xmin**, **ctid**, **tid.**), you need to rename the column. To rename the column, add **\_new** at the end. For example, change **xc_node_id** to **xc_node_id_new**.

**Input**

::

   DROP TABLE IF EXISTS `renameFieldName`;
   CREATE TABLE IF NOT EXISTS `renameFieldName`(
       xc_node_id int,
       tableoid char(3),
       cmax smallint,
       xmax bigint auto_increment,
       cmin varchar(10),
       xmin int,
       ctid int auto_increment,
       tid int
   );
   DROP TABLE IF EXISTS `renameFieldName`;

**Output**

::

   DROP TABLE IF EXISTS "public"."renamefieldname";
   CREATE TABLE IF NOT EXISTS "public"."renamefieldname" (
     "xc_node_id_new" INTEGER,
     "tableoid_new" CHAR(12),
     "cmax_new" SMALLINT,
     "xmax_new" BIGSERIAL,
     "cmin_new" VARCHAR(40),
     "xmin_new" INTEGER,
     "ctid_new" SERIAL,
     "tid_new" INTEGER
   ) WITH (ORIENTATION = ROW, COMPRESSION = NO) NOCOMPRESS DISTRIBUTE BY HASH ("xc_node_id_new");
   DROP TABLE IF EXISTS "public"."renamefieldname";
