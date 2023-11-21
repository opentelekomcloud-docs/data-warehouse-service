:original_name: dws_mt_0094.html

.. _dws_mt_0094:

System Functions and Operators
==============================

This section contains the migration syntax for migrating Teradata system functions and operators. The migration syntax determines how the keywords and features are migrated.

Schema
------

The database with the schema name should be changed to **SET SESSION CURRENT_SCHEMA**.

+-----------------------------------+------------------------------------------+
| Oracle Syntax                     | Syntax After Migration                   |
+===================================+==========================================+
| .. code-block::                   | .. code-block::                          |
|                                   |                                          |
|    DATABASE SCHTERA               |    SET SESSION CURRENT_SCHEMA TO SCHTERA |
+-----------------------------------+------------------------------------------+

Analytical Functions
--------------------

Analytical functions are collectively called ordered analytical functions in Teradata, and they provide powerful analytical abilities for data mining, analysis and business intelligence.

#. **Analytical Functions in ORDER BY**

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

#. **Analytical Functions in PARTITION BY**

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

#. **Window Functions**

   Window functions perform calculations across rows of the query result. DSC supports the following Teradata window functions:

   .. note::

      The DSC supports only single occurrance of window function in QUALIFY clause. Multiple window functions in a QUALIFY may result in invalid migration.

#. **CSUM**

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

#. **MDIFF**

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

#. **RANK**

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

#. **ROW_NUMBER**

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

#. **COMPRESS specified with \****\***

   **Input**

   .. code-block::

      ORDCADBRN VARCHAR(6) CHARACTER SET LATIN CASESPECIFIC TITLE '    ' COMPRESS '******'

   **Output**

   .. code-block::

      ORDCADBRN VARCHAR( 6 ) /* CHARACTER SET LATIN*/ /* CASESPECIFIC*/ /*TITLE '    '*/ /* COMPRESS  '******' */

Comparison and List Operators
-----------------------------

.. note::

   The comparison operators LT, LE, GT, GE, EQ, and NE must not be used as TABLE alias or COLUMN alias.

The following comparison and list operators are supported:

#. **^= and GT**

   Input: Comparison operations (^= and GT)

   ::

      SELECT t1.c1, t2.c2
        FROM tab1 t1, tab2 t2
       WHERE t1.c3 ^= t1.c3
         AND t2.c4 GT 100;

   Output

   ::

      SELECT t1.c1, t2.c2
        FROM tab1 t1, tab2 t2
       WHERE t1.c3 <> t1.c3
         AND t2.c4 > 100;

#. **EQ and NE**

   Input: Comparison operations (EQ and NE)

   ::

      SELECT t1.c1, t2.c2
        FROM tab1 t1 INNER JOIN tab2 t2
          ON t1.c2 EQ t2.c2
       WHERE t1.c6 NE 1000;

   Output

   ::

       SELECT t1.c1, t2.c2
        FROM tab1 t1 INNER JOIN tab2 t2
          ON t1.c2 = t2.c2
       WHERE
              t1.c6 <> 1000;

#. **LE and GE**

   Input: Comparison operations (LE and GE)

   ::

      SELECT t1.c1, t2.c2
        FROM tab1 t1, tab2 t2
       WHERE t1.c3 LE 200
         AND t2.c4 GE 100;

   Output

   ::

       SELECT t1.c1, t2.c2
         FROM tab1 t1, tab2 t2
        WHERE t1.c3 <= 200
          AND t2.c4 >= 100;

#. **NOT= and LT**

   Input: Comparison operations (NOT= and LT)

   ::

      SELECT t1.c1, t2.c2
        FROM tab1 t1, tab2 t2
       WHERE t1.c3 NOT= t1.c3
         AND t2.c4 LT 100;

   Output

   ::

      SELECT t1.c1, t2.c2
        FROM tab1 t1, tab2 t2
       WHERE t1.c3 <> t1.c3
         AND t2.c4 < 100;

#. **IN and NOT IN**

   For details, see :ref:`IN and NOT IN Conversion <en-us_topic_0000001234200637__en-us_topic_0238518365_en-us_topic_0237362248_section102601577415>`.

   Input: IN and NOT IN

   ::

       SELECT c1, c2
         FROM tab1
        WHERE c1 IN 'XY';

   Output

   ::

      SELECT c1, c2
        FROM tab1
       WHERE c1 = 'XY';

   .. note::

      GaussDB(DWS) does not support **IN** and **NOT IN** operators in some specific scenarios.

#. **IS NOT IN**

   Input: IS NOT IN

   ::

      SELECT c1, c2
        FROM tab1
       WHERE c1 IS NOT IN (subquery);

   Output

   ::

      SELECT c1, c2
        FROM tab1
       WHERE c1 NOT IN (subquery);

#. **LIKE ALL / NOT LIKE ALL**

   Input: LIKE ALL / NOT LIKE ALL

   ::

      SELECT c1, c2
        FROM tab1
       WHERE c3 NOT LIKE ALL ('%STR1%', '%STR2%', '%STR3%');

   Output

   ::

      SELECT c1, c2
        FROM tab1
       WHERE c3 NOT LIKE ALL (ARRAY[ '%STR1%', '%STR2%', '%STR3%' ]);

#. **LIKE ANY / NOT LIKE ANY**

   Input: LIKE ANY / NOT LIKE ANY

   ::

      SELECT c1, c2
        FROM tab1
       WHERE c3 LIKE ANY ('STR1%', 'STR2%', 'STR3%');

   Output

   ::

      SELECT c1, c2
        FROM tab1
       WHERE c3 LIKE ANY (ARRAY[ 'STR1%', 'STR2%', 'STR3%' ]);

Table Operators
---------------

The functions that can be called in the FROM clause of a query are from the table operator.

**Input: Table operator with RETURNS**

::

   SELECT *
     FROM TABLE( sales_retrieve (9005) RETURNS ( store INTEGER, item CLOB, quantity BYTEINT) ) AS ret;

**Output**

::

   SELECT *
     FROM sales_retrieve(9005) AS ret (store, item, quantity);
