:original_name: dws_16_0102.html

.. _dws_16_0102:

System Views
============

DSC migrates the system views **dbc.columnsV** and **dbc.IndicesV**, and the resulting output is displayed below.

**Input:**

::

   SELECT A.ColumnName
   AS V_COLS
         ,A.columnname  || ' ' ||CASE WHEN columnType in ('CF','CV')
                                           THEN CASE WHEN columnType='CV' THEN 'VAR' ELSE ''
                 END||'CHAR('||TRIM(columnlength (INT))||
                     ') CHARACTER SET LATIN'||
                       CASE WHEN UpperCaseFlag='N'
                          THEN ' NOT' ELSE ''
                       END || ' CASESPECIFIC'
                                      WHEN columnType='DA' THEN 'DATE'
                                      WHEN columnType='TS' THEN 'TIMESTAMP(' || TRIM(DecimalFractionalDigits)||')'
                                      WHEN columnType='AT' THEN 'TIME('|| TRIM(DecimalFractionalDigits)||')'
                                      WHEN columnType='I' THEN 'INTEGER'
                                      WHEN columnType='I1' THEN 'BYTEINT'
                                      WHEN columnType='I2' THEN 'SMALLINT'
                                      WHEN columnType='I8' THEN 'BIGINT'
                                      WHEN columnType='D' THEN 'DECIMAL('||TRIM(DecimalTotalDigits)||','||TRIM(DecimalFractionalDigits)||')'
                                      ELSE 'Unknown'
                                  END||CASE WHEN Nullable='Y'
            THEN '' ELSE ' NOT NULL' END||'0A'XC
   AS V_ColT           -      ,B.ColumnName
   AS V_PICol
    FROM dbc.columnsV A LEFT JOIN dbc.IndicesV B
      ON A.columnName = B.columnName AND B.IndexType IN ('Q','P')
     AND B.DatabaseName = '${V_TDDLDB}'  AND B.tablename='${TARGET_TABLE}'
   WHERE A.databasename='${V_TDDLDB}' AND A.tablename = '${TARGET_TABLE}'
     AND A.columnname NOT IN ( 'ETL_JOB_NAME'                                                                                                     ,'ETL_TX_DATE'
                               ,'ETL_PROC_DATE'
                               )
    ORDER BY A.columnid;

**Output:**

::

   DECLARE lv_mig_V_COLS   TEXT;
             lv_mig_V_ColT        TEXT;
             lv_mig_V_PICol       TEXT;
   BEGIN
   SELECT STRING_AGG(A.ColumnName, ',')
         , STRING_AGG(A.columnname  || ' ' ||CASE WHEN columnType in ('CF','CV')
                                           THEN CASE WHEN columnType='CV' THEN 'VAR' ELSE ''
                 END||'CHAR('||TRIM(mig_td_ext.mig_fn_castasint(columnlength))||
                     ') /*CHARACTER SET LATIN*/'||
                       CASE WHEN UpperCaseFlag='N'
                          THEN ' NOT' ELSE ''
                       END || ' /*CASESPECIFIC*/'
                                      WHEN columnType='DA' THEN 'DATE'
                                      WHEN columnType='TS' THEN 'TIMESTAMP(' || TRIM(DecimalFractionalDigits)||')'
                                      WHEN columnType='AT' THEN 'TIME('|| TRIM(DecimalFractionalDigits)||')'
                                      WHEN columnType='I' THEN 'INTEGER'
                                      WHEN columnType='I1' THEN 'BYTEINT'
                                      WHEN columnType='I2' THEN 'SMALLINT'
                                      WHEN columnType='I8' THEN 'BIGINT'
                                      WHEN columnType='D' THEN 'DECIMAL('||TRIM(DecimalTotalDigits)||','||TRIM(DecimalFractionalDigits)||')'
                                      ELSE 'Unknown'
                                  END||CASE WHEN Nullable='Y'
            THEN '' ELSE ' NOT NULL' END||E'\x0A', ',')                  , STRING_AGG(B.ColumnName, ',')
   INTO lv_mig_V_COLS, lv_mig_V_ColT, lv_mig_V_PICol
   FROM mig_td_ext.vw_td_dbc_columnsV A LEFT JOIN mig_td_ext.vw_td_dbc_IndicesV B
      ON A.columnName = B.columnName AND B.IndexType IN ('Q','P')
     AND B.DatabaseName = 'public'  AND B.tablename='emp2'
   WHERE A.databasename='public' AND A.tablename = 'emp2';
   -- ORDER BY A.columnid;
   END;
   /
