:original_name: dws_16_0199.html

.. _dws_16_0199:

.. _en-us_topic_0000001772536568:

LOW_PRIORITY
============

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
