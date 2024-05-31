:original_name: dws_16_0043.html

.. _dws_16_0043:

.. _en-us_topic_0000001819416125:

Analytical Functions
====================

Analytical functions are collectively called ordered analytical functions in Teradata, and they provide powerful analytical abilities for data mining, analysis and business intelligence.

.. _en-us_topic_0000001819416125__en-us_topic_0000001657865294_en-us_topic_0000001434910237_section08780617917:

Analytical Functions in ORDER BY
--------------------------------

**Input:** **Analytic function in ORDER BY clause**

::

   SELECT customer_id, customer_name, RANK(customer_id, customer_address DESC)
      FROM customer_t
    WHERE  customer_state = 'CA'
    ORDER BY RANK(customer_id, customer_address DESC);

**Output**

::

   SELECT customer_id, customer_name, RANK() over(order by customer_id, customer_address DESC)
      FROM customer_t
    WHERE  customer_state = 'CA'
    ORDER BY RANK() over(order by customer_id DESC, customer_address DESC) ;

**Input:** **Analytic function in GROUP BY clause**

::

   SELECT customer_city, customer_state, postal_code
        , rank(postal_code)
        , rank() over(partition by customer_state order by postal_code)
       , rank() over(order by postal_code)
     FROM Customer_T
     GROUP BY customer_state
     ORDER BY customer_state;

**Output**

::

   SELECT customer_city, customer_state, postal_code
        , rank() over(PARTITION BY customer_state ORDER BY postal_code DESC)
        , rank() over(partition by customer_state order by postal_code)
        , rank() over(order by postal_code)
     FROM Customer_T
     ORDER BY customer_state;

.. _en-us_topic_0000001819416125__en-us_topic_0000001657865294_en-us_topic_0000001434910237_section185781189816:

**Analytical Functions in PARTITION BY**
----------------------------------------

When the input script contains a numeric value in the PARTITION BY clause, the migrated script retains the numeric value as it is.

**Input:** **Analytic function in PARTITION BY clause** (with numeric value)

::

   SELECT
             Customer_id
             ,customer_name
             ,rank (
             ) over( partition BY 1 ORDER BY Customer_id )
             ,rank (customer_name)
        FROM
             Customer_t
        GROUP BY
             1
   ;

**Output**

.. code-block::

   SELECT
             Customer_id
             ,customer_name
             ,rank (
             ) over( partition BY 1 ORDER BY Customer_id )
             ,rank (
             ) over( PARTITION BY Customer_id ORDER BY customer_name DESC )
        FROM
             Customer_t
   ;

Window Functions
----------------

Window functions perform calculations across rows of the query result. DSC supports the following Teradata window functions:

.. note::

   The DSC supports only single occurrence of window function in QUALIFY clause. Multiple window functions in a QUALIFY may result in invalid migration.

CSUM
----

The Cumulative Sum (CSUM) function provides a running or cumulative total for a column's numeric value. It is recommended that ALIAS be used in the QUALIFY statements.

**Input - CSUM with GROUP_ID**

::

   INSERT INTO GSIS_SUM.DW_DAT71 (
      col1
     ,PROD_GROUP
   )
      SELECT
         CSUM(1, T1.col1)
         ,T1.PROD_GROUP
        FROM tab1  T1
       WHERE T1.col1 = 'ABC'
   ;

**Output**

::

   INSERT
        INTO
             GSIS_SUM.DW_DAT71 (
                  col1
                  ,PROD_GROUP
             ) SELECT
                       SUM (1) over( ORDER BY T1.col1 ROWS UNBOUNDED PRECEDING )
                       ,T1.PROD_GROUP
                  FROM
                       tab1 T1
                  WHERE
                       T1.col1 = 'ABC'
   ;

**Input - CSUM with GROUP_ID**

::

   SELECT  top 10
         CSUM(1, T1.Test_GROUP)
         ,T1.col1
     FROM  $[schema}.  T1
    WHERE T1.Test_GROUP = 'Test_group' group by Test_group order by Test_Group;

**Output**

::

   SELECT
          SUM (1) over( partition BY Test_group ORDER BY T1.Test_GROUP ROWS UNBOUNDED PRECEDING )
          ,T1.col1
     FROM
          $[schema}. T1
    WHERE
          T1.Test_GROUP = 'Test_group'
    ORDER BY
          Test_Group LIMIT 10
   ;

**Input - CSUM with GROUP BY + QUALIFY**

::

   SELECT c1, c2, c3, CSUM(c4, c3)
     FROM tab1
   QUALIFY ROW_NUMBER(c4) = 1
   GROUP BY 1, 2;

**Output**

::

   SELECT c1, c2, c3, ColumnAlias1
     FROM ( SELECT c1, c2, c3
                 , SUM (c4) OVER(PARTITION BY 1 ,2 ORDER BY c3 ROWS UNBOUNDED PRECEDING) AS ColumnAlias1
                 , ROW_NUMBER( ) OVER(PARTITION BY 1, 2 ORDER BY c4) AS ROW_NUM1
              FROM tab1
          ) Q1
      WHERE Q1.ROW_NUM1 = 1;

MDIFF
-----

The MDIFF function calculates the moving difference for a column based on the preset query width. The query width is the specified number of rows. It is recommended that ALIAS be used in the QUALIFY statements.

**Input: MDIFF with QUALIFY**

::

   SELECT DT_A.Acct_ID, DT_A.Trade_Date, DT_A.Stat_PBU_ID
         , CAST( MDIFF( Stat_PBU_ID_3, 1, DT_A.Trade_No ASC ) AS DECIMAL(20,0) ) AS MDIFF_Stat_PBU_ID
      FROM Trade_His DT_A
     WHERE Trade_Date >= CAST( '20170101' AS DATE FORMAT 'YYYYMMDD' )
     GROUP BY DT_A.Acct_ID, DT_A.Trade_Date
    QUALIFY MDIFF_Stat_PBU_ID <> 0 OR MDIFF_Stat_PBU_ID IS NULL;

**Output**

::

   SELECT Acct_ID, Trade_Date, Stat_PBU_ID, MDIFF_Stat_PBU_ID
      FROM (SELECT DT_A.Acct_ID, DT_A.Trade_Date, DT_A.Stat_PBU_ID
            , CAST( (Stat_PBU_ID_3 - (LAG(Stat_PBU_ID_3, 1, NULL) OVER (PARTITION BY DT_A.Acct_ID, DT_A.Trade_Date ORDER BY DT_A.Trade_No ASC)))  AS MDIFF_Stat_PBU_ID
              FROM Trade_His DT_A
             WHERE Trade_Date >= CAST( '20170101' AS DATE)
                   )
    WHERE MDIFF_Stat_PBU_ID <> 0 OR MDIFF_Stat_PBU_ID IS NULL;

RANK
----

**RANK(col1, col2...)**

**Input: RANK with GROUP BY**

::

   SELECT  c1, c2, c3, RANK(c4, c1 DESC, c3) AS Rank1
     FROM  tab1
    WHERE  ...
    GROUP BY c1;

**Output**

::

   SELECT c1, c2, c3, RANK() OVER (PARTITION BY c1 ORDER BY c4, c1 DESC ,c3) AS Rank1
     FROM tab1
    WHERE ...;

ROW_NUMBER
----------

**ROW_NUMBER(col1, col2...)**

**Input: ROW NUMBER with GROUP BY + QUALIFY**

::

   SELECT c1, c2, c3, ROW_NUMBER(c4, c3)
      FROM tab1
   QUALIFY RANK(c4) = 1
     GROUP BY 1, 2;

**Output**

::

   SELECT
         c1
        ,c2
        ,c3
        ,ColumnAlias1
     FROM
         (
           SELECT
                   c1
                  ,c2
                  ,c3
                  ,ROW_NUMBER( ) over( PARTITION BY 1 ,2 ORDER BY c4 ,c3 ) AS ColumnAlias1
                  ,RANK (
                  ) over( PARTITION BY 1 ,2 ORDER BY c4 ) AS ROW_NUM1
             FROM
                 tab1
         ) Q1
    WHERE
         Q1.ROW_NUM1 = 1
   ;

COMPRESS (specified with \*****)
--------------------------------

**Input**

.. code-block::

   ORDCADBRN VARCHAR(6) CHARACTER SET LATIN CASESPECIFIC TITLE '    ' COMPRESS '******'

**Output**

.. code-block::

   ORDCADBRN VARCHAR( 6 ) /* CHARACTER SET LATIN*/ /* CASESPECIFIC*/ /*TITLE '    '*/ /* COMPRESS  '******' */
