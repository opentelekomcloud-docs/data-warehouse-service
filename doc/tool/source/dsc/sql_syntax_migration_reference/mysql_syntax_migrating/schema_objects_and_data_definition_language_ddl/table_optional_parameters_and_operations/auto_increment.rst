:original_name: dws_16_0122.html

.. _dws_16_0122:

.. _en-us_topic_0000001772536492:

AUTO_INCREMENT
==============

In database application, unique numbers that increase automatically are needed to identify records. In MySQL, the **AUTO_INCREMENT** attribute of a data column can be used to automatically generate the numbers. When creating a table, you can use **AUTO_INCREMENT=**\ *n* to specify a start value. You can also use the **ALTER TABLE** *TABLE_NAME* **AUTO_INCREMENT=**\ *n* command to reset the start value. GaussDB(DWS) does not support this parameter. During DSC migration, the columns with this attribute set are migrated to the SERIAL type, and the keyword is deleted. The following :ref:`Table <en-us_topic_0000001772536492__en-us_topic_0000001658024682_en-us_topic_0000001435896181_table10533101812314>` describes the conversion:

.. _en-us_topic_0000001772536492__en-us_topic_0000001658024682_en-us_topic_0000001435896181_table10533101812314:

.. table:: **Table 1** Data type conversion

   +-----------------------+-----------------------+-----------------------+
   | MySQL Numeric Type    | MySQL INPUT           | GaussDB(DWS) OUTPUT   |
   +=======================+=======================+=======================+
   | TINYINT               | TINYINT               | SMALLSERIAL           |
   +-----------------------+-----------------------+-----------------------+
   | SMALLINT              | SMALLINT UNSIGNED     | SERIAL                |
   |                       |                       |                       |
   |                       | SMALLINT              | SMALLSERIAL           |
   +-----------------------+-----------------------+-----------------------+
   | DOUBLE/FLOAT          | DOUBLE/FLOAT          | BIGSERIAL             |
   +-----------------------+-----------------------+-----------------------+
   | INT/INTEGER           | INT/INTEGER UNSIGNED  | BIGSERIAL             |
   |                       |                       |                       |
   |                       | INT/INTEGER           | SERIAL                |
   +-----------------------+-----------------------+-----------------------+
   | BIGINT/SERIAL         | BIGINT/SERIAL         | BIGSERIAL             |
   +-----------------------+-----------------------+-----------------------+

**Input**

.. code-block::

   CREATE TABLE `public`.`job_instance` (
     `job_sche_id` int(11) NOT NULL AUTO_INCREMENT,
     `task_name` varchar(100) NOT NULL DEFAULT '',
     PRIMARY KEY (`job_sche_id`)
   ) ENGINE=InnoDB AUTO_INCREMENT=219 DEFAULT CHARSET=utf8;

**Output**

.. code-block::

   CREATE TABLE "public"."job_instance"
   (
     "job_sche_id" SERIAL NOT NULL,
     "task_name" VARCHAR(400) NOT NULL DEFAULT '',
     PRIMARY KEY ("job_sche_id")
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("job_sche_id");

In addition, GaussDB(DWS) does not support table definition modification using the **AUTO_INCREMENT** attribute. DSC will delete this attribute during migration.

**Input**

.. code-block::

   CREATE TABLE IF NOT EXISTS `public`.`runoob_alter_test`(
       `dataType1` int NOT NULL AUTO_INCREMENT,
       `dataType2` FLOAT(10,2),
       PRIMARY KEY(`dataType1`)
   );

   ALTER TABLE runoob_alter_test AUTO_INCREMENT 100;
   ALTER TABLE runoob_alter_test AUTO_INCREMENT=100;

**Output**

.. code-block::

   CREATE TABLE IF NOT EXISTS "public"."runoob_alter_test"
   (
     "datatype1" SERIAL NOT NULL,
     "datatype2" REAL,
     PRIMARY KEY ("datatype1")
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype1");
