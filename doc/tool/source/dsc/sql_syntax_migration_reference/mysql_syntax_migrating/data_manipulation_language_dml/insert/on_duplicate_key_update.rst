:original_name: dws_16_0191.html

.. _dws_16_0191:

.. _en-us_topic_0000001772536560:

ON DUPLICATE KEY UPDATE
=======================

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
