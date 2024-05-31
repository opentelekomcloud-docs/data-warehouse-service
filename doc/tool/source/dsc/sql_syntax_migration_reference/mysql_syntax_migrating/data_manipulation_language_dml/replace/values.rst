:original_name: dws_16_0202.html

.. _dws_16_0202:

.. _en-us_topic_0000001772696248:

VALUES
======

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
