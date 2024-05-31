:original_name: dws_16_0201.html

.. _dws_16_0201:

.. _en-us_topic_0000001819336257:

DELAYED
=======

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
