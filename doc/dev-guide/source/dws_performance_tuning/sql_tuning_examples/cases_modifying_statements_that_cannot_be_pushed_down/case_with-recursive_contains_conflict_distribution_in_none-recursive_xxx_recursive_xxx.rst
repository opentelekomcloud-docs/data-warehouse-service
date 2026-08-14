:original_name: dws_04_0138.html

.. _dws_04_0138:

Case: WITH-RECURSIVE Contains Conflict Distribution in None-RECURSIVE (Xxx) RECURSIVE (Xxx)
===========================================================================================

Possible Cause
--------------

The non-recursive part (before UNION ALL) uses a replicate plan, while the recursive part (after UNION ALL) uses a hash plan. These plans don't match.

**Solution**: Dump the non-recursive statements to a temporary table to ensure that the distribution of the temporary table is different from that of the plan.

Case 1: Using the Replicate Plan for Non-RECURSIVE Statements
-------------------------------------------------------------

Original statement

::

   WITH RECURSIVE CTE AS (
       SELECT
           AOH.ORG_CODE,1 AS LEVEL,
           1 FLAG,
           LAST_UPDATE_DATE
       FROM SDIHR.HW_HR_ACCOUNT_ORG_HY_V_1100 AOH
       WHERE AOH.ORG_CODE IN (SELECT V.ORG_CODE
           FROM SDIHR.HW_HR_ACCOUNT_ORG_HY_V_1100 V
           WHERE V.ORG_CODE IS NOT NULL
           AND AOH.ORG_CODE = V.ORG_CODE
       )

       UNION ALL

       SELECT
         AOH.ORG_CODE,T.LEVEL + 1 AS LEVEL,
         1 FLAG,
         AOH.LAST_UPDATE_DATE
       FROM SDIHR.HW_HR_ACCOUNT_ORG_HY_V_1100 AOH
       INNER JOIN CTE T ON T.ORG_CODE = AOH.PARENT_ORG_CODE
   )
   SELECT
       ORG_CODE,
       MAX(LEVEL) -1 AS AOH_LEVEL,
       1 FLAG,
       LAST_UPDATE_DATE AS LAST_UPDATE_DATE1
   FROM CTE
   GROUP BY ORG_CODE, LAST_UPDATE_DATE

SQL self-diagnosis information

|image1|

Rewritten statement

::

   CREATE TEMP TABLE t WITH(orientation=column) DISTRIBUTE BY ROUNDROBIN
   AS
   SELECT
       AOH.ORG_CODE,1 AS LEVEL,
       1 FLAG,
       LAST_UPDATE_DATE
   FROM SDIHR.HW_HR_ACCOUNT_ORG_HY_V_1100 AOH
   WHERE AOH.ORG_CODE IN (SELECT V.ORG_CODE
       FROM SDIHR.HW_HR_ACCOUNT_ORG_HY_V_1100 V
       WHERE V.ORG_CODE IS NOT NULL
       AND AOH.ORG_CODE = V.ORG_CODE
   );

   WITH RECURSIVE CTE AS (
       SELECT * FROM t

       UNION ALL

       SELECT
         AOH.ORG_CODE,T.LEVEL + 1 AS LEVEL,
         1 FLAG,
         AOH.LAST_UPDATE_DATE
       FROM SDIHR.HW_HR_ACCOUNT_ORG_HY_V_1100 AOH
       INNER JOIN CTE T ON T.ORG_CODE = AOH.PARENT_ORG_CODE
   )
   SELECT
       ORG_CODE,
       MAX(LEVEL) -1 AS AOH_LEVEL,
       1 FLAG,
       LAST_UPDATE_DATE AS LAST_UPDATE_DATE1
   FROM CTE
   GROUP BY ORG_CODE, LAST_UPDATE_DATE

Modification comparison

|image2|

.. |image1| image:: /_static/images/en-us_image_0000002494126958.png
.. |image2| image:: /_static/images/en-us_image_0000002494127274.png
