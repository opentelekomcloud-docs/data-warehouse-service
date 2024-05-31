:original_name: dws_16_0136.html

.. _dws_16_0136:

FOREIGN_KEY_CHECKS
==================

For foreign key constraints in MySQL, GaussDB(DWS) does not support them modifying table definition. They will be deleted during DSC migration.

**Input**

.. code-block::

   set foreign_key_checks = 0;
   Create Table mall_order_dc (
       id bigint NOT NULL AUTO_INCREMENT,
       order_id varchar(50) NOT NULL,
       key order_id(order_id)
   );

**Output**

.. code-block::

   CREATE TABLE "public"."mall_order_dc" (
     "id" BIGSERIAL NOT NULL,
     "order_id" VARCHAR(200) NOT NULL
   ) WITH (ORIENTATION = ROW, COMPRESSION = NO) NOCOMPRESS DISTRIBUTE BY HASH ("id");
   CREATE INDEX "order_id" ON "public"."mall_order_dc" USING BTREE ("order_id");
