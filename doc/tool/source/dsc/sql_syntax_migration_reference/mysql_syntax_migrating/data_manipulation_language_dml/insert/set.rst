:original_name: dws_16_0192.html

.. _dws_16_0192:

.. _en-us_topic_0000001819416281:

SET
===

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
