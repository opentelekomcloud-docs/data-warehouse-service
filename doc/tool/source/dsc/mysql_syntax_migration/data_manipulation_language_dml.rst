:original_name: dws_07_8313.html

.. _dws_07_8313:

Data Manipulation Language (DML)
================================

This section describes the migration syntax of MySQL DML. The migration syntax determines how the keywords and features are migrated.

For details, see the following topics:

-  :ref:`INSERT <en-us_topic_0000001234200629__en-us_topic_0238518451_en-us_topic_0237362190_section1449751153020>`
-  :ref:`UPDATE <en-us_topic_0000001234200629__en-us_topic_0238518451_en-us_topic_0237362190_section68760571318>`
-  :ref:`REPLACE <en-us_topic_0000001234200629__en-us_topic_0238518451_en-us_topic_0237362190_section234585863114>`

.. _en-us_topic_0000001234200629__en-us_topic_0238518451_en-us_topic_0237362190_section1449751153020:

INSERT
------

In MySQL, **INSERT** allows the following keywords: **HIGH_PRIORITY**, **LOW_PRIORITY**, **PARTITION**, **DELAYED**, **IGNORE**, **VALUES**, and **ON DUPLICATE KEY UPDATE**. GaussDB(DWS) does not support these keywords, and DSC will convert them.

#. **HIGH_PRIORITY**

   MySQL uses HIGH_PRIORITY will override the effect of the LOW_PRIORITY option.

   **Input**

   .. code-block::

      # HIGH_PRIORITY
      INSERT HIGH_PRIORITY INTO  exmp_tb2 VALUES(100, 12.3, 'cheap', '2018-11-11');
      INSERT HIGH_PRIORITY INTO  exmp_tb2 VALUES(DEFAULT, 128.23, 'nice', '2018-10-11');
      INSERT HIGH_PRIORITY INTO  exmp_tb2 VALUES(DEFAULT, DEFAULT, 'nice', '2018-12-14');
      INSERT HIGH_PRIORITY INTO  exmp_tb2 VALUES(DEFAULT, DEFAULT, 'nice', DEFAULT);
      INSERT HIGH_PRIORITY INTO  exmp_tb2 (tb2_id, tb2_price) VALUES(DEFAULT, DEFAULT);
      INSERT HIGH_PRIORITY INTO  exmp_tb2 (tb2_id, tb2_price, tb2_note) VALUES(DEFAULT, DEFAULT, DEFAULT);
      INSERT HIGH_PRIORITY INTO  exmp_tb2 (tb2_id, tb2_price , tb2_note) VALUES(DEFAULT, DEFAULT, DEFAULT);
      INSERT HIGH_PRIORITY INTO  exmp_tb2 (tb2_id, tb2_price, tb2_note, tb2_date) VALUES(DEFAULT, DEFAULT, DEFAULT, DEFAULT);

   **Output**

   .. code-block::

      -- HIGH_PRIORITY
      INSERT INTO "public"."exmp_tb2"  VALUES (100,12.3,'cheap','2018-11-11');
      INSERT INTO "public"."exmp_tb2"  VALUES (DEFAULT,128.23,'nice','2018-10-11');
      INSERT INTO "public"."exmp_tb2"  VALUES (DEFAULT,DEFAULT,'nice','2018-12-14');
      INSERT INTO "public"."exmp_tb2"  VALUES (DEFAULT,DEFAULT,'nice',DEFAULT);
      INSERT INTO "public"."exmp_tb2" ("tb2_id","tb2_price") VALUES (DEFAULT,DEFAULT);
      INSERT INTO "public"."exmp_tb2" ("tb2_id","tb2_price","tb2_note") VALUES (DEFAULT,DEFAULT,DEFAULT);
      INSERT INTO "public"."exmp_tb2" ("tb2_id","tb2_price","tb2_note") VALUES (DEFAULT,DEFAULT,DEFAULT);
      INSERT INTO "public"."exmp_tb2" ("tb2_id","tb2_price","tb2_note","tb2_date") VALUES (DEFAULT,DEFAULT,DEFAULT,DEFAULT);

#. **LOW_PRIORITY**

   When the **LOW_PRIORITY** modifier is used, execution of **INSERT** is delayed.

   **Input**

   .. code-block::

      # LOW_PRIORITY
      INSERT LOW_PRIORITY INTO  exmp_tb2 VALUES( DEFAULT, '128.23', 'nice', '2018-10-11');
      INSERT LOW_PRIORITY INTO  exmp_tb2 VALUES(DEFAULT, DEFAULT, 'nice', '2018-12-14' );
      INSERT LOW_PRIORITY INTO  exmp_tb2 VALUES(DEFAULT, DEFAULT, 'nice', DEFAULT);
      INSERT LOW_PRIORITY INTO  exmp_tb2 (tb2_id, tb2_price) VALUES(DEFAULT, DEFAULT);
      INSERT LOW_PRIORITY INTO  exmp_tb2 (tb2_id, tb2_price, tb2_note) VALUES(DEFAULT, DEFAULT, DEFAULT);

   **Output**

   .. code-block::

      -- LOW_PRIORITY
      INSERT INTO "public"."exmp_tb2"  VALUES (DEFAULT,'128.23','nice','2018-10-11');
      INSERT INTO "public"."exmp_tb2"  VALUES (DEFAULT,DEFAULT,'nice','2018-12-14');
      INSERT INTO "public"."exmp_tb2"  VALUES (DEFAULT,DEFAULT,'nice',DEFAULT);
      INSERT INTO "public"."exmp_tb2" ("tb2_id","tb2_price") VALUES (DEFAULT,DEFAULT);
      INSERT INTO "public"."exmp_tb2" ("tb2_id","tb2_price","tb2_note") VALUES (DEFAULT,DEFAULT,DEFAULT);

#. **PRATITION**

   When inserting into a partitioned table, you can control which partitions and subpartitions accept new rows.

   **Input**

   .. code-block::

      INSERT INTO employees PARTITION(p3) VALUES (19, 'Frank1', 'Williams', 1, 2);
      INSERT INTO employees PARTITION(p0) VALUES (4, 'Frank1', 'Williams', 1, 2);
      INSERT INTO employees PARTITION(p1) VALUES (9, 'Frank1', 'Williams', 1, 2);
      INSERT INTO employees PARTITION(p2) VALUES (10, 'Frank1', 'Williams', 1, 2);
      INSERT INTO employees PARTITION(p2) VALUES (11, 'Frank1', 'Williams', 1, 2);

   **Output**

   .. code-block::

      INSERT INTO "public"."employees"  VALUES (19,'Frank1','Williams',1,2);
      INSERT INTO "public"."employees"  VALUES (4,'Frank1','Williams',1,2);
      INSERT INTO "public"."employees"  VALUES (9,'Frank1','Williams',1,2);
      INSERT INTO "public"."employees"  VALUES (10,'Frank1','Williams',1,2);
      INSERT INTO "public"."employees"  VALUES (11,'Frank1','Williams',1,2);

#. **DELAYED**

   .. important::

      In MySQL 5.7, the **DELAYED** keyword is recognized but ignored by the server.

   **Input**

   .. code-block::

      # DELAYED
      INSERT DELAYED INTO  exmp_tb2 VALUES(99, 15.68, 'good', '2018-11-12');
      INSERT DELAYED INTO  exmp_tb2 VALUES(80, 12.3, 'cheap', '2018-11-11');
      INSERT DELAYED INTO  exmp_tb2 VALUES(DEFAULT, 128.23, 'nice', '2018-10-11');
      INSERT DELAYED INTO  exmp_tb2 VALUES(DEFAULT, DEFAULT, 'nice', '2018-12-14');
      INSERT DELAYED INTO  exmp_tb2 VALUES(DEFAULT, DEFAULT, 'nice', DEFAULT);
      INSERT DELAYED INTO  exmp_tb2 (tb2_id, tb2_price) VALUES(DEFAULT, DEFAULT);
      INSERT DELAYED INTO  exmp_tb2 (tb2_id, tb2_price, tb2_note) VALUES(DEFAULT, DEFAULT, DEFAULT);
      INSERT DELAYED INTO  exmp_tb2 (tb2_id, tb2_price, tb2_note, tb2_date) VALUES(DEFAULT, DEFAULT, DEFAULT, DEFAULT);

   **Output**

   .. code-block::

      -- DELAYED
      INSERT INTO "public"."exmp_tb2"  VALUES (99,15.68,'good','2018-11-12');
      INSERT INTO "public"."exmp_tb2"  VALUES (80,12.3,'cheap','2018-11-11');
      INSERT INTO "public"."exmp_tb2"  VALUES (DEFAULT,128.23,'nice','2018-10-11');
      INSERT INTO "public"."exmp_tb2"  VALUES (DEFAULT,DEFAULT,'nice','2018-12-14');
      INSERT INTO "public"."exmp_tb2"  VALUES (DEFAULT,DEFAULT,'nice',DEFAULT);
      INSERT INTO "public"."exmp_tb2" ("tb2_id","tb2_price") VALUES (DEFAULT,DEFAULT);
      INSERT INTO "public"."exmp_tb2" ("tb2_id","tb2_price","tb2_note") VALUES (DEFAULT,DEFAULT,DEFAULT);
      INSERT INTO "public"."exmp_tb2" ("tb2_id","tb2_price","tb2_note","tb2_date") VALUES (DEFAULT,DEFAULT,DEFAULT,DEFAULT);

#. **IGNORE**

   When the **IGNORE** modifier is used, errors that occur during **INSERT** execution are ignored.

   **Input**

   .. code-block::

      # New data will be ignored if there is duplicate in the table.
      INSERT IGNORE INTO  exmp_tb2 VALUES(189, '189.23','nice','2017-11-12');
      INSERT IGNORE INTO  exmp_tb2 VALUES(130,'189.23','nice','2017-11-12');
      INSERT IGNORE INTO  exmp_tb2 VALUES(120,15.68,'good','2018-11-12');
      INSERT IGNORE INTO  exmp_tb2 VALUES(DEFAULT,128.23,'nice','2018-10-11');
      INSERT IGNORE INTO  exmp_tb2 VALUES(DEFAULT,DEFAULT,'nice','2018-12-14');
      INSERT IGNORE INTO  exmp_tb2 VALUES(DEFAULT,DEFAULT,'nice',DEFAULT);test
      INSERT IGNORE INTO  exmp_tb2 (tb2_id,tb2_price) VALUES(DEFAULT,DEFAULT);
      INSERT IGNORE INTO  exmp_tb2 (tb2_id,tb2_price,tb2_note) VALUES(DEFAULT,DEFAULT,DEFAULT);
      INSERT IGNORE INTO  exmp_tb2 (tb2_id,tb2_price,tb2_note,tb2_date) VALUES(DEFAULT,DEFAULT,DEFAULT,DEFAULT);

   **Output**

   .. code-block::

      -- New data will be ignored if there is duplicate in the table.
      INSERT INTO "public"."exmp_tb2"  VALUES (101,'189.23','nice','2017-11-12');
      INSERT INTO "public"."exmp_tb2"  VALUES (130,'189.23','nice','2017-11-12');
      INSERT INTO "public"."exmp_tb2"  VALUES (120,15.68,'good','2018-11-12');
      INSERT INTO "public"."exmp_tb2"  VALUES (DEFAULT,128.23,'nice','2018-10-11');
      INSERT INTO "public"."exmp_tb2"  VALUES (DEFAULT,DEFAULT,'nice','2018-12-14');
      INSERT INTO "public"."exmp_tb2"  VALUES (DEFAULT,DEFAULT,'nice',DEFAULT);
      INSERT INTO "public"."exmp_tb2" ("tb2_id","tb2_price") VALUES (DEFAULT,DEFAULT);
      INSERT INTO "public"."exmp_tb2" ("tb2_id","tb2_price","tb2_note") VALUES (DEFAULT,DEFAULT,DEFAULT);
      INSERT INTO "public"."exmp_tb2" ("tb2_id","tb2_price","tb2_note","tb2_date") VALUES (DEFAULT,DEFAULT,DEFAULT,DEFAULT);

#. **VALUES**

   INSERT statements use the VALUES syntax can insert multiple lines, separated by commas.

   **Input**

   .. code-block::

      INSERT INTO  exmp_tb1 (tb1_name,tb1_sex,tb1_address,tb1_number) VALUES('David','male','NewYork','01015827875'),('Rachel','female','NewYork','01015827749'),('Monica','female','NewYork','010158996743');

   **Output**

   .. code-block::

      INSERT INTO "public"."exmp_tb1" ("tb1_name","tb1_sex","tb1_address","tb1_number") VALUES ('David','male','NewYork','01015827875');
      INSERT INTO "public"."exmp_tb1" ("tb1_name","tb1_sex","tb1_address","tb1_number") VALUES ('Rachel','female','NewYork','01015827749');
      INSERT INTO "public"."exmp_tb1" ("tb1_name","tb1_sex","tb1_address","tb1_number") VALUES ('Monica','female','NewYork','010158996743');

#. **ON DUPLICATE KEY UPDATE**

   INSERT uses the ON DUPLICATE KEY UPDATE clause to update existing rows.

   **Input**

   .. code-block::

      # ON DUPLICATE KEY UPDATE (If new data will cause a duplicate value in the primary/unique key, UPDATE will work. Otherwise, INSERT will work.)
      INSERT INTO  exmp_tb2(tb2_id,tb2_price) VALUES(3,12.3) ON DUPLICATE KEY UPDATE tb2_price=12.3;
      INSERT INTO  exmp_tb2(tb2_id,tb2_price) VALUES(4,12.3) ON DUPLICATE KEY UPDATE tb2_price=12.3;
      INSERT INTO  exmp_tb2(tb2_id,tb2_price,tb2_note) VALUES(10,DEFAULT,DEFAULT) ON DUPLICATE KEY UPDATE tb2_price=66.6;
      INSERT INTO  exmp_tb2(tb2_id,tb2_price,tb2_note,tb2_date) VALUES(11,DEFAULT,DEFAULT,DEFAULT) ON DUPLICATE KEY UPDATE tb2_price=66.6;

   **Output**

   .. code-block::

      -- ON DUPLICATE KEY UPDATE (If new data will cause a duplicate value in the primary/unique key, UPDATE will work. Otherwise, INSERT will work.)
      INSERT INTO "public"."exmp_tb2" ("tb2_id","tb2_price") VALUES (3,12.3);
      INSERT INTO "public"."exmp_tb2" ("tb2_id","tb2_price") VALUES (4,12.3);
      INSERT INTO "public"."exmp_tb2" ("tb2_id","tb2_price","tb2_note") VALUES (10,DEFAULT,DEFAULT);
      INSERT INTO "public"."exmp_tb2" ("tb2_id","tb2_price","tb2_note","tb2_date") VALUES (11,DEFAULT,DEFAULT,DEFAULT);

#. **SET**

   MySQL INSERT...SET statement inserts rows based on explicitly specified values.

   **Input**

   .. code-block::

      # INSERT INTO  SET (Data records can be inserted specially. One record can be inserted at a time, and batch insertion is not supported.)
      INSERT INTO  exmp_tb2 SET tb2_price=56.1,tb2_note='unbelievable',tb2_date='2018-11-13';
      INSERT INTO  exmp_tb2 SET tb2_price=99.9,tb2_note='perfect',tb2_date='2018-10-13';
      INSERT INTO  exmp_tb2 SET tb2_id=9,tb2_price=99.9,tb2_note='perfect',tb2_date='2018-10-13';

   **Output**

   .. code-block::

      -- INSERT INTO  SET (Data records can be inserted specially. One record can be inserted at a time, and batch insertion is not supported.)
      INSERT INTO "public"."exmp_tb2" ("tb2_price","tb2_note","tb2_date") VALUES (56.1,'unbelievable','2018-11-13');
      INSERT INTO "public"."exmp_tb2" ("tb2_price","tb2_note","tb2_date") VALUES (99.9,'perfect','2018-10-13');
      INSERT INTO "public"."exmp_tb2" ("tb2_id","tb2_price","tb2_note","tb2_date") VALUES (9,99.9,'perfect','2018-10-13');

.. _en-us_topic_0000001234200629__en-us_topic_0238518451_en-us_topic_0237362190_section68760571318:

UPDATE
------

In MySQL, **UPDATE** allows the following keywords: **LOW_PRIORITY**, **ORDER BY**, **LIMIT**, and **IGNORE**. GaussDB(DWS) does not support these keywords, and DSC will convert them.

#. **LOW_PRIORITY**

   With the **LOW_PRIORITY** modifier, execution of **UPDATE** is delayed.

   **Input**

   .. code-block::

      # LOW_PRIORITY
      UPDATE LOW_PRIORITY employees SET department_id=2;

   **Output**

   .. code-block::

      -- LOW_PRIORITY
      UPDATE "public"."employees" SET "department_id" = 2;

#. **ORDER_BY**

   In MySQL, if an **UPDATE** statement includes an **ORDER BY** clause, the rows will be updated in the order specified by the clause.

   **Input**

   .. code-block::

      # ORDER BY
      UPDATE  employees SET department_id=department_id+1  ORDER BY id;

   **Output**

   .. code-block::

      -- ORDER BY
      UPDATE "public"."employees" SET "department_id" = department_id+1;

#. **LIMIT**

   UPDATE LIMIT syntax can be used to limit the scope. A clause is a limit on row matching. As long as the rows that satisfy the clause are found, the statements will stop, regardless of whether they have actually changed.

   **Input**

   .. code-block::

      # LIMIT
      UPDATE  employees SET department_id=department_id+1   LIMIT 3 ;
      UPDATE  employees SET department_id=department_id+1   LIMIT 3 , 10 ;

      # LIMIT + OFFSET
      UPDATE  employees SET department_id=department_id+1   LIMIT 3   OFFSET 2;

      # LIMIT + ORDER BY
      UPDATE  employees SET department_id=department_id+1 ORDER BY fname  LIMIT 3 ;

      # LIMIT + WHERE + ORDER BY
      UPDATE  employees SET department_id=department_id+1 WHERE id<5 ORDER BY  fname  LIMIT 3 ;

      # LIMIT + WHERE + ORDER BY + OFFSET
      UPDATE  employees SET department_id=department_id+1 WHERE id<5 ORDER BY  fname  LIMIT 3 OFFSET 2 ;

   **Output**

   .. code-block::

      -- LIMIT
      UPDATE "public"."employees" SET "department_id" = department_id+1;
      UPDATE "public"."employees" SET "department_id" = department_id+1;

      -- LIMIT + OFFSET
      UPDATE "public"."employees" SET "department_id" = department_id+1;

      -- LIMIT + ORDER BY
      UPDATE "public"."employees" SET "department_id" = department_id+1;

      -- LIMIT + WHERE + ORDER BY
      UPDATE "public"."employees" SET "department_id" = department_id+1 WHERE id<5;

      -- LIMIT + WHERE + ORDER BY + OFFSET
      UPDATE "public"."employees" SET "department_id" = department_id+1 WHERE id<5;

#. **IGNORE**

   With the **IGNORE** modifier, the **UPDATE** statement does not abort even if errors occur during execution.

   **Input**

   .. code-block::

      # IGNORE
      UPDATE IGNORE employees SET department_id=3;

   **Output**

   .. code-block::

      -- IGNORE
      UPDATE "public"."employees" SET "department_id" = 3;

.. _en-us_topic_0000001234200629__en-us_topic_0238518451_en-us_topic_0237362190_section234585863114:

REPLACE
-------

In MySQL, **REPLACE** allows the following keywords: **LOW_PRIORITY**, **PARTITION**, **DELAYED**, **VALUES**, and **SET**. The following examples are temporary migration solutions only.

.. note::

   **REPLACE** works exactly like **INSERT**, except that if an old row in the table has the same value as a new row for a primary key or unique index, the old row is deleted before the new row is inserted.

#. **LOW_PRIORITY**

   MySQL REPLACE supports the use of LOW_PRIORITY, which is converted by the Migration tool.

   **Input**

   .. code-block::

      # LOW_PRIORITY
      Replace LOW_PRIORITY INTO  exmp_tb2 VALUES(1, '128.23', 'nice', '2018-10-11 19:00:00');
      Replace LOW_PRIORITY INTO  exmp_tb2 VALUES(2, DEFAULT, 'nice', '2018-12-14 19:00:00' );
      Replace LOW_PRIORITY INTO  exmp_tb2 VALUES(3, DEFAULT, 'nice', DEFAULT);
      Replace LOW_PRIORITY INTO  exmp_tb2 (tb2_id, tb2_price) VALUES(5, DEFAULT);
      Replace LOW_PRIORITY INTO  exmp_tb2 (tb2_id, tb2_price, tb2_note) VALUES(4, DEFAULT, DEFAULT);

   **Output**

   .. code-block::

      -- LOW_PRIORITY
      INSERT INTO "public"."exmp_tb2"  VALUES (1,'128.23','nice','2018-10-11 19:00:00');
      INSERT INTO "public"."exmp_tb2"  VALUES (2,DEFAULT,'nice','2018-12-14 19:00:00');
      INSERT INTO "public"."exmp_tb2"  VALUES (3,DEFAULT,'nice',DEFAULT);
      INSERT INTO "public"."exmp_tb2" ("tb2_id","tb2_price") VALUES (5,DEFAULT);
      INSERT INTO "public"."exmp_tb2" ("tb2_id","tb2_price","tb2_note") VALUES (4,DEFAULT,DEFAULT);

#. **PARTITION**

   MySQL REPLACE supports explicit partitioning selection using the PARTITION keyword and a comma-separated name list for partitions, subpartitions, or both.

   **Input**

   .. code-block::

      replace INTO employees PARTITION(p3) VALUES (19, 'Frank1', 'Williams', 1, 2);
      replace INTO employees PARTITION(p0) VALUES (4, 'Frank1', 'Williams', 1, 2);
      replace INTO employees PARTITION(p1) VALUES (9, 'Frank1', 'Williams', 1, 2);
      replace INTO employees PARTITION(p2) VALUES (10, 'Frank1', 'Williams', 1, 2);
      replace INTO employees PARTITION(p2) VALUES (11, 'Frank1', 'Williams', 1, 2);

   **Output**

   .. code-block::

      INSERT INTO "public"."employees"  VALUES (19,'Frank1','Williams',1,2);
      INSERT INTO "public"."employees"  VALUES (4,'Frank1','Williams',1,2);
      INSERT INTO "public"."employees"  VALUES (9,'Frank1','Williams',1,2);
      INSERT INTO "public"."employees"  VALUES (10,'Frank1','Williams',1,2);
      INSERT INTO "public"."employees"  VALUES (11,'Frank1','Williams',1,2);

#. **DELAYED**

   .. warning::

      **DELAYED** **INSERT** and **REPLACE** operations were deprecated in MySQL 5.6. In MySQL 5.7, **DELAYED** was not supported. The server recognizes but ignores the **DELAYED** keyword, handles **REPLACE** as a non-delayed one, and generates an **ER_WARN_LEGACY_SYNTAX_CONVERTED** warning. (**REPLACE DELAYED** is no longer supported, and the statement is converted to **REPLACE**.) The keyword **DELAYED** will be deleted in later versions.

   **Input**

   .. code-block::

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

   .. code-block::

      --DELAYED INSERT DELAYED works only with MyISAM, MEMORY, ARCHIVE, and BLACKHOLE tables.
      --If you execute INSERT DELAYED with another storage engine,
      --you will get an error like this: ERROR 1616 (HY000): DELAYED option not supported.
      INSERT INTO "public"."exmp_tb2"  VALUES (10,128.23,'nice','2018-10-11 19:00:00');
      INSERT INTO "public"."exmp_tb2"  VALUES (6,DEFAULT,'nice','2018-12-14 19:00:00');
      INSERT INTO "public"."exmp_tb2"  VALUES (7,20,'nice',DEFAULT);
      INSERT INTO "public"."exmp_tb2" ("tb2_id","tb2_price") VALUES (11,DEFAULT);
      INSERT INTO "public"."exmp_tb2" ("tb2_id","tb2_price","tb2_note") VALUES (12,DEFAULT,DEFAULT);
      INSERT INTO "public"."exmp_tb2" ("tb2_id","tb2_price","tb2_note","tb2_date") VALUES (13,DEFAULT,DEFAULT,DEFAULT);

#. **VALUES**

   MySQL REPLACE supports a statement to insert or delete multiple values, separated by commas.

   **Input**

   .. code-block::

      # If data is available, replacement will be performed. Otherwise, insertion will be performed.
      Replace INTO  exmp_tb1 (tb1_id,tb1_name,tb1_sex,tb1_address,tb1_number) VALUES(17,'David','male','NewYork11','01015827875'),(18,'Rachel','female','NewYork22','01015827749'),(20,'Monica','female','NewYork','010158996743');
      Replace INTO  exmp_tb1 (tb1_id,tb1_name,tb1_sex,tb1_address,tb1_number) VALUES(17,'David1','male','NewYork11','01015827875'),(21,'Rachel','female','NewYork22','01015827749'),(22,'Monica','female','NewYork','010158996743');
      Replace INTO  exmp_tb1 (tb1_id,tb1_name,tb1_sex,tb1_address,tb1_number,tb1_date) VALUES(17,'David2',DEFAULT,'NewYork11','01015827875',DEFAULT),(18,'Rachel','female',DEFAULT,'01015827749','2018-12-14 10:44:20'),(DEFAULT,'Monica','female',DEFAULT,DEFAULT,'2018-12-14 10:44:20');
      Replace INTO  exmp_tb1 VALUES(DEFAULT,'David',DEFAULT,'NewYork11','01015827875',DEFAULT),(18,'Rachel','female',DEFAULT,'01015827749','2018-12-14 10:44:20'),(DEFAULT,'Monica','female',DEFAULT,DEFAULT,'2018-12-14 10:44:20');

   **Output**

   .. code-block::

      -- If data is available, replacement will be performed. Otherwise, insertion will be performed.
      INSERT INTO "public"."exmp_tb1" ("tb1_id","tb1_name","tb1_sex","tb1_address","tb1_number") VALUES (17,'David','male','NewYork11','01015827875');
      INSERT INTO "public"."exmp_tb1" ("tb1_id","tb1_name","tb1_sex","tb1_address","tb1_number") VALUES (18,'Rachel','female','NewYork22','01015827749');
      INSERT INTO "public"."exmp_tb1" ("tb1_id","tb1_name","tb1_sex","tb1_address","tb1_number") VALUES (20,'Monica','female','NewYork','010158996743');
      INSERT INTO "public"."exmp_tb1" ("tb1_id","tb1_name","tb1_sex","tb1_address","tb1_number") VALUES (17,'David1','male','NewYork11','01015827875');
      INSERT INTO "public"."exmp_tb1" ("tb1_id","tb1_name","tb1_sex","tb1_address","tb1_number") VALUES (21,'Rachel','female','NewYork22','01015827749');
      INSERT INTO "public"."exmp_tb1" ("tb1_id","tb1_name","tb1_sex","tb1_address","tb1_number") VALUES (22,'Monica','female','NewYork','010158996743');
      INSERT INTO "public"."exmp_tb1" ("tb1_id","tb1_name","tb1_sex","tb1_address","tb1_number","tb1_date") VALUES (17,'David2',DEFAULT,'NewYork11','01015827875',DEFAULT);
      INSERT INTO "public"."exmp_tb1" ("tb1_id","tb1_name","tb1_sex","tb1_address","tb1_number","tb1_date") VALUES (18,'Rachel','female',DEFAULT,'01015827749','2018-12-14 10:44:20');
      INSERT INTO "public"."exmp_tb1" ("tb1_id","tb1_name","tb1_sex","tb1_address","tb1_number","tb1_date") VALUES (DEFAULT,'Monica','female',DEFAULT,DEFAULT,'2018-12-14 10:44:20');
      INSERT INTO "public"."exmp_tb1"  VALUES (DEFAULT,'David',DEFAULT,'NewYork11','01015827875',DEFAULT);
      INSERT INTO "public"."exmp_tb1"  VALUES (18,'Rachel','female',DEFAULT,'01015827749','2018-12-14 10:44:20');
      INSERT INTO "public"."exmp_tb1"  VALUES (DEFAULT,'Monica','female',DEFAULT,DEFAULT,'2018-12-14 10:44:20');

#. **SET**

   MySQL REPLACE supports the use of SET settings, which the Migration tool will convert.

   **Input**

   .. code-block::

      replace INTO `runoob_datatype_test` VALUES (100, 100, 100, 0, 1);
      replace INTO `runoob_datatype_test` VALUES (100.23, 100.25, 100.26, 0.12,1.5);
      replace INTO `runoob_datatype_test` (dataType_numeric,dataType_numeric1) VALUES (100.23, 100.25);
      replace INTO `runoob_datatype_test` (dataType_numeric,dataType_numeric1,dataType_numeric2) VALUES (100.23, 100.25, 2.34);
      replace into runoob_datatype_test set dataType_numeric=23.1, dataType_numeric4 = 25.12 ;

   **Output**

   .. code-block::

      INSERT INTO "public"."runoob_datatype_test"  VALUES (100,100,100,0,1);
      INSERT INTO "public"."runoob_datatype_test"  VALUES (100.23,100.25,100.26,0.12,1.5);
      INSERT INTO "public"."runoob_datatype_test" ("datatype_numeric","datatype_numeric1") VALUES (100.23,100.25);
      INSERT INTO "public"."runoob_datatype_test" ("datatype_numeric","datatype_numeric1","datatype_numeric2") VALUES (100.23,100.25,2.34);
      INSERT INTO "public"."runoob_datatype_test" ("datatype_numeric","datatype_numeric4") VALUES (23.1,25.12);
