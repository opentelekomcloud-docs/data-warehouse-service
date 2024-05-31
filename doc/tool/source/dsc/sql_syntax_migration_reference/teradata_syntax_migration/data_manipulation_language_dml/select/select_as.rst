:original_name: dws_16_0091.html

.. _dws_16_0091:

SELECT AS
=========

GaussDB(DWS) variable names are case insensitive, and TD variable names are case sensitive. To ensure that the TD script is correct before and after the migration, retain the case of the original variable name in the variable definition of the SELECT statement. The converted variable is defined in the **AS** *variable_name*.

**Input**

.. code-block::

   SELECT  TRIM('${JOB_NAME}')                                                                    AS JOB_NAME
          ,CASE WHEN LENGTH(trim(STRTOK('${JOB_NAME}','-',4)))=2
               THEN trim(STRTOK('${JOB_NAME}','-',4))
                ELSE ''
                END                                                                                     AS EDW_BANK_NM
          ,TRIM('${TX_DATE}')                                                                     AS TX_DATE
          ,USER                                                                                   AS ETL_USER
          ,CAST( CURRENT_TIMESTAMP(0) AS VARCHAR(19))                                             AS CURR_STIME
          ,'${ETL_DATA}'                                                                          AS ETL_DATA
          ,'T61_INDV_CUST_ACCT_ORG_AUM'                                                           AS TARGET_TABLE
          ,'CAST(''8999-12-31'' AS DATE)'                                                         AS MAXDATE
   ;
   .IF ERRORCODE <> 0 THEN .QUIT 12

**Output**

.. code-block::

   SELECT
             TRIM( '${job_name}' ) AS "JOB_NAME"
             ,CASE
                       WHEN LENGTH( TRIM( split_part ( '${job_name}' ,'-' ,4 ) ) ) = 2 THEN TRIM( split_part ( '${job_name}' ,'-' ,4 ) )
                  ELSE ''
             END AS "EDW_BANK_NM"
             ,TRIM( '${tx_date}' ) AS "TX_DATE"
             ,USER AS "ETL_USER"
             ,CAST( CURRENT_TIMESTAMP( 0 ) AS VARCHAR( 19 ) ) AS "CURR_STIME"
             ,'${etl_data}' AS "ETL_DATA"
             ,'T61_INDV_CUST_ACCT_ORG_AUM' AS "TARGET_TABLE"
             ,'CAST(''8999-12-31'' AS DATE)' AS "MAXDATE" ;
   \if ${ERROR} != 'false'
    \q 12
   \endif

   ;

Definition nested with AS expression is implemented by splitting multiple statements.

**Input**

.. code-block::

   SELECT  TRIM('${JOB_NAME}')                                                                    AS JOB_NAME
          ,'CAST(''0001-01-02'' AS DATE)'                                                         AS ILLDATE
          ,'T61_INDV_CUST_HOLD_PROD_IND_AUM'                                                      AS TARGET_TABLE
          ,0                                                                                      AS NULLNUMBER
          ,'CAST(''00:00:00.999'' AS TIME(3))'                                                    AS NULLTIME
          ,'CAST(''0001-01-01 00:00:00.000000'' AS TIMESTAMP(6))'                                 AS NULLTIMESTAMP
          ,'VT_'||TARGET_TABLE                                                                    AS VT_TABLE
          ,'V'||SUBSTR(TARGET_TABLE,2,CHAR(TARGET_TABLE)-1)                                       AS TARGET_TABLE_V
          ,'${GDM_DETAIL_DDL}'                                                                    AS V_TDDLDB
          ,'${GDM_DETAIL_VIEW}'                                                                   AS V_TARGETDB
          ,'${UDF}'                                                                               AS V_PUB_UDF

   ;
   .IF ERRORCODE <> 0 THEN .QUIT 12

**Output**

.. code-block::

   SELECT
             TRIM( '${job_name}' ) AS "JOB_NAME"
             ,'CAST(''0001-01-02'' AS DATE)' AS "ILLDATE"
             ,'T61_INDV_CUST_HOLD_PROD_IND_AUM' AS "TARGET_TABLE"
             ,0 AS "NULLNUMBER"
             ,'CAST(''00:00:00.999'' AS TIME(3))' AS "NULLTIME"
             ,'CAST(''0001-01-01 00:00:00.000000'' AS TIMESTAMP(6))' AS "NULLTIMESTAMP"
             ,'${gdm_detail_ddl}' AS "V_TDDLDB"
             ,'${gdm_detail_view}' AS "V_TARGETDB"
             ,'${udf}' AS "V_PUB_UDF" ;
   SELECT
             'VT_' || '${TARGET_TABLE}' AS "VT_TABLE" ;
   SELECT
             'V' || SUBSTR( '${TARGET_TABLE}' ,2 ,LENGTH( '${TARGET_TABLE}' ) - 1 ) AS "TARGET_TABLE_V" ;
   \if ${ERROR} != 'false'
    \q 12
   \endif

   ;
