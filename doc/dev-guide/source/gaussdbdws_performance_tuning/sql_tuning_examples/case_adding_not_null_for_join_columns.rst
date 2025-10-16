:original_name: dws_04_0477.html

.. _dws_04_0477:

Case: Adding NOT NULL for JOIN Columns
======================================

If there are many **NULL** values in the **JOIN** columns, you can add the filter criterion **IS NOT NULL** to filter data in advance to improve the **JOIN** efficiency.

Before optimization
-------------------

::

   SELECT
    *
   FROM
   ( ( SELECT
     STARTTIME STTIME,
     SUM(NVL(PAGE_DELAY_MSEL,0)) PAGE_DELAY_MSEL,
     SUM(NVL(PAGE_SUCCEED_TIMES,0)) PAGE_SUCCEED_TIMES,
     SUM(NVL(FST_PAGE_REQ_NUM,0)) FST_PAGE_REQ_NUM,
     SUM(NVL(PAGE_AVG_SIZE,0)) PAGE_AVG_SIZE,
     SUM(NVL(FST_PAGE_ACK_NUM,0)) FST_PAGE_ACK_NUM,
     SUM(NVL(DATATRANS_DW_DURATION,0)) DATATRANS_DW_DURATION,
     SUM(NVL(PAGE_SR_DELAY_MSEL,0)) PAGE_SR_DELAY_MSEL
    FROM
     PS.SDR_WEB_BSCRNC_1DAY SDR
     INNER JOIN (SELECT
         BSCRNC_ID,
         BSCRNC_NAME,
         ACCESS_TYPE,
         ACCESS_TYPE_ID
        FROM
         nethouse.DIM_LOC_BSCRNC
        GROUP BY
         BSCRNC_ID,
         BSCRNC_NAME,
         ACCESS_TYPE,
         ACCESS_TYPE_ID) DIM
     ON SDR.BSCRNC_ID = DIM.BSCRNC_ID
     AND DIM.ACCESS_TYPE_ID IN (0,1,2)
     INNER JOIN nethouse.DIM_RAT_MAPPING RAT
     ON (RAT.RAT = SDR.RAT)
    WHERE
     ( (STARTTIME >= 1461340800
     AND STARTTIME < 1461427200) )
     AND RAT.ACCESS_TYPE_ID IN (0,1,2)
    GROUP BY STTIME ) ) ;

:ref:`Figure 1 <en-us_topic_0000002052813838__en-us_topic_0000001233883207_f557f925b95334298a446cb27ba7f464a>` shows the execution plan.

.. _en-us_topic_0000002052813838__en-us_topic_0000001233883207_f557f925b95334298a446cb27ba7f464a:

.. figure:: /_static/images/en-us_image_0000001233761911.jpg
   :alt: **Figure 1** Adding NOT NULL for JOIN columns (1)

   **Figure 1** Adding NOT NULL for JOIN columns (1)

After optimization
------------------

#. As shown in :ref:`Figure 1 <en-us_topic_0000002052813838__en-us_topic_0000001233883207_f557f925b95334298a446cb27ba7f464a>`, the sequential scan phase is time consuming.

#. The JOIN performance is poor because a large number of null values exist in the JOIN column **BSCRNC_ID** of the PS.SDR_WEB_BSCRNC_1DAY table.

   Therefore, you are advised to manually add **NOT NULL** for **JOIN** columns in the statement, as shown below:

   ::

      SELECT
       *
      FROM
      ( ( SELECT
        STARTTIME STTIME,
        SUM(NVL(PAGE_DELAY_MSEL,0)) PAGE_DELAY_MSEL,
        SUM(NVL(PAGE_SUCCEED_TIMES,0)) PAGE_SUCCEED_TIMES,
        SUM(NVL(FST_PAGE_REQ_NUM,0)) FST_PAGE_REQ_NUM,
        SUM(NVL(PAGE_AVG_SIZE,0)) PAGE_AVG_SIZE,
        SUM(NVL(FST_PAGE_ACK_NUM,0)) FST_PAGE_ACK_NUM,
        SUM(NVL(DATATRANS_DW_DURATION,0)) DATATRANS_DW_DURATION,
        SUM(NVL(PAGE_SR_DELAY_MSEL,0)) PAGE_SR_DELAY_MSEL
       FROM
        PS.SDR_WEB_BSCRNC_1DAY SDR
        INNER JOIN (SELECT
            BSCRNC_ID,
            BSCRNC_NAME,
            ACCESS_TYPE,
            ACCESS_TYPE_ID
           FROM
            nethouse.DIM_LOC_BSCRNC
           GROUP BY
            BSCRNC_ID,
            BSCRNC_NAME,
            ACCESS_TYPE,
            ACCESS_TYPE_ID) DIM
        ON SDR.BSCRNC_ID = DIM.BSCRNC_ID
        AND DIM.ACCESS_TYPE_ID IN (0,1,2)
        INNER JOIN nethouse.DIM_RAT_MAPPING RAT
        ON (RAT.RAT = SDR.RAT)
       WHERE
        ( (STARTTIME >= 1461340800
        AND STARTTIME < 1461427200) )
        AND RAT.ACCESS_TYPE_ID IN (0,1,2)
        and SDR.BSCRNC_ID is not null
       GROUP BY
        STTIME ) ) A;

   :ref:`Figure 2 <en-us_topic_0000002052813838__en-us_topic_0000001233883207_fig376817271615>` shows the execution plan.

   .. _en-us_topic_0000002052813838__en-us_topic_0000001233883207_fig376817271615:

   .. figure:: /_static/images/en-us_image_0000001493802070.jpg
      :alt: **Figure 2** Adding NOT NULL for JOIN columns (2)

      **Figure 2** Adding NOT NULL for JOIN columns (2)
