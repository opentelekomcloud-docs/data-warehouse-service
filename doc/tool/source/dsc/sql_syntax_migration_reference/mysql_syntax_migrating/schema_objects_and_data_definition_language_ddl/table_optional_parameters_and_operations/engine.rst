:original_name: dws_16_0135.html

.. _dws_16_0135:

.. _en-us_topic_0000001819416217:

ENGINE
======

In MySQL, **ENGINE** specifies the storage engine for a table. When the storage engine is **ARCHIVE**, **BLACKHOLE**, **CSV**, **FEDERATED**, **INNODB**, **MYISAM**, **MEMORY**, **MRG_MYISAM**, **NDB**, **NDBCLUSTER**, or **PERFORMANCE_SCHEMA**, this attribute can be migrated and will be deleted during the migration.

**Input**

.. code-block::

   CREATE TABLE `public`.`runoob_alter_test`(
   `dataType1` int NOT NULL,
   `dataType2` DOUBLE(20,8),
   PRIMARY KEY(`dataType1`)
   )ENGINE=MYISAM;

   ## A.
   ALTER TABLE runoob_alter_test ENGINE INNODB;
   ALTER TABLE runoob_alter_test ENGINE=INNODB;

   ## B.
   ALTER TABLE runoob_alter_test ENGINE MYISAM;
   ALTER TABLE runoob_alter_test ENGINE=MYISAM;

   ## C.
   ALTER TABLE runoob_alter_test ENGINE MEMORY;
   ALTER TABLE runoob_alter_test ENGINE=MEMORY;

**Output**

.. code-block::

   CREATE TABLE "public"."runoob_alter_test"
   (
     "datatype1" INTEGER NOT NULL,
     "datatype2" DOUBLE PRECISION,
     PRIMARY KEY ("datatype1")
   )
     WITH ( ORIENTATION = ROW, COMPRESSION = NO )
     NOCOMPRESS
     DISTRIBUTE BY HASH ("datatype1");

   -- A.

   -- B.

   -- C.
