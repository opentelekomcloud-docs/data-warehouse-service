:original_name: dws_16_0184.html

.. _dws_16_0184:

.. _en-us_topic_0000001860198669:

INSERT
======

In MySQL, **INSERT** allows the following keywords: **HIGH_PRIORITY**, **LOW_PRIORITY**, **PARTITION**, **DELAYED**, **IGNORE**, **VALUES**, and **ON DUPLICATE KEY UPDATE**.

.. _en-us_topic_0000001860198669__en-us_topic_0000001382527670_section161721845173119:

HIGH_PRIORITY
-------------

MySQL uses **HIGH_PRIORITY** will override the effect of the **LOW_PRIORITY** option.

**Input**

::

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

::

   -- HIGH_PRIORITY
   INSERT INTO "public"."exmp_tb2"  VALUES (100,12.3,'cheap','2018-11-11');
   INSERT INTO "public"."exmp_tb2"  VALUES (DEFAULT,128.23,'nice','2018-10-11');
   INSERT INTO "public"."exmp_tb2"  VALUES (DEFAULT,DEFAULT,'nice','2018-12-14');
   INSERT INTO "public"."exmp_tb2"  VALUES (DEFAULT,DEFAULT,'nice',DEFAULT);
   INSERT INTO "public"."exmp_tb2" ("tb2_id","tb2_price") VALUES (DEFAULT,DEFAULT);
   INSERT INTO "public"."exmp_tb2" ("tb2_id","tb2_price","tb2_note") VALUES (DEFAULT,DEFAULT,DEFAULT);
   INSERT INTO "public"."exmp_tb2" ("tb2_id","tb2_price","tb2_note") VALUES (DEFAULT,DEFAULT,DEFAULT);
   INSERT INTO "public"."exmp_tb2" ("tb2_id","tb2_price","tb2_note","tb2_date") VALUES (DEFAULT,DEFAULT,DEFAULT,DEFAULT);

.. _en-us_topic_0000001860198669__en-us_topic_0000001382527670_section123153113216:

LOW_PRIORITY
------------

When the **LOW_PRIORITY** modifier is used, execution of **INSERT** is delayed.

**Input**

::

   # LOW_PRIORITY
   INSERT LOW_PRIORITY INTO  exmp_tb2 VALUES( DEFAULT, '128.23', 'nice', '2018-10-11');
   INSERT LOW_PRIORITY INTO  exmp_tb2 VALUES(DEFAULT, DEFAULT, 'nice', '2018-12-14' );
   INSERT LOW_PRIORITY INTO  exmp_tb2 VALUES(DEFAULT, DEFAULT, 'nice', DEFAULT);
   INSERT LOW_PRIORITY INTO  exmp_tb2 (tb2_id, tb2_price) VALUES(DEFAULT, DEFAULT);
   INSERT LOW_PRIORITY INTO  exmp_tb2 (tb2_id, tb2_price, tb2_note) VALUES(DEFAULT, DEFAULT, DEFAULT);

**Output**

::

   -- LOW_PRIORITY
   INSERT INTO "public"."exmp_tb2"  VALUES (DEFAULT,'128.23','nice','2018-10-11');
   INSERT INTO "public"."exmp_tb2"  VALUES (DEFAULT,DEFAULT,'nice','2018-12-14');
   INSERT INTO "public"."exmp_tb2"  VALUES (DEFAULT,DEFAULT,'nice',DEFAULT);
   INSERT INTO "public"."exmp_tb2" ("tb2_id","tb2_price") VALUES (DEFAULT,DEFAULT);
   INSERT INTO "public"."exmp_tb2" ("tb2_id","tb2_price","tb2_note") VALUES (DEFAULT,DEFAULT,DEFAULT);

.. _en-us_topic_0000001860198669__en-us_topic_0000001382527670_section57981355335:

PARTITION
---------

When inserting into a partitioned table, you can control which partitions and subpartitions accept new rows.

**Input**

::

   INSERT INTO employees PARTITION(p3) VALUES (19, 'Frank1', 'Williams', 1, 2);
   INSERT INTO employees PARTITION(p0) VALUES (4, 'Frank1', 'Williams', 1, 2);
   INSERT INTO employees PARTITION(p1) VALUES (9, 'Frank1', 'Williams', 1, 2);
   INSERT INTO employees PARTITION(p2) VALUES (10, 'Frank1', 'Williams', 1, 2);
   INSERT INTO employees PARTITION(p2) VALUES (11, 'Frank1', 'Williams', 1, 2);

**Output**

::

   INSERT INTO "public"."employees"  VALUES (19,'Frank1','Williams',1,2);
   INSERT INTO "public"."employees"  VALUES (4,'Frank1','Williams',1,2);
   INSERT INTO "public"."employees"  VALUES (9,'Frank1','Williams',1,2);
   INSERT INTO "public"."employees"  VALUES (10,'Frank1','Williams',1,2);
   INSERT INTO "public"."employees"  VALUES (11,'Frank1','Williams',1,2);

.. _en-us_topic_0000001860198669__en-us_topic_0000001382527670_section4855132113311:

DELAYED
-------

.. important::

   In MySQL 5.7, the **DELAYED** keyword is recognized but ignored by the server.

**Input**

::

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

::

   -- DELAYED
   INSERT INTO "public"."exmp_tb2"  VALUES (99,15.68,'good','2018-11-12');
   INSERT INTO "public"."exmp_tb2"  VALUES (80,12.3,'cheap','2018-11-11');
   INSERT INTO "public"."exmp_tb2"  VALUES (DEFAULT,128.23,'nice','2018-10-11');
   INSERT INTO "public"."exmp_tb2"  VALUES (DEFAULT,DEFAULT,'nice','2018-12-14');
   INSERT INTO "public"."exmp_tb2"  VALUES (DEFAULT,DEFAULT,'nice',DEFAULT);
   INSERT INTO "public"."exmp_tb2" ("tb2_id","tb2_price") VALUES (DEFAULT,DEFAULT);
   INSERT INTO "public"."exmp_tb2" ("tb2_id","tb2_price","tb2_note") VALUES (DEFAULT,DEFAULT,DEFAULT);
   INSERT INTO "public"."exmp_tb2" ("tb2_id","tb2_price","tb2_note","tb2_date") VALUES (DEFAULT,DEFAULT,DEFAULT,DEFAULT);

.. _en-us_topic_0000001860198669__en-us_topic_0000001382527670_section010446153414:

IGNORE
------

When the **IGNORE** modifier is used, errors that occur during **INSERT** execution are ignored.

**Input**

::

   # New data will be ignored if there is duplicate in the table.
   INSERT IGNORE INTO  exmp_tb2 VALUES(189, '189.23','nice','2017-11-12');
   INSERT IGNORE INTO  exmp_tb2 VALUES(130,'189.23','nice','2017-11-12');
   INSERT IGNORE INTO  exmp_tb2 VALUES(120,15.68,'good','2018-11-12');
   INSERT IGNORE INTO  exmp_tb2 VALUES(DEFAULT,128.23,'nice','2018-10-11');
   INSERT IGNORE INTO  exmp_tb2 VALUES(DEFAULT,DEFAULT,'nice','2018-12-14');
   INSERT IGNORE INTO  exmp_tb2 VALUES(DEFAULT,DEFAULT,'nice',DEFAULT);
   INSERT IGNORE INTO  exmp_tb2 (tb2_id,tb2_price) VALUES(DEFAULT,DEFAULT);
   INSERT IGNORE INTO  exmp_tb2 (tb2_id,tb2_price,tb2_note) VALUES(DEFAULT,DEFAULT,DEFAULT);
   INSERT IGNORE INTO  exmp_tb2 (tb2_id,tb2_price,tb2_note,tb2_date) VALUES(DEFAULT,DEFAULT,DEFAULT,DEFAULT);

**Output**

::

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

.. _en-us_topic_0000001860198669__en-us_topic_0000001382527670_section6667221103611:

VALUES
------

**INSERT** statements that use the **VALUES** syntax can insert multiple lines, separated by commas.

**Input**

::

   INSERT INTO  exmp_tb1 (tb1_name,tb1_gender,tb1_address,tb1_number) VALUES('David','male','NewYork','01015827875'),('Rachel','female','NewYork','01015827749'),('Monica','female','NewYork','010158996743');

**Output**

::

   INSERT INTO "public"."exmp_tb1" ("tb1_name","tb1_gender","tb1_address","tb1_number") VALUES ('David','male','NewYork','01015827875');
   INSERT INTO "public"."exmp_tb1" ("tb1_name","tb1_gender","tb1_address","tb1_number") VALUES ('Rachel','female','NewYork','01015827749');
   INSERT INTO "public"."exmp_tb1" ("tb1_name","tb1_gender","tb1_address","tb1_number") VALUES ('Monica','female','NewYork','010158996743');

.. _en-us_topic_0000001860198669__en-us_topic_0000001382527670_section7107114883613:

ON DUPLICATE KEY UPDATE
-----------------------

**INSERT** uses the **ON DUPLICATE KEY UPDATE** clause to update existing rows.

**Input**

::

   # ON DUPLICATE KEY UPDATE (If new data will cause a duplicate value in the primary/unique key, UPDATE will work. Otherwise, INSERT will work.)
   INSERT INTO  exmp_tb2(tb2_id,tb2_price) VALUES(3,12.3) ON DUPLICATE KEY UPDATE tb2_price=12.3;
   INSERT INTO  exmp_tb2(tb2_id,tb2_price) VALUES(4,12.3) ON DUPLICATE KEY UPDATE tb2_price=12.3;
   INSERT INTO  exmp_tb2(tb2_id,tb2_price,tb2_note) VALUES(10,DEFAULT,DEFAULT) ON DUPLICATE KEY UPDATE tb2_price=66.6;
   INSERT INTO  exmp_tb2(tb2_id,tb2_price,tb2_note,tb2_date) VALUES(11,DEFAULT,DEFAULT,DEFAULT) ON DUPLICATE KEY UPDATE tb2_price=66.6;

**Output**

::

   -- ON DUPLICATE KEY UPDATE (If new data will cause a duplicate value in the primary/unique key, UPDATE will work. Otherwise, INSERT will work.)
   INSERT INTO "public"."exmp_tb2" ("tb2_id","tb2_price") VALUES (3,12.3);
   INSERT INTO "public"."exmp_tb2" ("tb2_id","tb2_price") VALUES (4,12.3);
   INSERT INTO "public"."exmp_tb2" ("tb2_id","tb2_price","tb2_note") VALUES (10,DEFAULT,DEFAULT);
   INSERT INTO "public"."exmp_tb2" ("tb2_id","tb2_price","tb2_note","tb2_date") VALUES (11,DEFAULT,DEFAULT,DEFAULT);

.. _en-us_topic_0000001860198669__en-us_topic_0000001382527670_section16566172713372:

SET
---

**MySQL INSERT...SET** statement inserts rows based on explicitly specified values.

**Input**

::

   # INSERT INTO  SET (Data records can be inserted specially. One record can be inserted at a time and batch insertion is not supported.)
   INSERT INTO  exmp_tb2 SET tb2_price=56.1,tb2_note='unbelievable',tb2_date='2018-11-13';
   INSERT INTO  exmp_tb2 SET tb2_price=99.9,tb2_note='perfect',tb2_date='2018-10-13';
   INSERT INTO  exmp_tb2 SET tb2_id=9,tb2_price=99.9,tb2_note='perfect',tb2_date='2018-10-13';

**Output**

::

   -- INSERT INTO  SET (Data records can be inserted specially. One record can be inserted at a time, and batch insertion is not supported.)
   INSERT INTO "public"."exmp_tb2" ("tb2_price","tb2_note","tb2_date") VALUES (56.1,'unbelievable','2018-11-13');
   INSERT INTO "public"."exmp_tb2" ("tb2_price","tb2_note","tb2_date") VALUES (99.9,'perfect','2018-10-13');
   INSERT INTO "public"."exmp_tb2" ("tb2_id","tb2_price","tb2_note","tb2_date") VALUES (9,99.9,'perfect','2018-10-13');
