:original_name: dws_16_0190.html

.. _dws_16_0190:

.. _en-us_topic_0000001772696236:

VALUES
======

INSERT statements that use the VALUES syntax can insert multiple lines, separated by commas.

**Input**

.. code-block::

   INSERT INTO  exmp_tb1 (tb1_name,tb1_sex,tb1_address,tb1_number) VALUES('David','male','NewYork','01015827875'),('Rachel','female','NewYork','01015827749'),('Monica','female','NewYork','010158996743');

**Output**

.. code-block::

   INSERT INTO "public"."exmp_tb1" ("tb1_name","tb1_sex","tb1_address","tb1_number") VALUES ('David','male','NewYork','01015827875');
   INSERT INTO "public"."exmp_tb1" ("tb1_name","tb1_sex","tb1_address","tb1_number") VALUES ('Rachel','female','NewYork','01015827749');
   INSERT INTO "public"."exmp_tb1" ("tb1_name","tb1_sex","tb1_address","tb1_number") VALUES ('Monica','female','NewYork','010158996743');
