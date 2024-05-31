:original_name: dws_16_0174.html

.. _dws_16_0174:

.. _en-us_topic_0000001772696220:

Renaming an Index
=================

DSC supports renaming indexes. Prefix a table name to an index name to prevent name conflicts. (Only DDL statements with specific index names can be created. Currently, the name of a renamed index cannot be deleted. Exercise caution when modifying this parameter.)

**Modifying the configuration**

Open :ref:`Table 1 Configuration parameters in the features-mysql.properties file <en-us_topic_0000001772696060__en-us_topic_0000001706105077_en-us_topic_0000001434418777_table1740918360500>` and change the value of the following parameter to **true**: The default value is **false**, indicating that the file will not be renamed.

.. code-block::

   # Whether to rename an index when creating the index.
   table.index.rename=true

**Input**

.. code-block::

   CREATE TABLE IF NOT EXISTS `CUSTOMER`(
       `NAME` VARCHAR(64) PRIMARY KEY,
       ID INTEGER,
       ID2 INTEGER);
   CREATE INDEX ID_INDEX USING BTREE ON CUSTOMER (ID);
   ALTER TABLE CUSTOMER ADD INDEX ID3_INDEX(ID2);

**Output**

.. code-block::

   CREATE TABLE IF NOT EXISTS "public"."customer" (
       "name" VARCHAR(256) PRIMARY KEY,
       "id" INTEGER,
       "id2" INTEGER) WITH (ORIENTATION = ROW, COMPRESSION = NO) NOCOMPRESS DISTRIBUTE BY HASH ("name");
   CREATE INDEX customer_id_index ON "public"."customer" USING BTREE ("id");
   CREATE INDEX customer_id3_index ON "public"."customer" ("id2");
