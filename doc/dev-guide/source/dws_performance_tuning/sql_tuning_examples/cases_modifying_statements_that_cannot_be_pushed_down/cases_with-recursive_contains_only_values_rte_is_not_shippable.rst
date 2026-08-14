:original_name: dws_04_0131.html

.. _dws_04_0131:

Cases: WITH-RECURSIVE Contains Only VALUES RTE Is Not Shippable
===============================================================

Possible Cause
--------------

The FROM clause is not contained in one of the branches of the RECURSIVE statement (only the VALUES clause or a statement similar to SELECT 1 is contained).

Case 1: No FROM Clause in a RECURSIVE Branch
--------------------------------------------

Original statement

::

   SELECT T.RPT_ITEM_ID, --Report item ID.
       T.RPT_ITEM_CODE,
       T.USER_GROUP_CODE AS USER_GROUP_CODE -- User group.
   FROM BIF.BIF_RPT_ITEM_DEF_T T,
   (WITH recursive cte AS (
           SELECT DISTINCT TRIM(SUBSTR('' :: text, INSTR('', ',', 1, 1) + 1, INSTR('', ',', 1, 2) - INSTR('', ',', 1, 1) - 1)) AS cte_RPT_ITEM_CODE,
               1 AS level
           FROM (SELECT '') AS tb0

           UNION ALL

           SELECT DISTINCT TRIM(SUBSTR('' :: text, INSTR('', ',', 1, cte.level + 1) + 1, INSTR('', ',', 1, cte.level + 2) - INSTR('', ',', 1, cte.level + 1) - 1)),
               cte.level + 1
           FROM (SELECT '') AS tb0, cte
           WHERE cte.level + 1 <= LENGTH('') - LENGTH(REPLACE('', ',', '')) - 1
       )
       SELECT
           DISTINCT cte_RPT_ITEM_CODE AS RPT_ITEM_CODE
       FROM cte
   ) T5
   WHERE NVL(INSTR(T.RPT_ITEM_FREQUENCE, 'M'), 0) > 0
   AND T.RPT_ITEM_CODE = NVL(T5.RPT_ITEM_CODE, T.RPT_ITEM_CODE)
   AND T.RPT_ITEM_TYPE = 1 --Whether the report item is a leaf report item. 1: yes; 0: no. Basic report item.
   AND T.ENABLE_FLAG = 1
   AND T.VERSION = '202308' --Use the snapshot and add conditions.
   ORDER BY T.RPT_ITEM_ID

Rewritten statement

::

   SELECT T.RPT_ITEM_ID, --Report item ID.
       T.RPT_ITEM_CODE,
       T.USER_GROUP_CODE AS USER_GROUP_CODE -- User group.
   FROM BIF.BIF_RPT_ITEM_DEF_T T,
   (WITH recursive cte AS (
           SELECT DISTINCT TRIM(SUBSTR('' :: text, INSTR('', ',', 1, 1) + 1, INSTR('', ',', 1, 2) - INSTR('', ',', 1, 1) - 1)) AS cte_RPT_ITEM_CODE,
               1 AS level
           FROM generate_series(1, 1) AS tb0

           UNION ALL

           SELECT DISTINCT TRIM(SUBSTR('' :: text, INSTR('', ',', 1, cte.level + 1) + 1, INSTR('', ',', 1, cte.level + 2) - INSTR('', ',', 1, cte.level + 1) - 1)),
               cte.level + 1
           FROM (SELECT '') AS tb0, cte
           WHERE cte.level + 1 <= LENGTH('') - LENGTH(REPLACE('', ',', '')) - 1
       )
       SELECT
           DISTINCT cte_RPT_ITEM_CODE AS RPT_ITEM_CODE
       FROM cte
   ) T5
   WHERE NVL(INSTR(T.RPT_ITEM_FREQUENCE, 'M'), 0) > 0
   AND T.RPT_ITEM_CODE = NVL(T5.RPT_ITEM_CODE, T.RPT_ITEM_CODE)
   AND T.RPT_ITEM_TYPE = 1 --Whether the report item is a leaf report item. 1: yes; 0: no. Basic report item.
   AND T.ENABLE_FLAG = 1
   AND T.VERSION = '202308' --Use the snapshot and add conditions.
   ORDER BY T.RPT_ITEM_ID

Modification comparison

|image1|

Case 2: No FROM Clause in a RECURSIVE Branch
--------------------------------------------

Original statement

::

   SELECT A.DYNM_COMP_ID,
       DECODE(B.LINE_NO, 1, '202308', A.VERSION)
   FROM BIF.BIF_DYNM_COMP_SOU_TBL_V A,
   (WITH recursive cte AS (
       SELECT 1 AS level
       UNION ALL
       SELECT cte.level + 1
       FROM cte
       WHERE cte.level + 1 < 3
       )
       SELECT
           level as LINE_NO
       FROM cte
   ) B
   WHERE EXISTS (SELECT 1
       FROM BIF.BIF_RPT_ITEM_DEF_MV RPT,
       BIF.BIF_PUB_SUBJECT_AREA_T SBJ,
       BIF.BIF_SNAPSHORT_SUBJECT_V TYP
       WHERE A.DYNM_COMP_ID = RPT.DYNM_COMP_ID
       AND RPT.VERSION = 'current'
       AND RPT.SUBJECT_AREA_ID = SBJ.SUBJECT_AREA_ID
       AND SBJ.SUBJECT_AREA_CODE =TYP.SUBJECT_CODE
       AND TYP.SUBJECT_TYPE ='TAX')
   AND A.VERSION = 'current'

Rewritten statement

::

   SELECT A.DYNM_COMP_ID,
       DECODE(B.LINE_NO, 1, '202308', A.VERSION)
   FROM BIF.BIF_DYNM_COMP_SOU_TBL_V A,
   (SELECT
       *
       FROM generate_series(1, 2) AS cte(LINE_NO)
   ) B
   WHERE EXISTS (SELECT 1
       FROM BIF.BIF_RPT_ITEM_DEF_MV RPT,
       BIF.BIF_PUB_SUBJECT_AREA_T SBJ,
       BIF.BIF_SNAPSHORT_SUBJECT_V TYP
       WHERE A.DYNM_COMP_ID = RPT.DYNM_COMP_ID
       AND RPT.VERSION = 'current'
       AND RPT.SUBJECT_AREA_ID = SBJ.SUBJECT_AREA_ID
       AND SBJ.SUBJECT_AREA_CODE =TYP.SUBJECT_CODE
       AND TYP.SUBJECT_TYPE ='TAX')
   AND A.VERSION = 'current'

Modification comparison

|image2|

Case 3: Using the VALUES Clause in a RECURSIVE Branch
-----------------------------------------------------

Original statement

::

   WITH RECURSIVE t(n) AS (
       VALUES (1)

       UNION ALL

       SELECT
           n+1
       FROM t
       WHERE n < (SELECT MAX(LENGTH(COMP_CODE)-LENGTH(REPLACE(COMP_CODE,',','')))+1 MAX_TOKENS
                                          FROM (SELECT DEPT_CODE,
                                                       to_char(APPLICABLE_GEO_PC_CODE) COMP_CODE
                                                  FROM SDIHR.MDM_DEPT_ACT_INFO_T_3600)
       )
   )
   SELECT n AS LVL FROM t

Rewritten statement

::

   WITH RECURSIVE t(n) AS (
       SELECT * FROM generate_series(1, 1)

       UNION ALL

       SELECT
           n+1
       FROM t
       WHERE n < (SELECT MAX(LENGTH(COMP_CODE)-LENGTH(REPLACE(COMP_CODE,',','')))+1 MAX_TOKENS
                                          FROM (SELECT DEPT_CODE,
                                                       to_char(APPLICABLE_GEO_PC_CODE) COMP_CODE
                                                  FROM SDIHR.MDM_DEPT_ACT_INFO_T_3600)
       )
   )
   SELECT n AS LVL FROM t

Modification comparison

|image3|

Case 4: Using the VALUES Clause in a RECURSIVE Branch
-----------------------------------------------------

Original statement

::

   WITH RECURSIVE t(n) AS (VALUES (1)
       UNION ALL
       SELECT n+1
       FROM t
       WHERE n < (SELECT MAX(LENGTH(COMP_CODE)-LENGTH(REPLACE(COMP_CODE,',','')))+1 MAX_TOKENS
       FROM (SELECT DEPT_CODE,
       to_char(APPLICABLE_GEO_PC_CODE) COMP_CODE
       FROM SDIHR.MDM_DEPT_ACT_INFO_T_3600))
   ) SELECT n AS LVL FROM t

Rewritten statement

::

   SELECT * FROM generate_series
       (1, (SELECT MAX(LENGTH(COMP_CODE)-LENGTH(REPLACE(COMP_CODE,',','')))+1 MAX_TOKENS
       FROM (SELECT DEPT_CODE,
       to_char(APPLICABLE_GEO_PC_CODE) COMP_CODE
       FROM SDIHR.MDM_DEPT_ACT_INFO_T_3600))
   ) AS t(lvl)

Modification comparison

|image4|

.. |image1| image:: /_static/images/en-us_image_0000002525799813.png
.. |image2| image:: /_static/images/en-us_image_0000002493600422.png
.. |image3| image:: /_static/images/en-us_image_0000002525800527.png
.. |image4| image:: /_static/images/en-us_image_0000002525920565.png
