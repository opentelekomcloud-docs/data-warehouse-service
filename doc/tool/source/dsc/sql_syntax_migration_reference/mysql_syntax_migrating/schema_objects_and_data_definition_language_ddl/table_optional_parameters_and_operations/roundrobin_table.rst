:original_name: dws_16_0160.html

.. _dws_16_0160:

ROUNDROBIN Table
================

GaussDB(DWS) supports the creation of roundrobin tables. You can set the :ref:`table.type <en-us_topic_0000001772696060__en-us_topic_0000001706105077_en-us_topic_0000001434418777_li15409381633>` in :ref:`Table 1 <en-us_topic_0000001772696060__en-us_topic_0000001706105077_en-us_topic_0000001434418777_table1740918360500>`. Set table.type=ROUND-ROBIN.

**Input**

.. code-block::

   CREATE TABLE charge_snapshot (
       id bigint NOT NULL,
       profit_model integer,
       ladder_rebate_rule text
   );

**Output**

.. code-block::

   CREATE TABLE "public"."charge_snapshot" (
     "id" BIGINT NOT NULL,
     "profit_model" INTEGER,
     "ladder_rebate_rule" TEXT
   ) WITH (ORIENTATION = ROW, COMPRESSION = NO) NOCOMPRESS DISTRIBUTE BY ROUNDROBIN;
