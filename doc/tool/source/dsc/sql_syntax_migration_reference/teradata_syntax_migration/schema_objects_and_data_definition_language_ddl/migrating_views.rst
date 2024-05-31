:original_name: dws_16_0076.html

.. _dws_16_0076:

.. _en-us_topic_0000001819416153:

Migrating Views
===============

**CREATE VIEW** (:ref:`short key <en-us_topic_0000001772696108>` CV) is used together with **SELECT** to create a view.

Teradata, GaussDB T, GaussDB A, and GaussDB(DWS) all support the keyword **VIEW**. It is enclosed by () in SELECT statements during migration. For details, see the following figures.

Use the :ref:`tdMigrateVIEWCHECKOPTIO.... <en-us_topic_0000001819416085__en-us_topic_0000001706224349_en-us_topic_0000001432527901_li166012191211>` configuration parameter to configure migration of views containing the WITH CHECK OPTION keyword. If **tdmigrateVIEWCHECKOPTION** is set to **false**, the tool will skip migration of the query and will log a message.

If the CREATE VIEW includes the LOCK keyword, then the VIEW query will be migrated based on the value of :ref:`tdMigrateLOCKoption <en-us_topic_0000001819416085__en-us_topic_0000001706224349_en-us_topic_0000001432527901_li18084318118>`.

**Input - CREATE VIEW**

::

   CREATE VIEW DP_STEDW.MY_PARAM
   AS
   SELECT RUNDATE FROM  DP_STEDW.DATE_TBL   WHERE   dummy = 1;

**Output**

::

   CREATE OR REPLACE VIEW DP_STEDW.MY_PARAM
   AS
   SELECT RUNDATE
     FROM  DP_STEDW.DATE_TBL
           WHERE   dummy = 1;

**Input: CREATE VIEW WITH FORCE KEYWORD**

::

   CREATE
   OR REPLACE FORCE VIEW IS2010_APP_INFO (
     APP_ID, APP_SHORTNAME, APP_CHNAME,
     APP_ENNAME
   ) AS
   select
     t.app_id,
     t.app_shortname,
     t.app_chname,
     t.app_enname
   from
     newdrms.seas_app_info t
   WHERE
     t.app_status <> '2';

**Output**

::

   CREATE
   OR REPLACE
   /*FORCE*/
   VIEW IS2010_APP_INFO (
       APP_ID,
       APP_SHORTNAME,
       APP_CHNAME,
       APP_ENNAME ) AS
   SELECT
       t.app_id,
       t.app_shortname,
       t.app_chname,
       t.app_enname
   FROM
       newdrms.seas_app_info t
   WHERE
   t.app_status <> '2';

.. _en-us_topic_0000001819416153__en-us_topic_0000001706224093_en-us_topic_0000001434790465_section920213323511:

REPLACE VIEW
------------

In Teradata, the **REPLACE VIEW** statement is used to create a view or rebuild the existing view. DSC migrates it into the statements of the **CREATE OR REPLACE VIEW** that is compatible with GaussDB T, GaussDB A, and GaussDB(DWS).

**Input - REPLACE VIEW**

::

   REPLACE VIEW DP_STEDW.MY_PARAM AS SELECT
             RUNDATE
        FROM
             DP_STEDW.DATE_TBL
        WHERE
             dummy = 1
   ;

**Output**

::

   CREATE
   OR REPLACE VIEW DP_STEDW.MY_PARAM AS (
        SELECT
                  RUNDATE
             FROM
                  DP_STEDW.DATE_TBL
             WHERE
                  dummy = 1
   )
   ;

**Input - REPLACE RECURSIVE VIEW**

::

   Replace RECURSIVE VIEW reachable_from (
   emp_id,emp_name,DEPTH)
   AS (
   SELECT root.emp_id,root.emp_name,0 AS DEPTH
   FROM emp AS root
   WHERE root.mgr_id IS NULL);

**Output**

::

   CREATE OR REPLACE VIEW reachable_from AS (
   WITH RECURSIVE reachable_from (
   emp_id,emp_name,DEPTH)
   AS (
   SELECT root.emp_id,root.emp_name,0 AS DEPTH
   FROM emp AS root
   WHERE root.mgr_id IS NULL
   ) SELECT * FROM reachable_from);

REPLACE FUNCTION
----------------

**Input**

.. code-block::

   REPLACE FUNCTION up_load1.RPT_016_BUS_DATE()
   RETURNS DATE
   LANGUAGE SQL
   CONTAINS SQL
   DETERMINISTIC
   SQL SECURITY DEFINER
   COLLATION INVOKER
   INLINE TYPE 1
   RETURN DATE'2017-08-22';

**Output**

.. code-block::

   CREATE OR REPLACE FUNCTION up_load1.RPT_016_BUS_DATE()
   RETURNS DATE
   LANGUAGE SQL
   IMMUTABLE
   SECURITY DEFINER
   AS
   $$
   SELECT CAST('2017-08-20' AS DATE)
   $$
   ;

.. _en-us_topic_0000001819416153__en-us_topic_0000001706224093_en-us_topic_0000001434790465_section626052234019:

CHECK OPTION
------------

Use the :ref:`tdMigrateVIEWCHECKOPTIO.... <en-us_topic_0000001819416085__en-us_topic_0000001706224349_en-us_topic_0000001432527901_li166012191211>` configuration parameter to configure migration of views containing the WITH CHECK OPTION keyword.

If a view with **CHECK OPTION** is present in the source, then the **CHECK OPTION** is commented from the target database.

**Input - VIEW with CHECK OPTION**

::

   CV  mgr15 AS SEL *
   FROM
       employee
   WHERE
       manager_id = 15 WITH CHECK OPTION
   ;

**Output** **(tdMigrateVIEWCHECKOPTION=True**)

::

   CREATE
        OR REPLACE VIEW mgr15 AS (
             SELECT
                       *
                  FROM
                       employee
                  WHERE
                       manager_id = 15 /*WITH CHECK OPTION */
        )
   ;

**Output** **(tdMigrateVIEWCHECKOPTION=False**)

::

   CV  mgr15 AS SEL *
   FROM
       employee
   WHERE
       manager_id = 15 WITH CHECK OPTION
   ;

.. _en-us_topic_0000001819416153__en-us_topic_0000001706224093_en-us_topic_0000001434790465_section101871839202310:

VIEW WITH RECURSIVE
-------------------

GaussDB T, GaussDB A, and GaussDB(DWS) do not support the Teradata keyword **RECURSIVE VIEW.** Therefore the keyword is replaced with **VIEW WITH RECURSIVE** keyword as shown in the following figures.


.. figure:: /_static/images/en-us_image_0000001434809241.png
   :alt: **Figure 1** Input view-CREATE RECURSIVE VIEW

   **Figure 1** Input view-CREATE RECURSIVE VIEW


.. figure:: /_static/images/en-us_image_0000001384569256.png
   :alt: **Figure 2** Output view

   **Figure 2** Output view

.. _en-us_topic_0000001819416153__en-us_topic_0000001706224093_en-us_topic_0000001434790465_section11504125643219:

VIEW WITH ACCESS LOCK
---------------------

Use the :ref:`tdMigrateLOCKOption <en-us_topic_0000001819416085__en-us_topic_0000001706224349_en-us_topic_0000001432527901_li18084318118>` configuration parameter to configure migration of query containing the LOCK keyword. If **tdMigrateLOCKOption** is set to **false**, the tool will skip migration of the query and will log a message.

**Input - VIEW** **with ACCESS LOCK**

::

   CREATE OR REPLACE VIEW DP_SVMEDW.S_LCR_909_001_LCRLOAN
    AS
    LOCK TABLE DP_STEDW.S_LCR_909_001_LCRLOAN FOR ACCESS  FOR ACCESS
    ( SELECT RUN_ID, PRODUCT_ID, CURRENCY
               , CASHFLOW, ENTITY, LCR
               , TIME_BUCKET, MT, Ctl_Id
               , File_Id, Business_Date
         FROM DP_STEDW.S_LCR_909_001_LCRLOAN ) ;

**Output**

::

   CREATE OR REPLACE VIEW DP_SVMEDW.S_LCR_909_001_LCRLOAN
    AS
   /* LOCK TABLE DP_STEDW.S_LCR_909_001_LCRLOAN FOR ACCESS */
    ( SELECT RUN_ID, PRODUCT_ID, CURRENCY
               , CASHFLOW, ENTITY, LCR
               , TIME_BUCKET, MT, Ctl_Id
               , File_Id, Business_Date
         FROM DP_STEDW.S_LCR_909_001_LCRLOAN ) ;

**dbc.columnsV**

+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| .. code-block::                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | .. code-block::                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
|    SELECT A.ColumnName AS V_COLS ,A.columnname ||' ' ||CASE WHEN columnType in ('CF','CV') THEN CASE WHEN columnType='CV' THEN 'VAR' ELSE'' END||'CHAR('||TRIM(columnlength (INT))||') CHARACTER SET LATIN'|| CASE WHEN UpperCaseFlag='N' THEN' NOT' ELSE'' END ||' CASESPECIFIC' WHEN columnType='DA' THEN 'DATE' WHEN columnType='TS' THEN 'TIMESTAMP(' || TRIM(DecimalFractionalDigits)||')' WHEN columnType='AT' THEN 'TIME('|| TRIM(DecimalFractionalDigits)||')' WHEN columnType='I' THEN 'INTEGER' WHEN columnType='I1' THEN 'BYTEINT' WHEN columnType='I2' THEN 'SMALLINT' WHEN columnType='I8' THEN 'BIGINT' WHEN columnType='D' THEN 'DECIMAL('||TRIM(DecimalTotalDigits)||','||TRIM(DecimalFractionalDigits)||')' ELSE 'Unknown' END||CASE WHEN Nullable='Y' THEN'' ELSE' NOT NULL' END||'0A'XC AS V_ColT - ,B.ColumnName AS V_PICol -- obtains the primary index FROM dbc.columnsV A LEFT JOIN dbc.IndicesV B ON A.columnName = B.columnName AND B.IndexType IN ('Q','P') AND B.DatabaseName = '${V_TDDLDB}' AND B.tablename='${TARGET_TABLE}' WHERE A.databasename='${V_TDDLDB}' AND A.tablename = '${TARGET_TABLE}' AND A.columnname NOT IN ( 'ETL_JOB_NAME' ,'ETL_TX_DATE' ,'ETL_PROC_DATE' )ORDER BY A.columnid of the target table. |    D DECLARE lv_mig_V_COLS   TEXT;         lv_mig_V_ColT        TEXT;         lv_mig_V_PICol       TEXT; BEGIN SELECT STRING_AGG(A.ColumnName, ',')                   , STRING_AGG(A.columnname  || ' ' ||CASE WHEN columnType in ('CF','CV')                                       THEN CASE WHEN columnType='CV' THEN 'VAR' ELSE ''             END||'CHAR('||TRIM(mig_td_ext.mig_fn_castasint(columnlength))||                 ') /*CHARACTER SET LATIN*/'||                   CASE WHEN UpperCaseFlag='N'                      THEN ' NOT' ELSE ''                   END || ' /*CASESPECIFIC*/'                                  WHEN columnType='DA' THEN 'DATE'                                  WHEN columnType='TS' THEN 'TIMESTAMP(' || TRIM(DecimalFractionalDigits)||')'                                  WHEN columnType='AT' THEN 'TIME('|| TRIM(DecimalFractionalDigits)||')'                                  WHEN columnType='I' THEN 'INTEGER'                                  WHEN columnType='I1' THEN 'BYTEINT'                                  WHEN columnType='I2' THEN 'SMALLINT'                                  WHEN columnType='I8' THEN 'BIGINT'                                  WHEN columnType='D' THEN 'DECIMAL('||TRIM(DecimalTotalDigits)||','||TRIM(DecimalFractionalDigits)||')'                                  ELSE 'Unknown'                              END||CASE WHEN Nullable='Y'        THEN '' ELSE ' NOT NULL' END||E'\x0A', ',')                  , STRING_AGG(B.ColumnName, ',')                INTO lv_mig_V_COLS, lv_mig_V_ColT, lv_mig_V_PICol FROM mig_td_ext.vw_td_dbc_columnsV A LEFT JOIN mig_td_ext.vw_td_dbc_IndicesV B  ON A.columnName = B.columnName AND B.IndexType IN ('Q','P') AND B.DatabaseName = 'public'  AND B.tablename='emp2' WHERE A.databasename='public' AND A.tablename = 'emp2'; -- ORDER BY A.columnid; END; /  |
+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
