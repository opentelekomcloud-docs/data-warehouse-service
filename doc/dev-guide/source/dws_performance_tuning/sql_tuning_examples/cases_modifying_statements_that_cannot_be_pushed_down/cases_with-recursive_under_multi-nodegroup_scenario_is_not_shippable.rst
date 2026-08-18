:original_name: dws_04_0137.html

.. _dws_04_0137:

Cases: WITH-RECURSIVE under Multi-nodegroup Scenario Is Not Shippable
=====================================================================

Possible Cause
--------------

The tables in the query come from different logical clusters. To fix this, use an intermediate table to put all tables in one cluster, or isolate the WITH RECURSIVE statement to prevent performance issues.

Case 1: Using a Temporary Table to Dump Tables from Other Logical Clusters in the WITH RECURSIVE Statement to the Current One
-----------------------------------------------------------------------------------------------------------------------------

**Application scenario**: The WITH RECURSIVE statement includes one or two tables from other logical clusters. These tables have small data volumes or produce small result sets after filtering.

Original statement: The target table **dwltax.dwl_tax_tom_cit_tp_account_tmp** is in the logical cluster **LC_DW1**, while the source table **dwtaxdi.dwi_tax_cit_tp_account_r_i** is in the logical cluster **LC_DL1**.

::

   INSERT INTO dwltax.dwl_tax_tom_cit_tp_account_tmp(account_id,parentid)
   SELECT
       S.account_id,S.parentid
   FROM (SELECT
           DISTINCT S.ACCOUNT_ID, S.PARENTID
       FROM (
           WITH RECURSIVE tmp AS (
               SELECT 1 AS LV,
                   '/'||T.PARENTID AS path,
                   account_ID AS account_ID,
                   PARENTID AS PARENTID ,
                   ROW_number() over() PATH_NUM,
                   T.DEL_FLAG
               FROM dwtaxdi.dwi_tax_cit_tp_account_r_i t
               WHERE T.DEL_FLAG = 'N'
               AND T.ISLEAF_FLAG = 1

               UNION ALL

               SELECT 1+lv AS LV,
                   p.path||'/'||t.PARENTID AS path,
                   p.account_ID,
                   t.PARENTID AS PARENTID,
                   ROW_number() over() PATH_NUM,
                   T.DEL_FLAG
               FROM dwtaxdi.dwi_tax_cit_tp_account_r_i t
               INNER JOIN tmp p ON p.PARENTID = t.account_ID
           )

           SELECT lv,
               '/' ||account_ID||path|| '/'AS ITEM_ID_PATH,
               account_ID,
               PARENTID,
               PATH_NUM
           FROM tmp
           WHERE DEL_FLAG='N'
       ) S
   ) S

Rewritten statement

::

   CREATE TEMP TABLE DWI_TAX_CIT_TP_ACCOUNT_R_i -- Import the source table data to a temporary table in the logical cluster LC_DW1.
   WITH(orientation=column)
   TO GROUP "LC_DW1"
   AS SELECT *
   FROM dwtaxdi.DWI_TAX_CIT_TP_ACCOUNT_R_i;

   INSERT INTO dwltax.dwl_tax_tom_cit_tp_account_tmp(account_id,parentid) -- The source table and target table are in the logical cluster LC_DW1.
   SELECT
       S.account_id,S.parentid
   FROM (SELECT
           DISTINCT S.ACCOUNT_ID, S.PARENTID
       FROM (
           WITH RECURSIVE tmp AS (
               SELECT 1 AS LV,
                   '/'||T.PARENTID AS path,
                   account_ID AS account_ID,
                   PARENTID AS PARENTID ,
                   ROW_number() over() PATH_NUM,
                   T.DEL_FLAG
               FROM dwi_tax_cit_tp_account_r_i t
               WHERE T.DEL_FLAG = 'N'
               AND T.ISLEAF_FLAG = 1

               UNION ALL

               SELECT 1+lv AS LV,
                   p.path||'/'||t.PARENTID AS path,
                   p.account_ID,
                   t.PARENTID AS PARENTID,
                   ROW_number() over() PATH_NUM,
                   T.DEL_FLAG
               FROM dwi_tax_cit_tp_account_r_i t
               INNER JOIN tmp p ON p.PARENTID = t.account_ID
           )

           SELECT lv,
               '/' ||account_ID||path|| '/'AS ITEM_ID_PATH,
               account_ID,
               PARENTID,
               PATH_NUM
           FROM tmp
           WHERE DEL_FLAG='N'
       ) S
   ) S

Modification comparison

|image1|

**Optimized execution plan**

|image2|

Case 2: Dumping the Result of the WITH RECURSIVE Statement to a Temporary Table
-------------------------------------------------------------------------------

**Application scenario**: The WITH RECURSIVE result set is small, but the previous solution's result set is large. The common scenarios include:

-  The WITH RECURSIVE statement involves multiple tables in other logical sets, making it hard to dump tables.
-  The current statement does not directly use WITH RECURSIVE, but a called view does contain the WITH RECURSIVE logic.

Original statement

::

   INSERT INTO dwltax.bif_rpt_item_htp_tax_t(PERIOD_ID,REPORT_ITEM_ID,CRT_CYCLE_ID,LAST_UPD_CYCLE_ID,UPD_JOB_INSTANCE_ID, DEL_FLAG, CRT_JOB_INSTANCE_ID, UCCID )
   SELECT
       DISTINCT T.PERIOD_ID ,158004503,-1,-1,-1,'N',-1,T.UCCID
   FROM dwltax.dwl_tax_tom_taxpln_htp_tag_v T
   INNER JOIN  dwltax.dwl_tax_tom_cit_tp_account_v  TAX_E1502044 ON T.ACCOUNT_ID =  TAX_E1502044.ACCOUNT_ID
   INNER JOIN  dwltax.dwl_tax_tom_cit_tp_rf_tag_v  TAX_E1502045 ON T.RF_ID =  TAX_E1502045.RF_ID
   WHERE  1 = 1
   AND T.SOURCE_SYSTEM = 'HWHTPLOC01'
   AND TAX_E1502044.PARENTID_ACCOUNT_CODE IN ('TARFAdjustmentsManual','TARFAdjustmentsExpenseDeferredReclass')
   AND TAX_E1502045.PARENTID_RF_CODE = 'TARFTotal'
   AND T.DATACATEGORY_CODE IN ('TARFDeferredNonCurrentLossNR')
   AND T.PERIOD_ID=202308

The **dwltax.dwl_tax_tom_cit_tp_rf_tag_v** in the statement contains the WITH RECURSIVE logic.

::

   CREATE OR REPLACE VIEW dwltax.dwl_tax_tom_cit_tp_rf_tag_v AS  SELECT d2.rf_id AS parent_rf_id,
       d2.label AS parentid_rf_code, d1.rf_id, d1.label AS rf_code,
       d1.userdefined1, d1.userdefined2, d1.userdefined3, d1.en_us, d1.en_gb,
       d2.en_us AS parent_en_us, d2.en_gb AS parent_en_gb, d2.source_system
   FROM (SELECT
           DISTINCT r1.rf_id, r1.parentid
       FROM (SELECT
                   DISTINCT s.rf_id, s.parentid
               FROM (WITH RECURSIVE tmp AS (SELECT 1 AS lv,
                       '/'::text || t.parentid::text AS path,
                       t.rf_id, t.parentid
                   FROM dwtaxdi.dwi_tax_cit_tp_rf_r_i t1
                   RIGHT JOIN dwtaxdi.dwi_tax_cit_tp_rf_r_i t ON t.rf_id = t1.rf_id AND t.parentid = t1.parentid AND t1.del_flag::text = 'N'::text
                   WHERE t.del_flag::text = 'N'::text AND t.isleaf_flag = 1::numeric AND 1 = 1 AND 1 = 1 AND 1 = 1

                   UNION ALL

                   SELECT 1 + p.lv AS lv,
                       (p.path || '/'::text) || t.parentid::text AS path,
                       p.rf_id, t.parentid
                   FROM tmp p, dwtaxdi.dwi_tax_cit_tp_rf_r_i t1
                   RIGHT JOIN dwtaxdi.dwi_tax_cit_tp_rf_r_i t ON t.rf_id = t1.rf_id AND t.parentid = t1.parentid AND t1.del_flag::text = 'N'::text
                   WHERE p.parentid = t.rf_id AND t.del_flag::text = 'N'::text AND 1 = 1 AND 1 = 1 AND 1 = 1
               )

               SELECT (('/'::text || tmp.rf_id::text) || tmp.path) || '/'::text AS item_id_path,
                   tmp.rf_id, tmp.parentid
               FROM tmp
           ) s

           UNION ALL

           SELECT r.rf_id, r.rf_id AS parentid
           FROM dwtaxdi.dwi_tax_cit_tp_rf_r_i r
           WHERE r.isleaf_flag = 1::numeric AND r.del_flag::text = 'N'::text) r1
   ) r, dwtaxdi.dwi_tax_cit_tp_rf_i d1, dwtaxdi.dwi_tax_cit_tp_rf_i d2
   WHERE r.rf_id = d1.rf_id AND r.parentid = d2.rf_id;

The **dwltax.dwl_tax_tom_cit_tp_rf_tag_v** view has about 10,000 rows. To ensure the main query can be pushed down, save this result set to an intermediate table.

**Statement after optimization:**

::

   -- 8.2.1 or later
   CREATE TEMP TABLE dwl_tax_tom_cit_tp_rf_tag_v WITH(orientation=column, enable_hstore= true)
   AS
   SELECT * FROM dwltax.dwl_tax_tom_cit_tp_rf_tag_v

   -- 8.2.0 or earlier
   CREATE TEMP TABLE dwl_tax_tom_cit_tp_rf_tag_v WITH(orientation=row)
   AS
   SELECT * FROM dwltax.dwl_tax_tom_cit_tp_rf_tag_v

   INSERT /*+ set global(enable_force_vector_engine on)*/INTO
   -- If orientation is column and enable_hstore is true for dwl_tax_tom_cit_tp_rf_tag_v, hints are not required.
   dwltax.bif_rpt_item_htp_tax_t(PERIOD_ID,REPORT_ITEM_ID,CRT_CYCLE_ID,LAST_UPD_CYCLE_ID,UPD_JOB_INSTANCE_ID, DEL_FLAG, CRT_JOB_INSTANCE_ID, UCCID )
   SELECT
       DISTINCT T.PERIOD_ID ,158004503,-1,-1,-1,'N',-1,T.UCCID
   FROM dwltax.dwl_tax_tom_taxpln_htp_tag_v T
   INNER JOIN  dwltax.dwl_tax_tom_cit_tp_account_v  TAX_E1502044 ON T.ACCOUNT_ID =  TAX_E1502044.ACCOUNT_ID
   INNER JOIN  pg_temp.dwl_tax_tom_cit_tp_rf_tag_v  TAX_E1502045 ON T.RF_ID =  TAX_E1502045.RF_ID
   WHERE  1 = 1
   AND T.SOURCE_SYSTEM = 'HWHTPLOC01'
   AND TAX_E1502044.PARENTID_ACCOUNT_CODE IN ('TARFAdjustmentsManual','TARFAdjustmentsExpenseDeferredReclass')
   AND TAX_E1502045.PARENTID_RF_CODE = 'TARFTotal'
   AND T.DATACATEGORY_CODE IN ('TARFDeferredNonCurrentLossNR')
   AND T.PERIOD_ID=202308

Modification comparison

|image3|

.. |image1| image:: /_static/images/en-us_image_0000002494108170.png
.. |image2| image:: /_static/images/en-us_image_0000002494109438.png
.. |image3| image:: /_static/images/en-us_image_0000002494126362.png
