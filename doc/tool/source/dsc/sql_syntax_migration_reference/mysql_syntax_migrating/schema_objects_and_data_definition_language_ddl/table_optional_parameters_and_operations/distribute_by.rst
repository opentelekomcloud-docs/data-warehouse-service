:original_name: dws_16_0133.html

.. _dws_16_0133:

DISTRIBUTE BY
=============

ADB supports distribution keys. During DSC migration, the corresponding distribution keys are retained.

**Input**

.. code-block::

   CREATE TABLE COPY_DI_DISTRIBUTOR_BUYER_CONTRIBUTION_RANKING_V2 (
       SHOP_ID VARCHAR ,
       DISTRIBUTOR_ID VARCHAR ,
       BUYER_ID VARCHAR ,
       DATE_TYPE BIGINT ,
       PRIMARY KEY (SHOP_ID,DISTRIBUTOR_ID,DATE_TYPE,BUYER_ID)
   ) DISTRIBUTE BY HASH(SHOP_ID,DISTRIBUTOR_ID,DATE_TYPE);

**Output**

.. code-block::

   CREATE TABLE "public"."copy_di_distributor_buyer_contribution_ranking_v2" (
     "shop_id" VARCHAR,
     "distributor_id" VARCHAR,
     "buyer_id" VARCHAR,
     "date_type" BIGINT,
     PRIMARY KEY (
       "shop_id",
       "distributor_id",
       "date_type",
       "buyer_id"
     )
   ) WITH (ORIENTATION = ROW, COMPRESSION = NO) NOCOMPRESS DISTRIBUTE BY HASH (SHOP_ID, DISTRIBUTOR_ID, DATE_TYPE);
