:original_name: dws_04_0449.html

.. _dws_04_0449:

Optimizing Statistics
=====================

What Is Statistic Optimization
------------------------------

GaussDB(DWS) generates optimal execution plans based on the cost estimation. Optimizers need to estimate the number of data rows and the cost based on statistics collected using **ANALYZE**. Therefore, the statistics is vital for the estimation of the number of rows and cost. Global statistics are collected using **ANALYZE**: **relpages** and **reltuples** in the **pg_class** table; **stadistinct**, **stanullfrac**, **stanumbersN**, **stavaluesN**, and **histogram_bounds** in the **pg_statistic** table.

Example 1: Poor Query Performance Due to the Lack of Statistics
---------------------------------------------------------------

The query performance is often significantly impacted by the absence of statistics for tables or columns involved in the query.

The structure of the example table is as follows:

::

   CREATE TABLE LINEITEM
   (
   L_ORDERKEY         BIGINT        NOT NULL
   , L_PARTKEY        BIGINT        NOT NULL
   , L_SUPPKEY        BIGINT        NOT NULL
   , L_LINENUMBER     BIGINT        NOT NULL
   , L_QUANTITY       DECIMAL(15,2) NOT NULL
   , L_EXTENDEDPRICE  DECIMAL(15,2) NOT NULL
   , L_DISCOUNT       DECIMAL(15,2) NOT NULL
   , L_TAX            DECIMAL(15,2) NOT NULL
   , L_RETURNFLAG     CHAR(1)       NOT NULL
   , L_LINESTATUS     CHAR(1)       NOT NULL
   , L_SHIPDATE       DATE          NOT NULL
   , L_COMMITDATE     DATE          NOT NULL
   , L_RECEIPTDATE    DATE          NOT NULL
   , L_SHIPINSTRUCT   CHAR(25)      NOT NULL
   , L_SHIPMODE       CHAR(10)      NOT NULL
   , L_COMMENT        VARCHAR(44)   NOT NULL
   ) with (orientation = column, COMPRESSION = MIDDLE) distribute by hash(L_ORDERKEY);

   CREATE TABLE ORDERS
   (
   O_ORDERKEY        BIGINT        NOT NULL
   , O_CUSTKEY       BIGINT        NOT NULL
   , O_ORDERSTATUS   CHAR(1)       NOT NULL
   , O_TOTALPRICE    DECIMAL(15,2) NOT NULL
   , O_ORDERDATE     DATE NOT NULL
   , O_ORDERPRIORITY CHAR(15)      NOT NULL
   , O_CLERK         CHAR(15)      NOT NULL
   , O_SHIPPRIORITY  BIGINT        NOT NULL
   , O_COMMENT       VARCHAR(79)   NOT NULL
   )with (orientation = column, COMPRESSION = MIDDLE) distribute by hash(O_ORDERKEY);

The query statements are as follows:

::

   explain verbose select
   count(*) as numwait
   from
   lineitem l1,
   orders
   where
   o_orderkey = l1.l_orderkey
   and o_orderstatus = 'F'
   and l1.l_receiptdate > l1.l_commitdate
   and not exists (
   select
   *
   from
   lineitem l3
   where
   l3.l_orderkey = l1.l_orderkey
   and l3.l_suppkey <> l1.l_suppkey
   and l3.l_receiptdate > l3.l_commitdate
   )
   order by
   numwait desc;

You can perform the following operations to check whether **ANALYZE** has been executed on the tables or columns involved in the query to collect statistics.

#. Execute **EXPLAIN VERBOSE** to analyze the execution plan and check the warning information.

   .. code-block::

      WARNING:Statistics in some tables or columns(public.lineitem(l_receiptdate,l_commitdate,l_orderkey, l_suppkey), public.orders(o_orderstatus,o_orderkey)) are not collected.
      HINT:Do analyze for them in order to generate optimized plan.

#. To determine if poor query performance was caused by a lack of statistics in certain tables or columns, check if the following information exists in the log file located in the **pg_log** directory.

   .. code-block::

      2017-06-14 17:28:30.336 CST 140644024579856 20971684 [BACKEND] LOG:Statistics in some tables or columns(public.lineitem(l_receiptdate, l_commitdate,l_orderkey,
      .l_suppkey), public.orders(o_orderstatus,o_orderkey)) are not collected.
      2017-06-14 17:28:30.336 CST 140644024579856 20971684 [BACKEND] HINT:Do analyze for them in order to generate optimized plan.

After confirming that **ANALYZE** has not been executed on the relevant tables or columns, you can execute **ANALYZE** on the tables or columns reported in the WARNING or logs to resolve the issue of slow query performance due to a lack of statistics

Example 2: Setting **cost_param** to Optimize Query Performance
---------------------------------------------------------------

For details, see :ref:`Case: Configuring cost_param for Better Query Performance <en-us_topic_0000001579070006>`.

Example 3: Optimization is Not Accurate When Intermediate Results Exist in the Query Where **JOIN** Is Used for Multiple Tables
-------------------------------------------------------------------------------------------------------------------------------

**Symptom**: Query the personnel who have checked in an Internet cafe within 15 minutes before and after the check-in of a specified person.

::

   SELECT
   C.WBM,
   C.DZQH,
   C.DZ,
   B.ZJHM,
   B.SWKSSJ,
   B.XWSJ
   FROM
   b_zyk_wbswxx A,
   b_zyk_wbswxx B,
   b_zyk_wbcs C
   WHERE
   A.ZJHM = '522522******3824'
   AND A.WBDM = B.WBDM
   AND A.WBDM = C.WBDM
   AND abs(to_date(A.SWKSSJ,'yyyymmddHH24MISS') - to_date(B.SWKSSJ,'yyyymmddHH24MISS')) < INTERVAL '15 MINUTES'
   ORDER BY
   B.SWKSSJ,
   B.ZJHM
   limit 10 offset 0
   ;

:ref:`Figure 1 <en-us_topic_0000001578750674__en-us_topic_0000001233563159_fb2e54654e9f945dbbfcb3347aa535169>` shows the execution plan. This query takes about 12s.

.. _en-us_topic_0000001578750674__en-us_topic_0000001233563159_fb2e54654e9f945dbbfcb3347aa535169:

.. figure:: /_static/images/en-us_image_0000001233681839.png
   :alt: **Figure 1** Using an unlogged table (1)

   **Figure 1** Using an unlogged table (1)

**Optimization analysis:**

#. In the execution plan, index scan is used for node scanning, the **Join Filter** calculation in the external **NEST LOOP IN** statement consumes most of the query time, and the calculation uses the string addition and subtraction, and unequal-value comparison.

#. Use an unlogged table to record the Internet access time of the specified person. The start time and end time are processed during data insertion, and this reduces subsequent addition and subtraction operations.

   ::

      //Create a temporary unlogged table.
      CREATE UNLOGGED TABLE temp_tsw
      (
      ZJHM         NVARCHAR2(18),
      WBDM         NVARCHAR2(14),
      SWKSSJ_START NVARCHAR2(14),
      SWKSSJ_END   NVARCHAR2(14),
      WBM          NVARCHAR2(70),
      DZQH         NVARCHAR2(6),
      DZ           NVARCHAR2(70),
      IPDZ         NVARCHAR2(39)
      )
      ;
      //Insert the Internet access record of the specified person, and process the start time and end time.
      INSERT INTO
      temp_tsw
      SELECT
      A.ZJHM,
      A.WBDM,
      to_char((to_date(A.SWKSSJ,'yyyymmddHH24MISS') - INTERVAL '15 MINUTES'),'yyyymmddHH24MISS'),
      to_char((to_date(A.SWKSSJ,'yyyymmddHH24MISS') + INTERVAL '15 MINUTES'),'yyyymmddHH24MISS'),
      B.WBM,B.DZQH,B.DZ,B.IPDZ
      FROM
      b_zyk_wbswxx A,
      b_zyk_wbcs B
      WHERE
      A.ZJHM='522522******3824' AND A.WBDM = B.WBDM
      ;

      //Query the personnel who have check in an Internet cafe before and after 15 minutes of the check-in of the specified person. Convert their ID card number format to int8 in comparison.
      SELECT
      A.WBM,
      A.DZQH,
      A.DZ,
      A.IPDZ,
      B.ZJHM,
      B.XM,
      to_date(B.SWKSSJ,'yyyymmddHH24MISS') as SWKSSJ,
      to_date(B.XWSJ,'yyyymmddHH24MISS') as XWSJ,
      B.SWZDH
      FROM temp_tsw A,
      b_zyk_wbswxx B
      WHERE
      A.ZJHM <> B.ZJHM
      AND A.WBDM = B.WBDM
      AND (B.SWKSSJ)::int8 > (A.swkssj_start)::int8
      AND (B.SWKSSJ)::int8 < (A.swkssj_end)::int8
      order by
      B.SWKSSJ,
      B.ZJHM
      limit 10 offset 0
      ;

   The query takes about 7s. :ref:`Figure 2 <en-us_topic_0000001578750674__en-us_topic_0000001233563159_f57ecd89cf73847d1a2183ccf0eed5a4e>` shows the execution plan.

   .. _en-us_topic_0000001578750674__en-us_topic_0000001233563159_f57ecd89cf73847d1a2183ccf0eed5a4e:

   .. figure:: /_static/images/en-us_image_0000001188323770.png
      :alt: **Figure 2** Using an unlogged table (2)

      **Figure 2** Using an unlogged table (2)

#. In the previous plan, **Hash Join** has been executed, and a Hash table has been created for the large table **b_zyk_wbswxx**. The table contains large amounts of data, so the creation takes long time.

   **temp_tsw** contains only hundreds of records, and an equal-value connection is created between **temp_tsw** and **b_zyk_wbswxx** using wbdm (the Internet cafe code). Therefore, if **JOIN** is changed to **NEST LOOP JOIN**, index scan can be used for node scanning, and the performance will be boosted.

#. Execute the following statement to change **JOIN** to **NEST LOOP JOIN**.

   ::

      SET enable_hashjoin = off;

   :ref:`Figure 3 <en-us_topic_0000001578750674__en-us_topic_0000001233563159_f962ab19471574220b202823b708cecaf>` shows the execution plan. The query takes about 3s.

   .. _en-us_topic_0000001578750674__en-us_topic_0000001233563159_f962ab19471574220b202823b708cecaf:

   .. figure:: /_static/images/en-us_image_0000001188642238.png
      :alt: **Figure 3** Using an unlogged table (3)

      **Figure 3** Using an unlogged table (3)

#. Save the query result set in the unlogged table for paging display.

   If paging display needs to be achieved on the upper-layer application page, change the **offset** value to determine the result set on the target page. In this way, the previous query statement will be executed every time after a page turning operation, which causes long response latency.

   To resolve this problem, you are advised to use the unlogged table to save the result set.

   ::

      //Create an unlogged table to save the result set.
      CREATE UNLOGGED TABLE temp_result
      (
      WBM      NVARCHAR2(70),
      DZQH     NVARCHAR2(6),
      DZ       NVARCHAR2(70),
      IPDZ     NVARCHAR2(39),
      ZJHM     NVARCHAR2(18),
      XM       NVARCHAR2(30),
      SWKSSJ   date,
      XWSJ     date,
      SWZDH    NVARCHAR2(32)
      );

      //Insert the result set to the unlogged table. The insertion takes about 3s.
      INSERT INTO
      temp_result
      SELECT
      A.WBM,
      A.DZQH,
      A.DZ,
      A.IPDZ,
      B.ZJHM,
      B.XM,
      to_date(B.SWKSSJ,'yyyymmddHH24MISS') as SWKSSJ,
      to_date(B.XWSJ,'yyyymmddHH24MISS') as XWSJ,
      B.SWZDH
      FROM temp_tsw A,
      b_zyk_wbswxx B
      WHERE
      A.ZJHM <> B.ZJHM
      AND A.WBDM = B.WBDM
      AND (B.SWKSSJ)::int8 > (A.swkssj_start)::int8
      AND (B.SWKSSJ)::int8 < (A.swkssj_end)::int8
      ;

      //Perform paging query on the result set. The paging query takes about 10 ms.
      SELECT
      *
      FROM
      temp_result
      ORDER BY
      SWKSSJ,
      ZJHM
      LIMIT 10 OFFSET 0;

   .. caution::

      Collecting global statistics using ANALYZE improves query performance.

      If a performance problem occurs, you can use plan hint to adjust the query plan to the previous one. For details, see :ref:`Hint-based Tuning <en-us_topic_0000001578910050>`.
