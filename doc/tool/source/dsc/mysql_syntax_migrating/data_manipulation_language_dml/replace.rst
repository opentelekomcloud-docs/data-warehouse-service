:original_name: dws_16_0198.html

.. _dws_16_0198:

.. _en-us_topic_0000001860318485:

REPLACE
=======

In MySQL, **REPLACE** allows the following keywords: **LOW_PRIORITY**, **PARTITION**, **DELAYED**, **VALUES**, and **SET**. The following examples are temporary migration solutions only.

.. note::

   **REPLACE** works exactly like **INSERT**, except that if an old row in the table has the same value as a new row for a primary key or unique index, the old row is deleted before the new row is inserted.

.. _en-us_topic_0000001860318485__en-us_topic_0000001432447489_section126212335453:

LOW_PRIORITY
------------

**MySQL REPLACE** supports the use of **LOW_PRIORITY**, which is converted by DSC.

**Input**

::

   # LOW_PRIORITY
   Replace LOW_PRIORITY INTO  exmp_tb2 VALUES(1, '128.23', 'nice', '2018-10-11 19:00:00');
   Replace LOW_PRIORITY INTO  exmp_tb2 VALUES(2, DEFAULT, 'nice', '2018-12-14 19:00:00' );
   Replace LOW_PRIORITY INTO  exmp_tb2 VALUES(3, DEFAULT, 'nice', DEFAULT);
   Replace LOW_PRIORITY INTO  exmp_tb2 (tb2_id, tb2_price) VALUES(5, DEFAULT);
   Replace LOW_PRIORITY INTO  exmp_tb2 (tb2_id, tb2_price, tb2_note) VALUES(4, DEFAULT, DEFAULT);

**Output**

::

   -- LOW_PRIORITY
   INSERT INTO "public"."exmp_tb2"  VALUES (1,'128.23','nice','2018-10-11 19:00:00');
   INSERT INTO "public"."exmp_tb2"  VALUES (2,DEFAULT,'nice','2018-12-14 19:00:00');
   INSERT INTO "public"."exmp_tb2"  VALUES (3,DEFAULT,'nice',DEFAULT);
   INSERT INTO "public"."exmp_tb2" ("tb2_id","tb2_price") VALUES (5,DEFAULT);
   INSERT INTO "public"."exmp_tb2" ("tb2_id","tb2_price","tb2_note") VALUES (4,DEFAULT,DEFAULT);

.. _en-us_topic_0000001860318485__en-us_topic_0000001432447489_section10939191164510:

PARTITION
---------

**MySQL REPLACE** supports explicit partitioning selection using the **PARTITION** keyword and a comma-separated name list for partitions, subpartitions, or both.

**Input**

::

   replace INTO employees PARTITION(p3) VALUES (19, 'Frank1', 'Williams', 1, 2);
   replace INTO employees PARTITION(p0) VALUES (4, 'Frank1', 'Williams', 1, 2);
   replace INTO employees PARTITION(p1) VALUES (9, 'Frank1', 'Williams', 1, 2);
   replace INTO employees PARTITION(p2) VALUES (10, 'Frank1', 'Williams', 1, 2);
   replace INTO employees PARTITION(p2) VALUES (11, 'Frank1', 'Williams', 1, 2);

**Output**

::

   INSERT INTO "public"."employees"  VALUES (19,'Frank1','Williams',1,2);
   INSERT INTO "public"."employees"  VALUES (4,'Frank1','Williams',1,2);
   INSERT INTO "public"."employees"  VALUES (9,'Frank1','Williams',1,2);
   INSERT INTO "public"."employees"  VALUES (10,'Frank1','Williams',1,2);
   INSERT INTO "public"."employees"  VALUES (11,'Frank1','Williams',1,2);

.. _en-us_topic_0000001860318485__en-us_topic_0000001432447489_section126112044124417:

DELAYED
-------

.. warning::

   -  **DELAYED** **INSERT** and **REPLACE** operations were deprecated in MySQL 5.6. In MySQL 5.7, **DELAYED** was not supported. The server recognizes but ignores the **DELAYED** keyword, handles **REPLACE** as a non-delayed one, and generates an **ER_WARN_LEGACY_SYNTAX_CONVERTED** warning.
   -  (**REPLACE DELAYED** is no longer supported and the statement is converted to **REPLACE**.)
   -  The keyword **DELAYED** will be deleted in later versions.

**Input**

::

   #DELAYED INSERT DELAYED works only with MyISAM, MEMORY, ARCHIVE, and BLACKHOLE tables.
   #If you execute INSERT DELAYED with another storage engine,
   #you will get an error like this: ERROR 1616 (HY000): DELAYED option not supported
   Replace DELAYED INTO  exmp_tb2 VALUES(10, 128.23, 'nice', '2018-10-11 19:00:00');
   Replace DELAYED INTO  exmp_tb2 VALUES(6, DEFAULT, 'nice', '2018-12-14 19:00:00');
   Replace DELAYED INTO  exmp_tb2 VALUES(7, 20, 'nice', DEFAULT);
   Replace DELAYED INTO  exmp_tb2 (tb2_id, tb2_price) VALUES(11, DEFAULT);
   Replace DELAYED INTO  exmp_tb2 (tb2_id, tb2_price, tb2_note) VALUES(12, DEFAULT, DEFAULT);
   Replace DELAYED INTO  exmp_tb2 (tb2_id, tb2_price, tb2_note, tb2_date) VALUES(13, DEFAULT, DEFAULT, DEFAULT);

**Output**

::

   --DELAYED INSERT DELAYED works only with MyISAM, MEMORY, ARCHIVE, and BLACKHOLE tables.
   --If you execute INSERT DELAYED with another storage engine,
   --you will get an error like this: ERROR 1616 (HY000): DELAYED option not supported.
   INSERT INTO "public"."exmp_tb2"  VALUES (10,128.23,'nice','2018-10-11 19:00:00');
   INSERT INTO "public"."exmp_tb2"  VALUES (6,DEFAULT,'nice','2018-12-14 19:00:00');
   INSERT INTO "public"."exmp_tb2"  VALUES (7,20,'nice',DEFAULT);
   INSERT INTO "public"."exmp_tb2" ("tb2_id","tb2_price") VALUES (11,DEFAULT);
   INSERT INTO "public"."exmp_tb2" ("tb2_id","tb2_price","tb2_note") VALUES (12,DEFAULT,DEFAULT);
   INSERT INTO "public"."exmp_tb2" ("tb2_id","tb2_price","tb2_note","tb2_date") VALUES (13,DEFAULT,DEFAULT,DEFAULT);

.. _en-us_topic_0000001860318485__en-us_topic_0000001432447489_section162551728134414:

VALUES
------

**MySQL REPLACE** supports a statement to insert or delete multiple values, separated by commas.

**Input**

::

   # If data is available, replacement will be performed. Otherwise, INSERT will be performed.
   Replace INTO  exmp_tb1 (tb1_id,tb1_name,tb1_gender,tb1_address,tb1_number) VALUES(17,'David','male','NewYork11','01015827875'),(18,'Rachel','female','NewYork22','01015827749'),(20,'Monica','female','NewYork','010158996743');
   Replace INTO  exmp_tb1 (tb1_id,tb1_name,tb1_gender,tb1_address,tb1_number) VALUES(17,'David1','male','NewYork11','01015827875'),(21,'Rachel','female','NewYork22','01015827749'),(22,'Monica','female','NewYork','010158996743');
   Replace INTO  exmp_tb1 (tb1_id,tb1_name,tb1_gender,tb1_address,tb1_number,tb1_date) VALUES(17,'David2',DEFAULT,'NewYork11','01015827875',DEFAULT),(18,'Rachel','female',DEFAULT,'01015827749','2018-12-14 10:44:20'),(DEFAULT,'Monica','female',DEFAULT,DEFAULT,'2018-12-14 10:44:20');
   Replace INTO  exmp_tb1 VALUES(DEFAULT,'David',DEFAULT,'NewYork11','01015827875',DEFAULT),(18,'Rachel','female',DEFAULT,'01015827749','2018-12-14 10:44:20'),(DEFAULT,'Monica','female',DEFAULT,DEFAULT,'2018-12-14 10:44:20');

**Output**

::

   -- If data is available, replacement will be performed. Otherwise, INSERT will be performed.
   INSERT INTO "public"."exmp_tb1" ("tb1_id","tb1_name","tb1_gender","tb1_address","tb1_number") VALUES (17,'David','male','NewYork11','01015827875');
   INSERT INTO "public"."exmp_tb1" ("tb1_id","tb1_name","tb1_gender","tb1_address","tb1_number") VALUES (18,'Rachel','female','NewYork22','01015827749');
   INSERT INTO "public"."exmp_tb1" ("tb1_id","tb1_name","tb1_gender","tb1_address","tb1_number") VALUES (20,'Monica','female','NewYork','010158996743');
   INSERT INTO "public"."exmp_tb1" ("tb1_id","tb1_name","tb1_gender","tb1_address","tb1_number") VALUES (17,'David1','male','NewYork11','01015827875');
   INSERT INTO "public"."exmp_tb1" ("tb1_id","tb1_name","tb1_gender","tb1_address","tb1_number") VALUES (21,'Rachel','female','NewYork22','01015827749');
   INSERT INTO "public"."exmp_tb1" ("tb1_id","tb1_name","tb1_gender","tb1_address","tb1_number") VALUES (22,'Monica','female','NewYork','010158996743');
   INSERT INTO "public"."exmp_tb1" ("tb1_id","tb1_name","tb1_gender","tb1_address","tb1_number","tb1_date") VALUES (17,'David2',DEFAULT,'NewYork11','01015827875',DEFAULT);
   INSERT INTO "public"."exmp_tb1" ("tb1_id","tb1_name","tb1_gender","tb1_address","tb1_number","tb1_date") VALUES (18,'Rachel','female',DEFAULT,'01015827749','2018-12-14 10:44:20');
   INSERT INTO "public"."exmp_tb1" ("tb1_id","tb1_name","tb1_gender","tb1_address","tb1_number","tb1_date") VALUES (DEFAULT,'Monica','female',DEFAULT,DEFAULT,'2018-12-14 10:44:20');
   INSERT INTO "public"."exmp_tb1"  VALUES (DEFAULT,'David',DEFAULT,'NewYork11','01015827875',DEFAULT);
   INSERT INTO "public"."exmp_tb1"  VALUES (18,'Rachel','female',DEFAULT,'01015827749','2018-12-14 10:44:20');
   INSERT INTO "public"."exmp_tb1"  VALUES (DEFAULT,'Monica','female',DEFAULT,DEFAULT,'2018-12-14 10:44:20');

.. _en-us_topic_0000001860318485__en-us_topic_0000001432447489_section138749424413:

SET
---

**MySQL REPLACE** supports the use of **SET** settings, which DSC will convert.

**Input**

::

   replace INTO `runoob_datatype_test` VALUES (100, 100, 100, 0, 1);
   replace INTO `runoob_datatype_test` VALUES (100.23, 100.25, 100.26, 0.12,1.5);
   replace INTO `runoob_datatype_test` (dataType_numeric,dataType_numeric1) VALUES (100.23, 100.25);
   replace INTO `runoob_datatype_test` (dataType_numeric,dataType_numeric1,dataType_numeric2) VALUES (100.23, 100.25, 2.34);
   replace into runoob_datatype_test set dataType_numeric=23.1, dataType_numeric4 = 25.12 ;

**Output**

::

   INSERT INTO "public"."runoob_datatype_test"  VALUES (100,100,100,0,1);
   INSERT INTO "public"."runoob_datatype_test"  VALUES (100.23,100.25,100.26,0.12,1.5);
   INSERT INTO "public"."runoob_datatype_test" ("datatype_numeric","datatype_numeric1") VALUES (100.23,100.25);
   INSERT INTO "public"."runoob_datatype_test" ("datatype_numeric","datatype_numeric1","datatype_numeric2") VALUES (100.23,100.25,2.34);
   INSERT INTO "public"."runoob_datatype_test" ("datatype_numeric","datatype_numeric4") VALUES (23.1,25.12);
