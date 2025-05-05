:original_name: dws_16_0051.html

.. _dws_16_0051:

.. _en-us_topic_0000001860318953:

QUALIFY
=======

In general, the **QUALIFY** clause is accompanied by analytic functions (window functions) such as CSUM(), MDIFF(), ROW_NUMBER() and RANK(). This is addressed using sub-query that contains the window functions specified in the **QUALIFY** clause. Migration tools support **QUALIFY** with **MDIFF()**, **RANK()** and **ROW_NUMBER()**. **QUALIFY** is a Teradata extension and not an ANSI standard syntax. It is executed after the WHERE and GROUP BY clauses. QUALIFY must start in new line.

.. note::

   Migration tools support column name and/or expressions in the ORDER BY clause only if the column name and/or expression is explicitly included in the SELECT statement as well.

**Input: QUALIFY**

::

   SELECT
           CUSTOMER_ID
          ,CUSTOMER_NAME
     FROM
          CUSTOMER_T QUALIFY row_number( ) Over( partition BY CUSTOMER_ID ORDER BY POSTAL_CODE DESC ) = 1
   ;

**Output:**

::

   SELECT
             CUSTOMER_ID
             ,CUSTOMER_NAME
        FROM
             (
                  SELECT
                            CUSTOMER_ID
                            ,CUSTOMER_NAME
                            ,row_number( ) Over( partition BY CUSTOMER_ID ORDER BY POSTAL_CODE DESC ) AS ROW_NUM1
                       FROM
                            CUSTOMER_T
             ) Q1
        WHERE
             Q1.ROW_NUM1 = 1
   ;

**Input: QUALIFY** **with MDIFF and RANK**

::

   SELECT
              material_name
             ,unit_of_measure * standard_cost AS tot_cost
        FROM
             raw_material_t m LEFT JOIN supplies_t s
                  ON s.material_id = m.material_id
             QUALIFY rank ( ) over( ORDER BY tot_cost DESC ) IN '5'
                     OR mdiff( tot_cost ,3 ,material_name ) IS NULL
   ;

**Output:**

::

   SELECT
             material_name
             ,tot_cost
        FROM
             (
                  SELECT
                            material_name
                            ,unit_of_measure * standard_cost AS tot_cost
                            ,rank ( ) over( ORDER BY unit_of_measure * standard_cost DESC ) AS ROW_NUM1
                            ,unit_of_measure * standard_cost - (LAG( unit_of_measure * standard_cost ,3 ,NULL ) over( ORDER BY material_name )) AS ROW_NUM2
                       FROM
                            raw_material_t m LEFT JOIN supplies_t s
                                 ON s.material_id = m.material_id
             ) Q1
        WHERE
             Q1.ROW_NUM1 = '5'
             OR Q1.ROW_NUM2 IS NULL
   ;

**Input: QUALIFY with ORDER BY having columns that do not exist in the SELECT list**

::

   SELECT Postal_Code
      FROM db_pvfc9_std.Customer_t t1
      GROUP BY Customer_Name ,Postal_Code
      QUALIFY ---comments
    (  Rank ( CHAR(Customer_Address) DESC  )  ) = 1
     ORDER BY t1.Customer_Name;

**Output:**

::

   SELECT Postal_Code FROM
              ( SELECT Customer_Name, Postal_Code
       , Rank () over( PARTITION BY Customer_Name, Postal_Code ORDER BY LENGTH(Customer_Address) DESC ) AS Rank_col
                  FROM db_pvfc9_std.Customer_t t1
              ) Q1
         WHERE /*comments*/
       Q1.Rank_col = 1
     ORDER BY Q1.Customer_Name;

**Input: QUALIFY with COLUMN ALIAS - the corresponding column expression should not be added again in SELECT list.**

::

   SELECT material_name, unit_of_measure * standard_cost as tot_cost,
           RANK() over(order by tot_cost desc) vendor_cnt
    FROM raw_material_t m left join supplies_t s
    ON s.material_id = m.material_id
    QUALIFY vendor_cnt < 5 or MDIFF(tot_cost, 3, material_name) IS NULL;

**Output:**

::

   SELECT material_name, tot_cost, vendor_cnt
      FROM  ( SELECT material_name
                          , unit_of_measure * standard_cost AS tot_cost
                         , rank () over (ORDER BY tot_cost DESC) vendor_cnt
                         , tot_cost - ( LAG(tot_cost ,3 ,NULL) over (ORDER BY material_name) ) AS anltfn
                  FROM raw_material_t m LEFT JOIN supplies_t s
                     ON s.material_id = m.material_id
              ) Q1
         WHERE  Q1.vendor_cnt < 5 OR Q1.anltfn IS NULL
    ;

TITLE with QUALIFY
------------------

**Input:**

.. code-block::

   REPLACE VIEW ${STG_VIEW}.LP06_BMCLIINFP${v_Table_Suffix_Inc}
   (
     CLICLINBR
   , CLICHNNAM
   , CLICHNSHO
   , CLICLIMNE
   , CLIBNKCOD
   )
   AS
   LOCKING ${STG_DATA}.LP06_BMCLIINFP${v_Table_Suffix_Inc} FOR ACCESS
   SELECT
     CLICLINBR (title '    VARCHAR(20)')
   , CLICHNNAM (title '        VARCHAR(200)')
   , CLICHNSHO (title '        VARCHAR(20)')
   , CLICLIMNE (title '      VARCHAR(10)')
   , CLIBNKCOD (title '       VARCHAR(11)')
   FROM
     ${STG_DATA}.LP06_BMCLIINFP${v_Table_Suffix_Inc} s1
   QUALIFY
       ROW_NUMBER() OVER(PARTITION BY  CLICLINBR ORDER BY CLICLINBR  ) = 1
   ;

**Output:**

.. code-block::

   CREATE OR REPLACE VIEW ${STG_VIEW}.LP06_BMCLIINFP${v_Table_Suffix_Inc}
   (
     CLICLINBR
   , CLICHNNAM
   , CLICHNSHO
   , CLICLIMNE
   , CLIBNKCOD
   )
   AS
   /* LOCKING ${STG_DATA}.LP06_BMCLIINFP${v_Table_Suffix_Inc} FOR ACCESS */
   SELECT CLICLINBR
           , CLICHNNAM
          , CLICHNSHO
          , CLICLIMNE
         , CLIBNKCOD
    FROM (
              SELECT
                             CLICLINBR /* (title '    VARCHAR(20)') */
                           , CLICHNNAM /* (title '        VARCHAR(200)') */
                          , CLICHNSHO /* (title '        VARCHAR(20)') */
                          , CLICLIMNE /* (title '      VARCHAR(10)') */
                          , CLIBNKCOD /* (title '       VARCHAR(11)') */
                          , ROW_NUMBER() OVER(PARTITION BY  CLICLINBR ORDER BY CLICLINBR  ) AS ROWNUM1
   FROM
     ${STG_DATA}.LP06_BMCLIINFP${v_Table_Suffix_Inc} s1 ) Q1
   WHERE Q1.ROWNUM1 = 1
   ;
