:original_name: dws_07_6824.html

.. _dws_07_6824:

System Functions
================

ISNULL()
--------

+--------------------------------------------------------+------------------------------------------------------+
| Netezza Syntax                                         | Syntax After Migration                               |
+========================================================+======================================================+
| ::                                                     | ::                                                   |
|                                                        |                                                      |
|    SELECT A.ETL_DATE, A.BRANCH_CODE, A.CUST_NO         |    SELECT A.ETL_DATE, A.BRANCH_CODE, A.CUST_NO       |
|               , ISNULL (  B.RES_STOCK,0) AS RES_STOCK  |                , NVL (  B.RES_STOCK,0) AS RES_STOCK  |
|               , ISNULL ( B.ZY_VOL ,0 ) AS ZY_VOL       |                , NVL ( B.ZY_VOL ,0 ) AS ZY_VOL       |
|               , ISNULL ( B.ZJ_VOL,0 ) AS ZJ_VOL        |                , NVL ( B.ZJ_VOL,0 ) AS ZJ_VOL        |
|        FROM tab123;                                    |        FROM tab123;                                  |
+--------------------------------------------------------+------------------------------------------------------+

NVL
---

Second parameter is missing.

+--------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------+
| Netezza Syntax                                                                             | Syntax After Migration                                                                       |
+============================================================================================+==============================================================================================+
| ::                                                                                         | ::                                                                                           |
|                                                                                            |                                                                                              |
|    SELECT NVL( SUM(A3.DA_CPTL_BAL_YEAR) / NULLIF(V_YEAR_DAYS, 0) ) AS CPTL_BAL_AVE_YR      |    ELECT NVL( SUM(A3.DA_CPTL_BAL_YEAR) / NULLIF(V_YEAR_DAYS, 0), NULL ) AS CPTL_BAL_AVE_YR   |
|          , NVL( NVL(SUM (CASE WHEN A3.OPENACT_DT >= V_YEAR_START THEN A3.DA_CPTL_BAL_YEAR  |          , NVL( NVL(SUM (CASE WHEN A3.OPENACT_DT >= V_YEAR_START THEN A3.DA_CPTL_BAL_YEAR    |
|           END) / NULLIF(V_YEAR_DAYS, 0) ), 0) AS CPTL_BAL_AVE_YR_OP                        |           END) / NULLIF(V_YEAR_DAYS, 0), NULL), 0) AS CPTL_BAL_AVE_YR_OP                     |
|          , NVL( SUM(A3.DA_CPTL_BAL) / NULLIF(V_YEAR_DAYS, 0) ) AS CPTL_BAL_AVE             |          , NVL( SUM(A3.DA_CPTL_BAL) / NULLIF(V_YEAR_DAYS, 0), NULL ) AS CPTL_BAL_AVE         |
|       FROM tab1 A3;                                                                        |       FROM tab1 A3;                                                                          |
+--------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------+

DATE
----

Casting the data type.

+-------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Netezza Syntax                                                                                                                                              | Syntax After Migration                                                                                                                                              |
+=============================================================================================================================================================+=====================================================================================================================================================================+
| ::                                                                                                                                                          | ::                                                                                                                                                                  |
|                                                                                                                                                             |                                                                                                                                                                     |
|    SELECT A1.ETL_DATE, A1.MARKET_CODE                                                                                                                       |    SELECT A1.ETL_DATE, A1.MARKET_CODE                                                                                                                               |
|          , A1.DECLARATION_DT                                                                                                                                |          , A1.DECLARATION_DT                                                                                                                                        |
|          , ROW_NUMBER() OVER(PARTITION BY A1.MARKET_CODE, A1.STOCK_CODE, DATE_PART('YEAR', DATE(A1.DECLARATION_DT)) ORDER BY A1.DECLARATION_DT DESC) AS RN  |          , ROW_NUMBER() OVER(PARTITION BY A1.MARKET_CODE, A1.STOCK_CODE, DATE_PART('YEAR', CAST(A1.DECLARATION_DT AS DATE)) ORDER BY A1.DECLARATION_DT DESC) AS RN  |
|       FROM tb_date_type_casting A1;                                                                                                                         |       FROM tb_date_type_casting A1;                                                                                                                                 |
|                                                                                                                                                             |                                                                                                                                                                     |
|                                                                                                                                                             |                                                                                                                                                                     |
|     SELECT A1.ETL_DATE, A1.MARKET_CODE                                                                                                                      |                                                                                                                                                                     |
|          , A1.DECLARATION_DT                                                                                                                                |                                                                                                                                                                     |
|          , ROW_NUMBER() OVER(PARTITION BY A1.MARKET_CODE, A1.STOCK_CODE, DATE_PART('YEAR', DATE(A1.DECLARATION_DT)) ORDER BY A1.DECLARATION_DT DESC) AS RN  |                                                                                                                                                                     |
|       FROM tb_date_type_casting A1;                                                                                                                         |                                                                                                                                                                     |
+-------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------+

analytic_function
-----------------

+-----------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------+
| Netezza Syntax                                                                                            | Syntax After Migration                                                                                 |
+===========================================================================================================+========================================================================================================+
| ::                                                                                                        | ::                                                                                                     |
|                                                                                                           |                                                                                                        |
|    SELECT COALESCE(NULLIF(GROUP_CONCAT(a.column_name),''),'*')                                            |    SELECT COALESCE(NULLIF(STRING_AGG(a.column_name, ','),''),'*')                                      |
|       FROM (SELECT a.column_name                                                                          |       FROM (SELECT a.column_name                                                                       |
|               FROM tb_ntz_group_concat a                                                                  |               FROM tb_ntz_group_concat a                                                               |
|              WHERE UPPER(a.table_name) = 'EMP'                                                            |              WHERE UPPER(a.table_name) = 'EMP'                                                         |
|              ORDER BY a.column_pos) a;                                                                    |              ORDER BY a.column_pos) a;                                                                 |
|     -------------                                                                                         |     -------------                                                                                      |
|     SELECT admin.group_concat('"top'||lpad(a.table_name,2,'0')||'":{'||a.column_name||'}') topofund_data  |     SELECT STRING_AGG('"top'||lpad(a.table_name,3,'0')||'":{'||a.column_name||'}', ',') topofund_data  |
|       FROM (SELECT a.table_name, a.column_name                                                            |       FROM (SELECT a.table_name, a.column_name                                                         |
|               FROM tb_ntz_group_concat a                                                                  |               FROM tb_ntz_group_concat a                                                               |
|              WHERE UPPER(a.table_name) = 'EMP'                                                            |              WHERE UPPER(a.table_name) = 'EMP'                                                         |
|              ORDER BY a.column_pos) a;                                                                    |              ORDER BY a.column_pos) a;                                                                 |
+-----------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------+

Stored Procedure
----------------

+-----------------------------------------------------------------+----------------------------------------------------------------------+
| Netezza Syntax                                                  | Syntax After Migration                                               |
+=================================================================+======================================================================+
| ::                                                              | ::                                                                   |
|                                                                 |                                                                      |
|    CREATE OR REPLACE PROCEDURE sp_ntz_proc_call                 |    CREATE OR REPLACE FUNCTION sp_ntz_proc_call                       |
|          ( CHARACTER VARYING(8) )                               |          ( CHARACTER VARYING(8) )                                    |
|     RETURNS INTEGER                                             |     RETURN INTEGER                                                   |
|     LANGUAGE NZPLSQL                                            |     AS                                                               |
|     AS                                                          |         V_PAR_DAY ALIAS for $1;                                      |
|     BEGIN_PROC                                                  |         V_PRCNAME    NCHAR VARYING(50):= 'SP_O_HXYW_LNSACCTINFO_H';  |
|     DECLARE                                                     |         D_TIME_START TIMESTAMP:= CURRENT_TIMESTAMP;                  |
|         V_PAR_DAY ALIAS for $1;                                 |         O_RETURN INTEGER;                                            |
|         V_PRCNAME    NVARCHAR(50):= 'SP_O_HXYW_LNSACCTINFO_H';  |     BEGIN                                                            |
|         D_TIME_START TIMESTAMP:= CURRENT_TIMESTAMP;             |       O_RETURN := 0;                                                 |
|         O_RETURN INTEGER;                                       |       SP_LOG_EXEC(V_PRCNAME,V_PAR_DAY,D_TIME_START,0,0);             |
|     BEGIN                                                       |                                                                      |
|       O_RETURN := 0;                                            |       RETURN O_RETURN;                                               |
|       CALL SP_LOG_EXEC(V_PRCNAME,V_PAR_DAY,D_TIME_START,0,0);   |                                                                      |
|                                                                 |     END;                                                             |
|       RETURN O_RETURN;                                          |     /                                                                |
|                                                                 |                                                                      |
|     END;                                                        |                                                                      |
|     END_PROC;                                                   |                                                                      |
+-----------------------------------------------------------------+----------------------------------------------------------------------+
