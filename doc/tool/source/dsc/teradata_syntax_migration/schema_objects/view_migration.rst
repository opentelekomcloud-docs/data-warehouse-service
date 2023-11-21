:original_name: dws_mt_0065.html

.. _dws_mt_0065:

View Migration
==============

**CREATE VIEW** (:ref:`short key <en-us_topic_0000001233922185__en-us_topic_0238518364_en-us_topic_0237362468_section75711541419>` CV) is used together with **SELECT** to create a view.

The keyword **VIEW** is supported by both Teradata and GaussDB(DWS), but the **SELECT** statements are enclosed in double quotation marks during the migration. For details, see the following figures.

Use the :ref:`tdMigrateVIEWCHECKOPTION <en-us_topic_0000001233922159__en-us_topic_0218440346_li166012191211>` configuration parameter to configure migration of views containing the WITH CHECK OPTION keyword. If **tdmigrateVIEWCHECKOPTION** is set to **false**, the tool will skip migration of the query and will log a message.

If the CREATE VIEW includes the LOCK keyword, then the VIEW query will be migrated based on the value of :ref:`tdMigrateLOCKoption <en-us_topic_0000001233922159__en-us_topic_0218440346_li18084318118>`.

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

**Input :CREATE VIEW WITH FORCE KEYWORD**

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

REPLACE VIEW
------------

In Teradata, the **REPLACE VIEW** statement is used to create a view or rebuild the existing view. DSC converts the **REPLACE VIEW** statement to the **CREATE OR REPLACE VIEW** statement that is compatible with GaussDB(DWS).

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

.. _en-us_topic_0000001188202532__en-us_topic_0238518356_en-us_topic_0237362518_section626052234019:

CHECK OPTION
------------

Use the :ref:`tdMigrateVIEWCHECKOPTION <en-us_topic_0000001233922159__en-us_topic_0218440346_li166012191211>` configuration parameter to configure migration of views containing the **CHECK OPTION** keyword

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

VIEW WITH RECURSIVE
-------------------

GaussDB(DWS) does not support the Teradata keyword **RECURSIVE VIEW**. Therefore the keyword is replaced with **VIEW WITH RECURSIVE** keyword as shown in the following figures.


.. figure:: /_static/images/en-us_image_0000001188521204.png
   :alt: **Figure 1** Input view-CREATE RECURSIVE VIEW

   **Figure 1** Input view-CREATE RECURSIVE VIEW


.. figure:: /_static/images/en-us_image_0000001188362654.png
   :alt: **Figure 2** Output view

   **Figure 2** Output view

VIEW WITH ACCESS LOCK
---------------------

Use the :ref:`tdMigrateLOCKOption <en-us_topic_0000001233922159__en-us_topic_0218440346_li18084318118>` configuration parameter to configure migration of query containing the LOCK keyword. If **tdMigrateLOCKOption** is set to **false**, the tool will skip migration of the query and will log a message.

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
