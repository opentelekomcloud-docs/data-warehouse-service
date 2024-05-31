:original_name: dws_16_0189.html

.. _dws_16_0189:

.. _en-us_topic_0000001819336245:

IGNORE
======

When the **IGNORE** modifier is used, errors that occur during **INSERT** execution are ignored.

**Input**

.. code-block::

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
