:original_name: dws_04_0136.html

.. _dws_04_0136:

Case: Main Plan Exec on CN and Subplan Exec on DN Can Not Be Shipped
====================================================================

Possible Cause
--------------

Possible cause: The main query in the complex statement does not involve user tables, but the related subquery does.

Case 1: Using the Dimension Table DUAL As the Main Query's Source Table
-----------------------------------------------------------------------

Original statement

::

   SELECT
       1
   FROM (SELECT 1
       FROM dual
       WHERE -2 = -2
       AND 202210 IN(SELECT /*+ PARALLEL(8) */
           DISTINCT PERIOD_ID
           FROM DWICBG_VI.CBGDWR_PSI_FIN_OVER_INV_V T
           WHERE T.LAST_MODIFIED_DATE > (SELECT NVL(MAX(LAST_MODIFIED_DATE), TRUNC(SYSDATE) - 1) from FIN_DWB_MSS.DWB_MSS_PSI_FIN_OVER_INV_M_F) AND T.LAST_MODIFIED_DATE <= (SELECT  TRUNC(SYSDATE) + 1)
       )
   ) t LIMIT 1

Rewritten statement

::

   SELECT
       1
   FROM(SELECT 1
       FROM (SELECT /*+ PARALLEL(8) */
               DISTINCT PERIOD_ID
           FROM DWICBG_VI.CBGDWR_PSI_FIN_OVER_INV_V T
           WHERE T.LAST_MODIFIED_DATE > (SELECT NVL(MAX(LAST_MODIFIED_DATE), TRUNC(SYSDATE) - 1) from FIN_DWB_MSS.DWB_MSS_PSI_FIN_OVER_INV_M_F) AND T.LAST_MODIFIED_DATE <= (SELECT  TRUNC(SYSDATE) + 1)
       ) t(PERIOD_ID)
   where PERIOD_ID = 202210
   ) t LIMIT 1

Modification comparison

|image1|

.. |image1| image:: /_static/images/en-us_image_0000002493938586.png
