:original_name: dws_07_6845.html

.. _dws_07_6845:

Statement
=========

CURRENT SCHEMA
--------------

SET CURRENT SCHEMA

+-----------------------------------------+---------------------------------------+
| DB2 Syntax                              | Syntax After Migration                |
+=========================================+=======================================+
| .. code-block::                         | .. code-block::                       |
|                                         |                                       |
|    CREATE NICKNAME  sc2.emp FOR sc.emp; |    CREATE SYNONYM sc2.emp FOR sc.emp; |
+-----------------------------------------+---------------------------------------+

GET DIAGNOSTICS with ROW_COUNT
------------------------------

+-------------------------------------------------------------------------------------------------------------------------------+-----------------------------------+
| DB2 Syntax                                                                                                                    | Syntax After Migration            |
+===============================================================================================================================+===================================+
| .. code-block::                                                                                                               | .. code-block::                   |
|                                                                                                                               |                                   |
|    CC_ORDER_HY_CUSTORCOUNT SET '                                                                                              |    V_NUMRECORDS = SQL%ROWCOUNT;   |
|               ||STR_CUST_COUNT||'_COUNT='||char(CUST_COUNT)||' ,'                                                             |                                   |
|               ||'SELF_UPDATE_TIME ='||''''||STR_DATE||''''||' ,'                                                              |                                   |
|               ||'SUB_UPDATE_TIME ='||''''||STR_DATE1||''''||','                                                               |                                   |
|               ||'REPORT_STATE ='||''''|| STR_CHAR ||''''                                                                      |                                   |
|               ||' WHERE ORG_CODE ='||''''||TEMP_ORG_CODE||''''                                                                |                                   |
|              ||' AND Y='||char(TEMP_YEAR)||' AND HY='||char(TEMP_HY)                                                          |                                   |
|               ||' AND REGION_CODE='||''''||TEMP_SALE_REG_CODE||''''||' AND BRAND_CODE='||''''||TEMP_BRAND_CODE||''''          |                                   |
|               ||' AND exists (select 1 from CC_ORDER_HY_CUSTORCOUNT'                                                          |                                   |
|               ||' where ORG_CODE ='||''''||TEMP_ORG_CODE||''''                                                                |                                   |
|              ||' AND Y='||char(TEMP_YEAR)||' AND HY='||char(TEMP_HY)                                                          |                                   |
|               ||' AND REGION_CODE='||''''||TEMP_SALE_REG_CODE||''''||' AND BRAND_CODE='||''''||TEMP_BRAND_CODE||''''||')';--  |                                   |
|     PREPARE S1 FROM STR_UPDATE1;                                                                                              |                                   |
|     EXECUTE S1;  --                                                                                                           |                                   |
|     GET DIAGNOSTICS V_NUMRECORDS = ROW_COUNT;                                                                                 |                                   |
+-------------------------------------------------------------------------------------------------------------------------------+-----------------------------------+
