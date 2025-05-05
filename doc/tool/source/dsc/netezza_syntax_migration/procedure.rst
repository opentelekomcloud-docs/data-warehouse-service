:original_name: dws_07_6823.html

.. _dws_07_6823:

Procedure
=========

Variable Data Type
------------------

NVARCHAR changed to NCHAR VARING.

+-----------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------+
| Netezza Syntax                                                                          | Syntax After Migration                                                            |
+=========================================================================================+===================================================================================+
| ::                                                                                      | ::                                                                                |
|                                                                                         |                                                                                   |
|    CREATE OR REPLACE PROCEDURE "NTZDB"."EDW"."SP_NTZ_NVARCHAR"                          |    CREATE OR REPLACE FUNCTION "EDW"."SP_NTZ_NVARCHAR"                             |
|               (CHARACTER VARYING(8))                                                    |                (CHARACTER VARYING(8))                                             |
|     RETURNS INTEGER                                                                     |     RETURN INTEGER                                                                |
|     LANGUAGE NZPLSQL AS                                                                 |     AS                                                                            |
|     BEGIN_PROC                                                                          |         V_PAR_DAY ALIAS for $1;                                                   |
|     DECLARE                                                                             |         V_PRCNAME    NCHAR VARYING(50):= 'SP_O_HXYW_LNSACCTINFO_H';               |
|         V_PAR_DAY ALIAS for $1;                                                         |         V_CNT        INTEGER;                                                     |
|         V_PRCNAME    NVARCHAR(50):= 'SP_O_HXYW_LNSACCTINFO_H';                          |         V_STEP_INFO   NCHAR VARYING(500);                                         |
|         V_CNT        INTEGER;                                                           |         D_TIME_START TIMESTAMP:= CURRENT_TIMESTAMP;                               |
|         V_STEP_INFO   NVARCHAR(500);                                                    |         O_RETURN INTEGER;                                                         |
|         D_TIME_START TIMESTAMP:= CURRENT_TIMESTAMP;                                     |     BEGIN                                                                         |
|         O_RETURN INTEGER;                                                               |       O_RETURN := 0;                                                              |
|     BEGIN                                                                               |                                                                                   |
|       O_RETURN := 0;                                                                    |     /* Writes logs and starts the recording process. */                           |
|                                                                                         |      SP_LOG_EXEC(V_PRCNAME,V_PAR_DAY,D_TIME_START,0,0,'The process running',' '); |
|    --Writes logs and starts the recording process.                                      |                                                                                   |
|      CALL SP_LOG_EXEC(V_PRCNAME,V_PAR_DAY,D_TIME_START,0,0,'The process running ',' '); |      V_STEP_INFO := '1.Initialization';                                           |
|                                                                                         |                                                                                   |
|      V_STEP_INFO := '1.Initialization';                                                 |      RETURN O_RETURN;                                                             |
|                                                                                         |                                                                                   |
|      RETURN O_RETURN;                                                                   |     END;                                                                          |
|                                                                                         |     /                                                                             |
|     END;                                                                                |                                                                                   |
|     END_PROC;                                                                           |                                                                                   |
+-----------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------+

row counts
----------

The **row_count** function is supported for affected row counting.

+--------------------------------------------------------------------------------+--------------------------------------------------------------------------------+
| Netezza Syntax                                                                 | Syntax After Migration                                                         |
+================================================================================+================================================================================+
| ::                                                                             | ::                                                                             |
|                                                                                |                                                                                |
|    CREATE OR REPLACE PROCEDURE "NTZDB"."EDW"."SP_NTZ_ROWCOUNT"                 |    CREATE OR REPLACE FUNCTION "EDW"."SP_NTZ_ROWCOUNT"                          |
|            (CHARACTER VARYING(8))                                              |                (CHARACTER VARYING(8))                                          |
|     RETURNS INTEGER                                                            |     RETURN INTEGER                                                             |
|     LANGUAGE NZPLSQL AS                                                        |     AS                                                                         |
|     BEGIN_PROC                                                                 |         V_PAR_DAY ALIAS for $1;                                                |
|     DECLARE                                                                    |         O_RETURN INTEGER;                                                      |
|         V_PAR_DAY ALIAS for $1;                                                |     BEGIN                                                                      |
|         O_RETURN INTEGER;                                                      |        O_RETURN := 0;                                                          |
|     BEGIN                                                                      |         EXECUTE IMMEDIATE 'INSERT  INTO TMPO_HXYW_LNSACCTINFO_H1               |
|       O_RETURN := 0;                                                           |                ( ACCTNO, ACCTYPE, SUBCTRLCODE, CCY, NAME )                     |
|                                                                                |                   SELECT ACCTNO, ACCTYPE, SUBCTRLCODE, CCY, NAME               |
|      EXECUTE IMMEDIATE 'INSERT  INTO TMPO_HXYW_LNSACCTINFO_H1                  |                    FROM O_HXYW_LNSACCTINFO T                                   |
|                ( ACCTNO, ACCTYPE, SUBCTRLCODE, CCY, NAME )                     |                  WHERE NOT EXISTS (SELECT 1 FROM O_HXYW_LNSACCT T1             |
|                   SELECT ACCTNO, ACCTYPE, SUBCTRLCODE, CCY, NAME               |                                  WHERE T1.DATA_START_DT<='''||V_PAR_DAY||'''   |
|                    FROM O_HXYW_LNSACCTINFO T                                   |                                    AND T.MD5_VAL=T1.MD5_VAL)';                 |
|                  WHERE NOT EXISTS (SELECT 1 FROM O_HXYW_LNSACCT T1             |                                                                                |
|                                  WHERE T1.DATA_START_DT<='''||V_PAR_DAY||'''   |        O_RETURN := SQL%ROWCOUNT;                                               |
|                                    AND T.MD5_VAL=T1.MD5_VAL)';                 |                                                                                |
|                                                                                |        RETURN O_RETURN;                                                        |
|      O_RETURN := ROW_COUNT;                                                    |                                                                                |
|                                                                                |     END;                                                                       |
|      RETURN O_RETURN;                                                          |     /                                                                          |
|                                                                                |                                                                                |
|     END;                                                                       |                                                                                |
|     END_PROC;                                                                  |                                                                                |
+--------------------------------------------------------------------------------+--------------------------------------------------------------------------------+

.. note::

   **ROW_COUNT** identifies the number of rows associated with the previous SQL statement. If the previous SQL statement is a DELETE, INSERT, or UPDATE statement, ROW_COUNT identifies the number of rows that qualified for the operation.

System Tables
-------------

System **tables \_V_SYS_COLUMNS** is replaced with **information_schema.columns**.

+----------------------------------------------------------------------+----------------------------------------------------------------------+
| Netezza Syntax                                                       | Syntax After Migration                                               |
+======================================================================+======================================================================+
| ::                                                                   | ::                                                                   |
|                                                                      |                                                                      |
|     BEGIN                                                            |    BEGIN                                                             |
|         SELECT COUNT(*) INTO V_CNT FROM _V_SYS_COLUMNS               |         SELECT COUNT(*) INTO V_CNT FROM information_schema.columns   |
|            WHERE table_schem = 'SCOTT'                               |           WHERE table_schema = lower('SCOTT')                        |
|               AND TABLE_NAME='TMPO_HXYW_LNSACCTINFO_H1';             |                AND table_name = lower('TMPO_HXYW_LNSACCTINFO_H1');   |
|         if V_CNT>0 then                                              |         if V_CNT>0 then                                              |
|            EXECUTE IMMEDIATE 'DROP TABLE TMPO_HXYW_LNSACCTINFO_H1';  |            EXECUTE IMMEDIATE 'DROP TABLE TMPO_HXYW_LNSACCTINFO_H1';  |
|         end if;                                                      |         end if;                                                      |
|       END;                                                           |       END;                                                           |
+----------------------------------------------------------------------+----------------------------------------------------------------------+

.. note::

   Column mapping:

   -  table_schem => table_schema
   -  table_name => table_name
   -  column_name => column_name
   -  ordinal_position => ordinal_position
   -  type_name => data_type
   -  is_nullable => is_nullable

For date subtraction, the corresponding Integer should be returned
------------------------------------------------------------------

Return value should be integer for date subtraction.

+-------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------+
| Netezza Syntax                                                                      | Syntax After Migration                                                                                         |
+=====================================================================================+================================================================================================================+
| ::                                                                                  | ::                                                                                                             |
|                                                                                     |                                                                                                                |
|    SELECT CAST( T1.Buyback_Mature_Dt - CAST( '${gsTXDate}' AS DATE) AS CHAR( 5 ) )  |    SELECT CAST( EXTRACT( 'DAY' FROM ( T1.Buyback_Mature_Dt - CAST( '${gsTXDate}' AS DATE ) ) ) AS CHAR( 5 ) )  |
|       FROM tab1 T1                                                                  |        FROM tab1 T1                                                                                            |
|     WHERE T1.col1 > 10;                                                             |      WHERE T1.col1 > 10;                                                                                       |
|     -----                                                                           |     -------                                                                                                    |
|     SELECT CURRENT_DATE - DATE '2019-03-30';                                        |     SELECT EXTRACT( 'DAY' FROM (CURRENT_DATE - CAST( '2019-03-30' AS DATE ) ) );                               |
+-------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------+

Support of TRANSLATE Function
-----------------------------

The SQL TRANSLATE() function replaces a sequence of characters in a string with another sequence of characters. The function replaces a single character at a time.

+-----------------------------------------------------------+------------------------------------------------------------------------------------+
| Netezza Syntax                                            | Syntax After Migration                                                             |
+===========================================================+====================================================================================+
| ::                                                        | ::                                                                                 |
|                                                           |                                                                                    |
|    TRANSLATE(param1)                                      |    UPPER(param1)                                                                   |
|     TRANSLATE(1st param, 2nd param, 3rd param)            |     TRANSLATE(1st param, 3rd param, RPAD(2nd param, LENGTH(3rd param), ' '))       |
|     TRANSLATE(1st param, 2nd param, 3rd param, 4th param) |     TRANSLATE(1st param, 3rd param, RPAD(2nd param, LENGTH(3rd param), 4th param)) |
+-----------------------------------------------------------+------------------------------------------------------------------------------------+

.. note::

   If it contains a single parameter, just execute the UPPER.

   UPPER(param1)

   If it contains two parameters, throw error.

   If it contains three parameters, TRANSLATE(1st param, 3rd param, RPAD(2nd param, LENGTH(3rd param), ' ')).

   If it contains four parameters, TRANSLATE(1st param, 3rd param, RPAD(2nd param, LENGTH(3rd param), 4th param)).

Data Type
---------

NATIONAL CHARACTER VARYING ( ANY )

+--------------------------------------------------------+-------------------------------------------------------+
| Netezza Syntax                                         | Syntax After Migration                                |
+========================================================+=======================================================+
| ::                                                     | ::                                                    |
|                                                        |                                                       |
|    CREATE OR REPLACE PROCEDURE sp_ntz_nchar_with_any   |    CREATE OR REPLACE FUNCTION sp_ntz_nchar_with_any   |
|           ( NATIONAL CHARACTER VARYING(10)             |               ( NATIONAL CHARACTER VARYING(10)        |
|           , NATIONAL CHARACTER VARYING(ANY) )          |               , NATIONAL CHARACTER VARYING  )         |
|     RETURN NATIONAL CHARACTER VARYING(ANY)             |     RETURN NATIONAL CHARACTER VARYING                 |
|     LANGUAGE NZPLSQL AS                                |     AS                                                |
|     BEGIN_PROC                                         |          I_LOAD_DT ALIAS FOR $1 ;                     |
|     DECLARE                                            |          /*  ETL Date */                              |
|          I_LOAD_DT ALIAS FOR $1 ;                      |          V_TASK_ID ALIAS FOR $2 ;                     |
|          -- ETL Date                                   |     BEGIN                                             |
|          V_TASK_ID ALIAS FOR $2 ;                      |        RETURN I_LOAD_DT || ',' || V_TASK_ID;          |
|     BEGIN                                              |     END;                                              |
|        RETURN I_LOAD_DT || ',' || V_TASK_ID;           |     /                                                 |
|     END;                                               |                                                       |
|     END_PROC;                                          |                                                       |
+--------------------------------------------------------+-------------------------------------------------------+

CHARACTER VARYING ( ANY )

+-------------------------------------------------------+------------------------------------------------------+
| Netezza Syntax                                        | Syntax After Migration                               |
+=======================================================+======================================================+
| ::                                                    | ::                                                   |
|                                                       |                                                      |
|    CREATE OR REPLACE PROCEDURE sp_ntz_char_with_any   |    CREATE OR REPLACE FUNCTION sp_ntz_char_with_any   |
|               ( NATIONAL CHARACTER VARYING(10)        |              ( NATIONAL CHARACTER VARYING(10)        |
|               , CHARACTER VARYING(ANY) )              |              , CHARACTER VARYING  )                  |
|     RETURN CHARACTER VARYING(ANY)                     |     RETURN CHARACTER VARYING                         |
|     LANGUAGE NZPLSQL AS                               |     AS                                               |
|     BEGIN_PROC                                        |          I_LOAD_DT ALIAS FOR $1 ;                    |
|     DECLARE                                           |          /*  ETL Date */                             |
|          I_LOAD_DT ALIAS FOR $1 ;                     |          V_TASK_ID ALIAS FOR $2 ;                    |
|          -- ETL Date                                  |     BEGIN                                            |
|          V_TASK_ID ALIAS FOR $2 ;                     |        RETURN I_LOAD_DT || ',' || V_TASK_ID;         |
|     BEGIN                                             |     END;                                             |
|        RETURN I_LOAD_DT || ',' || V_TASK_ID;          |     /                                                |
|     END;                                              |                                                      |
|     END_PROC;                                         |                                                      |
+-------------------------------------------------------+------------------------------------------------------+

Numeric (ANY)

+-------------------------------------------------------------------------+-------------------------------------------------------------------------+
| Netezza Syntax                                                          | Syntax After Migration                                                  |
+=========================================================================+=========================================================================+
| ::                                                                      | ::                                                                      |
|                                                                         |                                                                         |
|    CREATE or replace PROCEDURE sp_ntz_numeric_with_any                  |    CREATE or replace FUNCTION  sp_ntz_numeric_with_any                  |
|               ( NUMERIC(ANY)                                            |                ( NUMERIC                                                |
|               , NUMERIC(ANY) )                                          |                , NUMERIC )                                              |
|     RETURNS NATIONAL CHARACTER VARYING(ANY)                             |     RETURN NATIONAL CHARACTER VARYING                                   |
|     LANGUAGE NZPLSQL                                                    |     AS                                                                  |
|     AS BEGIN_PROC                                                       |       ERROR_INFO NCHAR VARYING(2000) := '';                             |
|     DECLARE                                                             |       V_VC_YCBZ NCHAR VARYING(1) := 'N';                                |
|       ERROR_INFO NVARCHAR(2000) := '';                                  |       V_VC_SUCCESS NCHAR VARYING(10) := 'SUCCESS';                      |
|       V_VC_YCBZ NVARCHAR(1) := 'N';                                     |                                                                         |
|       V_VC_SUCCESS NVARCHAR(10) := 'SUCCESS';                           |       p_l_begindate ALIAS FOR $1;                                       |
|                                                                         |       p_l_enddate ALIAS FOR $2;                                         |
|       p_l_begindate ALIAS FOR $1;                                       |     BEGIN                                                               |
|       p_l_enddate ALIAS FOR $2;                                         |       ERROR_INFO := CRHSP_CRH_ETL_EXCHDATE(p_l_begindate,p_l_enddate);  |
|     BEGIN                                                               |       if ERROR_INFO != V_VC_SUCCESS  then                               |
|       ERROR_INFO := CRHSP_CRH_ETL_EXCHDATE(p_l_begindate,p_l_enddate);  |         V_VC_YCBZ := 'C';                                               |
|       if ERROR_INFO != V_VC_SUCCESS  then                               |       end if;                                                           |
|         V_VC_YCBZ := 'C';                                               |                                                                         |
|       end if;                                                           |       RETURN V_VC_SUCCESS;                                              |
|                                                                         |                                                                         |
|       RETURN V_VC_SUCCESS;                                              |     END;                                                                |
|                                                                         |     /                                                                   |
|     END;                                                                |                                                                         |
|     END_PROC;                                                           |                                                                         |
+-------------------------------------------------------------------------+-------------------------------------------------------------------------+

Exception
---------

TRANSACTION_ABORTED

+-------------------------------------------------------------------------+-------------------------------------------------------------------------+
| Netezza Syntax                                                          | Syntax After Migration                                                  |
+=========================================================================+=========================================================================+
| ::                                                                      | ::                                                                      |
|                                                                         |                                                                         |
|    CREATE OR REPLACE PROCEDURE sp_ntz_transaction_aborted               |    CREATE OR REPLACE FUNCTION sp_ntz_transaction_aborted                |
|         ( NUMERIC(ANY)                                                  |               ( NUMERIC                                                 |
|         , NUMERIC(ANY) )                                                |               , NUMERIC )                                               |
|         RETURNS NATIONAL CHARACTER VARYING(ANY)                         |         RETURN NATIONAL CHARACTER VARYING                               |
|         LANGUAGE NZPLSQL                                                |     AS                                                                  |
|     AS BEGIN_PROC                                                       |       ERROR_INFO NCHAR VARYING(2000) := '';                             |
|     DECLARE                                                             |                                                                         |
|       ERROR_INFO NVARCHAR(2000) := '';                                  |                                                                         |
|                                                                         |       p_l_begindate ALIAS FOR $1;                                       |
|                                                                         |       p_l_enddate ALIAS FOR $2;                                         |
|       p_l_begindate ALIAS FOR $1;                                       |     BEGIN                                                               |
|       p_l_enddate ALIAS FOR $2;                                         |       ERROR_INFO := CRHSP_CRH_ETL_EXCHDATE(p_l_begindate,p_l_enddate);  |
|     BEGIN                                                               |       RETURN ERROR_INFO;                                                |
|       ERROR_INFO := CRHSP_CRH_ETL_EXCHDATE(p_l_begindate,p_l_enddate);  |                                                                         |
|       RETURN ERROR_INFO;                                                |                                                                         |
|                                                                         |     EXCEPTION                                                           |
|                                                                         |       WHEN INVALID_TRANSACTION_TERMINATION THEN                         |
|     EXCEPTION                                                           |       ROLLBACK;                                                         |
|       WHEN TRANSACTION_ABORTED THEN                                     |       BEGIN                                                             |
|       ROLLBACK;                                                         |         ERROR_INFO := SQLERRM||'  sp_o_transaction_aborted:';           |
|       BEGIN                                                             |         RETURN ERROR_INFO;                                              |
|         ERROR_INFO := SQLERRM||'  sp_o_transaction_aborted:';           |       END;                                                              |
|         RETURN ERROR_INFO;                                              |                                                                         |
|       END;                                                              |                                                                         |
|                                                                         |       WHEN OTHERS THEN                                                  |
|                                                                         |       BEGIN                                                             |
|       WHEN OTHERS THEN                                                  |         ERROR_INFO := SQLERRM||'  sp_o_transaction_aborted:';           |
|       BEGIN                                                             |         RETURN ERROR_INFO;                                              |
|         ERROR_INFO := SQLERRM||'  sp_o_transaction_aborted:';           |       END;                                                              |
|         RETURN ERROR_INFO;                                              |     END;                                                                |
|       END;                                                              |     /                                                                   |
|     END;                                                                |                                                                         |
|     END_PROC;                                                           |                                                                         |
+-------------------------------------------------------------------------+-------------------------------------------------------------------------+

END statement is specified without semicolon (;)
------------------------------------------------

END statement specified without semicolon (;) is migrated as follows:

END /

+--------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------+
| Netezza Syntax                                                                                   | Syntax After Migration                                                                           |
+==================================================================================================+==================================================================================================+
| ::                                                                                               | ::                                                                                               |
|                                                                                                  |                                                                                                  |
|    CREATE OR REPLACE PROCEDURE sp_ntz_end_wo_semicolon                                           |    CREATE OR REPLACE FUNCTION sp_ntz_end_wo_semicolon                                            |
|          ( NATIONAL CHARACTER VARYING(10) )                                                      |          ( NATIONAL CHARACTER VARYING(10) )                                                      |
|     RETURNS CHARACTER VARYING(ANY)                                                               |         RETURN CHARACTER VARYING                                                                 |
|     LANGUAGE NZPLSQL                                                                             |     AS                                                                                           |
|     AS                                                                                           |        v_B64 Varchar(64) := 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/';  |
|     BEGIN_PROC                                                                                   |        v_I int := 0;                                                                             |
|     DECLARE                                                                                      |        v_J int := 0;                                                                             |
|        v_B64 Varchar(64) := 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/';  |        v_K int := 0;                                                                             |
|        v_I int := 0;                                                                             |        v_N int := 0;                                                                             |
|        v_J int := 0;                                                                             |        v_out Numeric(38,0) := 0;                                                                 |
|        v_K int := 0;                                                                             |        I_LOAD_DT ALIAS FOR $1;                                                                   |
|        v_N int := 0;                                                                             |                                                                                                  |
|        v_out Numeric(38,0) := 0;                                                                 |       BEGIN                                                                                      |
|        I_LOAD_DT ALIAS FOR $1;                                                                   |         v_N:=Length(v_B64);                                                                      |
|                                                                                                  |         FOR v_I In Reverse 1..Length(IN_base64)                                                  |
|     BEGIN                                                                                        |         LOOP                                                                                     |
|         v_N:=Length(v_B64);                                                                      |           v_J:=Instr(v_B64,Substr(IN_base64,v_I,1))-1;                                           |
|         FOR v_I In Reverse 1..Length(IN_base64)                                                  |           If v_J <0 Then                                                                         |
|         LOOP                                                                                     |             RETURN -1;                                                                           |
|           v_J:=Instr(v_B64,Substr(IN_base64,v_I,1))-1;                                           |           End If;                                                                                |
|           If v_J <0 Then                                                                         |           V_Out:=V_Out+v_J*(v_N**v_K);                                                           |
|             RETURN -1;                                                                           |           v_K:=v_K+1;                                                                            |
|           End If;                                                                                |         END LOOP;                                                                                |
|           V_Out:=V_Out+v_J*(v_N**v_K);                                                           |         RETURN V_Out;                                                                            |
|           v_K:=v_K+1;                                                                            |       END;                                                                                       |
|         END LOOP;                                                                                |     /                                                                                            |
|         RETURN V_Out;                                                                            |                                                                                                  |
|     END                                                                                          |                                                                                                  |
|     END_PROC;                                                                                    |                                                                                                  |
+--------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------+

LOOP
----

+----------------------------------------------------------------+---------------------------------------------------------------+
| Netezza Syntax                                                 | Syntax After Migration                                        |
+================================================================+===============================================================+
| ::                                                             | ::                                                            |
|                                                                |                                                               |
|    CREATE OR REPLACE PROCEDURE sp_ntz_for_loop_with_more_dots  |    CREATE OR REPLACE FUNCTION sp_ntz_for_loop_with_more_dots  |
|          ( INTEGER )                                           |          ( INTEGER )                                          |
|         RETURNS CHARACTER VARYING(ANY)                         |       RETURN CHARACTER VARYING                                |
|         LANGUAGE NZPLSQL                                       |       AS                                                      |
|     AS BEGIN_PROC                                              |             p_abc INTEGER ;                                   |
|     DECLARE p_abc  INTEGER;                                    |             p_bcd  INTEGER;                                   |
|             p_bcd  INTEGER;                                    |             p_var1 ALIAS FOR $1;                              |
|             p_var1 ALIAS FOR $1;                               |                                                               |
|     BEGIN                                                      |     BEGIN                                                     |
|          p_bcd := ISNULL(p_var1, 10);                          |          p_bcd := NVL(p_var1, 10);                            |
|          RAISE NOTICE 'p_bcd=%', p_bcd;                        |                                                               |
|                                                                |          RAISE NOTICE 'p_bcd=%', p_bcd;                       |
|          FOR p_abc IN 0...(p_bcd)                              |                                                               |
|          LOOP                                                  |          FOR p_abc IN 0..(p_bcd)                              |
|               RAISE NOTICE 'hello world %', p_abc;             |          LOOP                                                 |
|          END LOOP;                                             |               RAISE NOTICE 'hello world %', p_abc;            |
|                                                                |                                                               |
|     END;                                                       |          END LOOP;                                            |
|     END_PROC;                                                  |                                                               |
|                                                                |     END;                                                      |
|                                                                |     /                                                         |
+----------------------------------------------------------------+---------------------------------------------------------------+

GaussDB(DWS) Keywords
---------------------

CURSOR

+-----------------------------------------------------------+----------------------------------------------------------------+
| Netezza Syntax                                            | Syntax After Migration                                         |
+===========================================================+================================================================+
| ::                                                        | ::                                                             |
|                                                           |                                                                |
|    CREATE OR REPLACE PROCEDURE sp_ntz_keyword_cursor()    |    CREATE OR REPLACE FUNCTION sp_ntz_keyword_cursor()          |
|         RETURNS INTEGER                                   |         RETURN INTEGER                                         |
|         LANGUAGE NZPLSQL                                  |     AS                                                         |
|     AS BEGIN_PROC                                         |        tablename NCHAR VARYING(100);                           |
|     DECLARE                                               |        mig_cursor RECORD;                                      |
|        tablename NVARCHAR(100);                           |     BEGIN                                                      |
|        cursor RECORD;                                     |        FOR mig_cursor IN (SELECT t.TABLENAME FROM _V_TABLE t   |
|     BEGIN                                                 |                       WHERE TABLENAME LIKE 'T_ODS_CRM%')       |
|        FOR cursor IN SELECT t.TABLENAME FROM _V_TABLE t   |        LOOP                                                    |
|                       WHERE TABLENAME LIKE 'T_ODS_CRM%'   |             tablename := mig_cursor.TABLENAME;                 |
|        LOOP                                               |        END LOOP;                                               |
|             tablename := cursor.TABLENAME;                |     END;                                                       |
|        END LOOP;                                          |     /                                                          |
|     END;                                                  |                                                                |
|     END_PROC;                                             |                                                                |
+-----------------------------------------------------------+----------------------------------------------------------------+

DECLARE
-------

+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Netezza Syntax                                                                                                                                                       | Syntax After Migration                                                                                                                                               |
+======================================================================================================================================================================+======================================================================================================================================================================+
| ::                                                                                                                                                                   | ::                                                                                                                                                                   |
|                                                                                                                                                                      |                                                                                                                                                                      |
|    CREATE OR REPLACE PROCEDURE sp_ntz_declare_inside_begin                                                                                                           |    CREATE OR REPLACE FUNCTION sp_ntz_declare_inside_begin                                                                                                            |
|          ( NATIONAL CHARACTER VARYING(10) )                                                                                                                          |          ( NATIONAL CHARACTER VARYING(10) )                                                                                                                          |
|     RETURNS INTEGER                                                                                                                                                  |     RETURN INTEGER                                                                                                                                                   |
|     LANGUAGE NZPLSQL                                                                                                                                                 |     AS                                                                                                                                                               |
|     AS BEGIN_PROC                                                                                                                                                    |         I_LOAD_DT ALIAS FOR $1;                                                                                                                                      |
|     DECLARE                                                                                                                                                          |     BEGIN                                                                                                                                                            |
|         I_LOAD_DT ALIAS FOR $1;                                                                                                                                      |        DECLARE                                                                                                                                                       |
|     BEGIN                                                                                                                                                            |                MYCUR              RECORD;                                                                                                                            |
|        DECLARE                                                                                                                                                       |                VIEWSQL1           NCHAR VARYING(4000);                                                                                                               |
|                MYCUR              RECORD;                                                                                                                            |        BEGIN                                                                                                                                                         |
|                VIEWSQL1           NVARCHAR(4000);                                                                                                                    |        FOR MYCUR IN ( SELECT VIEWNAME,VIEWSQL                                                                                                                        |
|        BEGIN                                                                                                                                                         |                            FROM T_DDW_AUTO_F5_VIEW_DEFINE                                                                                                            |
|        FOR MYCUR IN ( SELECT VIEWNAME,VIEWSQL                                                                                                                        |            WHERE OWNER = 'ODS_PROD' )                                                                                                                                |
|                            FROM T_DDW_AUTO_F5_VIEW_DEFINE                                                                                                            |              LOOP                                                                                                                                                    |
|            WHERE OWNER = 'ODS_PROD' )                                                                                                                                |                   VIEWSQL1 := MYCUR.VIEWSQL;                                                                                                                         |
|              LOOP                                                                                                                                                    |                   WHILE INSTR(VIEWSQL1,'v_p_etldate') > 0                                                                                                            |
|                   VIEWSQL1 := MYCUR.VIEWSQL;                                                                                                                         |          LOOP                                                                                                                                                        |
|                   WHILE INSTR(VIEWSQL1,'v_p_etldate') > 0                                                                                                            |                       VIEWSQL1 := SUBSTR(VIEWSQL1,1,INSTR(VIEWSQL1,'v_p_etldate') - 1)||''''||I_LOAD_DT||''''||SUBSTR(VIEWSQL1,INSTR(VIEWSQL1,'v_p_etldate') + 11);  |
|          LOOP                                                                                                                                                        |                   END LOOP;                                                                                                                                          |
|                       VIEWSQL1 := SUBSTR(VIEWSQL1,1,INSTR(VIEWSQL1,'v_p_etldate') - 1)||''''||I_LOAD_DT||''''||SUBSTR(VIEWSQL1,INSTR(VIEWSQL1,'v_p_etldate') + 11);  |                                                                                                                                                                      |
|                   END LOOP;                                                                                                                                          |                                                                                                                                                                      |
|                                                                                                                                                                      |                   EXECUTE IMMEDIATE VIEWSQL1;                                                                                                                        |
|                                                                                                                                                                      |              END LOOP;                                                                                                                                               |
|                   EXECUTE IMMEDIATE VIEWSQL1;                                                                                                                        |        END;                                                                                                                                                          |
|              END LOOP;                                                                                                                                               |                                                                                                                                                                      |
|        END;                                                                                                                                                          |                                                                                                                                                                      |
|                                                                                                                                                                      |        RETURN 0;                                                                                                                                                     |
|                                                                                                                                                                      |     END;                                                                                                                                                             |
|        RETURN 0;                                                                                                                                                     |     /                                                                                                                                                                |
|     END;                                                                                                                                                             |                                                                                                                                                                      |
|     END_PROC;                                                                                                                                                        |                                                                                                                                                                      |
+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------+

EXECUTE AS CALLER
-----------------

+--------------------------------------------------------+------------------------------------------------------+
| Netezza Syntax                                         | Syntax After Migration                               |
+========================================================+======================================================+
| ::                                                     | ::                                                   |
|                                                        |                                                      |
|    CREATE OR REPLACE PROCEDURE  sp_ntz_exec_as_caller  |    CREATE OR REPLACE FUNCTION sp_ntz_exec_as_caller  |
|          ( CHARACTER VARYING(512) )                    |          ( CHARACTER VARYING(512) )                  |
|         RETURNS INTEGER                                |         RETURN INTEGER                               |
|         LANGUAGE NZPLSQL                               |        SECURITY INVOKER                              |
|         EXECUTE AS CALLER                              |     AS                                               |
|     AS BEGIN_PROC                                      |      SQL ALIAS FOR $1;                               |
|     DECLARE                                            |     BEGIN                                            |
|      SQL ALIAS FOR $1;                                 |         EXECUTE IMMEDIATE SQL;                       |
|     BEGIN                                              |         RETURN 0;                                    |
|      EXECUTE IMMEDIATE SQL;                            |     END;                                             |
|      RETURN 0;                                         |     /                                                |
|     END;                                               |     ------------------------                         |
|     END_PROC;                                          |     CREATE OR REPLACE FUNCTION sp_ntz_exec_as_owner  |
|     ------------------------                           |          ( CHARACTER VARYING(512) )                  |
|     CREATE or replace PROCEDURE  sp_ntz_exec_as_owner  |         RETURN INTEGER                               |
|          ( CHARACTER VARYING(512) )                    |        SECURITY DEFINER                              |
|         RETURNS INTEGER                                |     AS                                               |
|         LANGUAGE NZPLSQL                               |      SQL ALIAS FOR $1;                               |
|         EXECUTE AS OWNER                               |     BEGIN                                            |
|     AS BEGIN_PROC                                      |         EXECUTE IMMEDIATE SQL;                       |
|     DECLARE                                            |         RETURN 0;                                    |
|      SQL ALIAS FOR $1;                                 |     END;                                             |
|     BEGIN                                              |     /                                                |
|      EXECUTE IMMEDIATE SQL;                            |                                                      |
|      RETURN 0;                                         |                                                      |
|     END;                                               |                                                      |
|     END_PROC;                                          |                                                      |
+--------------------------------------------------------+------------------------------------------------------+

Expression
----------

SELECT result assign into variable.

+----------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------+
| Netezza Syntax                                                                                                                   | Syntax After Migration                                                                                                           |
+==================================================================================================================================+==================================================================================================================================+
| ::                                                                                                                               | ::                                                                                                                               |
|                                                                                                                                  |                                                                                                                                  |
|    CREATE OR REPLACE PROCEDURE sp_sel_res_to_var                                                                                 |    CREATE OR REPLACE FUNCTION sp_sel_res_to_var                                                                                  |
|       ( NATIONAL CHARACTER VARYING(10) )                                                                                         |       ( NATIONAL CHARACTER VARYING(10) )                                                                                         |
|         RETURNS CHARACTER VARYING(ANY)                                                                                           |         RETURN CHARACTER VARYING                                                                                                 |
|         LANGUAGE NZPLSQL                                                                                                         |     AS                                                                                                                           |
|     AS BEGIN_PROC                                                                                                                |          counts INTEGER := 0 ;                                                                                                   |
|     DECLARE                                                                                                                      |          I_LOAD_DT ALIAS FOR $1 ;                                                                                                |
|          counts INTEGER := 0 ;                                                                                                   |     BEGIN                                                                                                                        |
|          I_LOAD_DT ALIAS FOR $1 ;                                                                                                |                                                                                                                                  |
|     BEGIN                                                                                                                        |             SELECT COUNT(*)                                                                                                      |
|                                                                                                                                  |         INTO COUNTS                                                                                                              |
|             COUNTS := SELECT COUNT( * )                                                                                          |               FROM tb_sel_res_to_var                                                                                             |
|                         FROM tb_sel_res_to_var                                                                                   |              WHERE ETLDATE = I_LOAD_DT;                                                                                          |
|                        WHERE ETLDATE = I_LOAD_DT;                                                                                |                                                                                                                                  |
|                                                                                                                                  |       EXECUTE IMMEDIATE 'insert into TABLES_COUNTS values( ''tb_sel_res_to_var'', ''' || I_LOAD_DT || ''', ' || COUNTS || ')' ;  |
|       EXECUTE IMMEDIATE 'insert into TABLES_COUNTS values( ''tb_sel_res_to_var'', ''' || I_LOAD_DT || ''', ' || COUNTS || ')' ;  |                                                                                                                                  |
|                                                                                                                                  |          RETURN '0' ;                                                                                                            |
|          RETURN '0' ;                                                                                                            |     END;                                                                                                                         |
|     END;                                                                                                                         |     /                                                                                                                            |
|     END_PROC;                                                                                                                    |                                                                                                                                  |
+----------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------+
