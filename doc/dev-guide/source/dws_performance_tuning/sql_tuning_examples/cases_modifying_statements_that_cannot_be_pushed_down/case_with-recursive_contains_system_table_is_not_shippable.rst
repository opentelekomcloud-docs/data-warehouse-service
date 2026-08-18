:original_name: dws_04_0132.html

.. _dws_04_0132:

Case: WITH-RECURSIVE Contains System Table Is Not Shippable
===========================================================

Possible Cause
--------------

The FROM clause is not complete in a branch of the RECURSIVE statement. Use the system catalog or system view (or DUAL) to replace the FROM clause.

Case 1: Using FROM DUAL Query in a RECURSIVE Branch
---------------------------------------------------

Original statement

::

   WITH recursive cte AS (
       SELECT
           TO_DATE(201701, 'YYYYMM') as level ,TO_DATE(20170131, 'YYYYMMDD')  LASTDAY
       FROM dual

       UNION ALL

       SELECT
           ADD_MONTHS(cte.LEVEL,   1) AS PERIOD,
           LAST_DAY(ADD_MONTHS(cte.LEVEL,   1)) AS LASTDAY
       FROM cte WHERE cte.LEVEL <=SYSDATE
   )
   SELECT
       TO_CHAR(cte.level,'YYYYMMDD') AS PERIOD , cte.LASTDAY
       FROM cte
   WHERE TO_CHAR(cte.level,'YYYYMMDD')<= TO_CHAR(SYSDATE,'YYYYMMDD')

Rewritten statement

::

   WITH recursive cte AS (
       SELECT
           TO_DATE(201701, 'YYYYMM') as level ,TO_DATE(20170131, 'YYYYMMDD')  LASTDAY
       FROM generate_series(1, 1)

       UNION ALL

       SELECT
           ADD_MONTHS(cte.LEVEL,   1) AS PERIOD,
           LAST_DAY(ADD_MONTHS(cte.LEVEL,   1)) AS LASTDAY
       FROM cte WHERE cte.LEVEL <=SYSDATE
   )
   SELECT
       TO_CHAR(cte.level,'YYYYMMDD') AS PERIOD , cte.LASTDAY
       FROM cte
   WHERE TO_CHAR(cte.level,'YYYYMMDD')<= TO_CHAR(SYSDATE,'YYYYMMDD')

Modification comparison

|image1|

.. |image1| image:: /_static/images/en-us_image_0000002525801453.png
