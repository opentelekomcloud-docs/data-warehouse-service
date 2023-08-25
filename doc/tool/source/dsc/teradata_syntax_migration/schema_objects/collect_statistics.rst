:original_name: dws_mt_0066.html

.. _dws_mt_0066:

COLLECT STATISTICS
==================

**COLLECT STAT** is used in Teradata for collecting optimizer statistics, which will be used for query performance. GaussDB(DWS) uses the **ANALYZE** statement to replace the **COLLECT STAT** statement.

For details, see :ref:`1 <en-us_topic_0000001234200625__en-us_topic_0238518363_en-us_topic_0237362323_li7975713452>`.

**Input - COLLECT STATISTICS**

::

   COLLECT STAT tab1 COLUMN (c1, c2);

**Output**

::

   ANALYZE tab1 (c1, c2);

**Input - COLLECT STATISTICS**

::

   COLLECT STATISTICS
     COLUMN (customer_id,customer_name)
   , COLUMN (postal_code)
   , COLUMN (customer_address)
   ON customer_t;

**Output**

::

   ANALYZE customer_t (
        customer_id
        ,customer_name
        ,postal_code
        ,customer_address
   )
   ;

**Input - COLLECT STATISTICS with COLUMN**

::

   COLLECT STATISTICS
     COLUMN (
     Order_Date
     -- ,o_orderID
   /*COLLECT
   STATISTICS*/
     ,Order_ID
        )
   ON order_t;

**Output**

::

   ANALYZE order_t (
       Order_Date
        ,Order_ID
   )
   ;

**Input - COLLECT STATISTICS with Schema Name**

::

   COLLECT STATS COLUMN (
        empno
        ,ename
   )
        ON ${schemaname}."usrTab1"
   ;

**Output**

::

   ANALYZE ${schemaname}."usrTab1"
   (
        empno
        ,ename
   )
   ;


COLLECT STATISTICS
------------------

Collect statistics based on sampling percentage.

**Input**

.. code-block::

   COLLECT STATISTICS
    USING SAMPLE 5.00 PERCENT
    COLUMN ( CDR_TYPE_KEY ) ,
    COLUMN ( PARTITION ) ,
    COLUMN ( SRC ) ,
    COLUMN ( PARTITION,SBSCRPN_KEY )
    ON DT_SDM.FCT_OTGO_NTWK_ACTVY_DAILY ;

**Output**

.. code-block::

   SET
   default_statistics_target = 5.00 ;
   ANALYZE DT_SDM.FCT_OTGO_NTWK_ACTVY_DAILY (CDR_TYPE_KEY) ;
   ANALYZE DT_SDM.FCT_OTGO_NTWK_ACTVY_DAILY (PARTITION) ;
   ANALYZE DT_SDM.FCT_OTGO_NTWK_ACTVY_DAILY (SRC) ;
   ANALYZE DT_SDM.FCT_OTGO_NTWK_ACTVY_DAILY (PARTITION,SBSCRPN_KEY) ;
    RESET default_statistics_target ;
