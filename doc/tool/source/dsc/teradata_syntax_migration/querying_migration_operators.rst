:original_name: dws_mt_0084.html

.. _dws_mt_0084:

Querying Migration Operators
============================

This section contains the migration syntax for migrating Teradata query migration operators. The migration syntax determines how the keywords and features are migrated.

For details, see the following topics:

:ref:`QUALIFY <en-us_topic_0000001233922185__en-us_topic_0238518364_en-us_topic_0237362468_section10663203410406>`

:ref:`ALIAS <en-us_topic_0000001233922185__en-us_topic_0238518364_en-us_topic_0237362468_section1723919563310>`

:ref:`FORMAT and CAST <en-us_topic_0000001233922185__en-us_topic_0238518364_en-us_topic_0237362468_section18168131049>`

:ref:`Short Keys Migration <en-us_topic_0000001233922185__en-us_topic_0238518364_en-us_topic_0237362468_section75711541419>`

:ref:`Object Names Starting with $ <en-us_topic_0000001233922185__en-us_topic_0238518364_en-us_topic_0237362468_section102601577415>`

.. _en-us_topic_0000001233922185__en-us_topic_0238518364_en-us_topic_0237362468_section10663203410406:

QUALIFY
-------

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

**Output**

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

**Output**

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

**Output**

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

**Output**

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

**TITLE with QUALIFY**

**Input**

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

**Output**

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

.. _en-us_topic_0000001233922185__en-us_topic_0238518364_en-us_topic_0237362468_section1723919563310:

ALIAS
-----

**ALIAS** is supported by all databases. In Teradata, an **ALIAS** can be referred in **SELECT** and **WHERE** clauses of the same statement where the alias is defined. Since **ALIAS** is not supported in **SELECT** and **WHERE** clauses in the target, it is replaced by the defined value/expression.

.. note::

   The comparison operators LT, LE, GT, GE, EQ, and NE must not be used as TABLE alias or COLUMN alias.

   Tools support ALIAS names for columns. If the ALIAS name is same as the column name, then it should be specified for that column only and not for other columns in that table. In the following example, there is a conflict between DATA_DT column name and the DATA_DT alias. This is not supported by the tool.

   ::

      SELECT DATA_DT,DATA_INT AS DATA_DT FROM KK WHERE DATA_DT=DATE;

**Input: ALIAS**

::

   SELECT
             expression1 (
                  TITLE 'Expression 1'
             ) AS alias1
             ,CASE
                  WHEN alias1 + Cx >= z
                  THEN 1
                  ELSE 0
             END AS alias2
        FROM
             tab1
        WHERE
             alias1 = y
   ;

**Output:** **tdMigrateALIAS = FALSE**

::

   SELECT
             expression1 AS alias1
             ,CASE
                  WHEN alias1 + Cx >= z
                  THEN 1
                  ELSE 0
             END AS alias2
        FROM
             tab1
        WHERE
             alias1 = y
   ;

**Output:** **tdMigrateALIAS = TRUE**

::

   SELECT
             expression1 AS alias1
             ,CASE
                  WHEN expression1 + Cx >= z
                  THEN 1
                  ELSE 0
             END AS alias2
        FROM
             tab1
        WHERE
             expression1 = y
   ;

.. _en-us_topic_0000001233922185__en-us_topic_0238518364_en-us_topic_0237362468_section18168131049:

FORMAT and CAST
---------------

In Teradata, the **FORMAT** keyword is used for formatting a column/expression. For example, FORMAT '9(n)' and 'z(n)' are addressed using LPAD with 0 and space (' ') respectively.

Data typing is done using **CAST** or direct data type [like (expression1)(CHAR(n))]. This feature is addressed using **CAST**. For details, see :ref:`Type Casting and Formatting <dws_mt_0103>`.

**Input: FORMAT with CAST**

::

   SELECT
             CAST(TRIM( Agt_Num ) AS DECIMAL( 5 ,0 ) FORMAT '9(5)' )
        FROM
             C03_AGENT_BOND
   ;

   SELECT
             CAST(CAST( Agt_Num AS INT FORMAT 'Z(17)' ) AS CHAR( 5 ) )
        FROM
             C03_AGENT_BOND
   ;

   SELECT
             CHAR(CAST( CAST( CND_VLU AS DECIMAL( 17 ,0 ) FORMAT 'Z(17)' ) AS VARCHAR( 17 ) ) )
        FROM
             C03_AGENT_BOND
   ;

**Output**

.. code-block::

   SELECT
             LPAD( CAST( TRIM( Agt_Num ) AS DECIMAL( 5 ,0 ) ) ,5 ,'0' ) AS Agt_Num
        FROM
             C03_AGENT_BOND
   ;
   SELECT
   CAST(CAST( Agt_Num AS INT FORMAT 'Z(17)' ) AS CHAR( 5 ) )
   FROM
   C03_AGENT_BOND
   ;

   SELECT
             LENGTH( CAST( LPAD( CAST( CND_VLU AS DECIMAL( 17 ,0 ) ) ,17 ,' ' ) AS VARCHAR( 17 ) ) ) AS CND_VLU
        FROM
             C03_AGENT_BOND
   ;

**Input - FORMAT 'Z(n)9'**

::

   SELECT
             standard_price (FORMAT 'Z(5)9') (CHAR( 6 ))
             ,max_price (FORMAT 'ZZZZZ9') (CHAR( 6 ))
        FROM
             product_t
   ;

**Output**

::

   SELECT
             CAST( TO_CHAR( standard_price ,'999990' ) AS CHAR( 6 ) ) AS standard_price
             ,CAST( TO_CHAR( max_price ,'999990' ) AS CHAR( 6 ) ) AS max_price
        FROM
             product_t
   ;

**Input - FORMAT 'z(m)9.9(n)'**

::

   SELECT
             standard_price (FORMAT 'Z(6)9.9(2)') (CHAR( 6 ))
        FROM
             product_t
   ;

**Output**

::

   SELECT
             CAST( TO_CHAR( standard_price ,'9999990.00' ) AS CHAR( 6 ) ) AS standard_price
        FROM
             product_t
   ;

**Input - CAST AS INTEGER**

::

   SELECT
             CAST( standard_price AS INTEGER )
        FROM
             product_t
   ;

**Output**

::

   SELECT
            (standard_price)
        FROM
             product_t
   ;

**Input - CAST AS INTEGER FORMAT**

::

   SELECT
             CAST( price11 AS INTEGER FORMAT 'Z(4)9' ) (
                  CHAR( 10 )
             )
        FROM
             product_t
   ;

**Output**

::

   SELECT
             CAST( TO_CHAR(  ( price11 ) ,'99990' ) AS CHAR( 10 ) ) AS price11
        FROM
             product_t
   ;

.. note::

   The following Gauss function is added to convert to integer:

   ::

      CREATE OR REPLACE FUNCTION
      /*  This function is used to support "CAST AS INTEGER" of Teradata.
          It should be created in the "mig_td_ext" schema.
      */
           ( i_param                            TEXT )
      RETURN INTEGER
      AS
        v_castasint    INTEGER;
      BEGIN

         v_castasint := CASE WHEN i_param IS NULL
                                                      THEN NULL         -- if NULL value is provided as input
                                                                      WHEN TRIM(i_param) IS NULL
                                                                                      THEN 0                  -- if empty string with one or more spaces is provided
                                                                      ELSE TRUNC(CAST(i_param AS NUMBER))            -- if any numeric value is provided
                                      END;

      RETURN v_castasint;
      END;

.. _en-us_topic_0000001233922185__en-us_topic_0238518364_en-us_topic_0237362468_section75711541419:

Short Keys Migration
--------------------

:ref:`Table 1 <en-us_topic_0000001233922185__en-us_topic_0238518364_en-us_topic_0237362468_table39390038143532>` lists the Teradata short keys supported by GaussDB(DWS) and their equivalent syntax in GaussDB(DWS).

.. _en-us_topic_0000001233922185__en-us_topic_0238518364_en-us_topic_0237362468_table39390038143532:

.. table:: **Table 1** List of short keys

   ================== =================================
   Teradata Short Key Equivalent Syntax in GaussDB(DWS)
   ================== =================================
   SEL                SELECT
   INS                INSERT
   UPD                UPDATE
   DEL                DELETE
   CT                 CREATE TABLE
   CV                 CREATE VIEW
   BT                 START TRANSACTION
   ET                 COMMIT
   ================== =================================

**Input - BT**

.. code-block::

   BT;
   --
   delete from ${BRTL_DCOR}.BRTL_CS_CUST_CID_UID_REL
   where DW_Job_Seq = ${v_Group_No};

   .if ERRORCODE <> 0 then .quit 12;

   --
   insert into ${BRTL_DCOR}.BRTL_CS_CUST_CID_UID_REL
   (
      Cust_Id
     ,Cust_UID
     ,DW_Upd_Dt
     ,DW_Upd_Tm
     ,DW_Job_Seq
     ,DW_Etl_Dt
   )
   select
      a.Cust_Id
     ,a.Cust_UID
     ,current_date as Dw_Upd_Dt
     ,current_time(0) as DW_Upd_Tm
     ,cast(${v_Group_No} as byteint) as DW_Job_Seq
     ,cast('${v_Trx_Dt}' as date format 'yyyy-mm-dd') as DW_Etl_Dt
   from ${BRTL_VCOR}.BRTL_CS_CUST_CID_UID_REL_S a
   where a.DW_Snsh_Dt = cast('${v_Trx_Dt}' as date format 'yyyy-mm-dd');

   .if ERRORCODE <> 0 then .quit 12;

   ET;cd ..

**Output**

.. code-block::

   BEGIN
   --
      BEGIN
         delete from ${BRTL_DCOR}.BRTL_CS_CUST_CID_UID_REL
          where DW_Job_Seq = ${v_Group_No};
            lv_mig_errorcode = 0;
      EXCEPTION
         WHEN OTHERS THEN
            lv_mig_errorcode = -1;
      END;

      IF lv_mig_errorcode <> 0 THEN
           RAISE EXCEPTION '12';
      END IF;

   --
      BEGIN
           insert into ${BRTL_DCOR}.BRTL_CS_CUST_CID_UID_REL
           (
             Cust_Id
            ,Cust_UID
            ,DW_Upd_Dt
            ,DW_Upd_Tm
            ,DW_Job_Seq
            ,DW_Etl_Dt
          )
         select
             a.Cust_Id
            ,a.Cust_UID
            ,current_date as Dw_Upd_Dt
            ,current_time(0) as DW_Upd_Tm
            ,cast(${v_Group_No} as byteint) as DW_Job_Seq
            ,cast('${v_Trx_Dt}' as date format 'yyyy-mm-dd') as DW_Etl_Dt
          from ${BRTL_VCOR}.BRTL_CS_CUST_CID_UID_REL_S a
        where a.DW_Snsh_Dt = cast('${v_Trx_Dt}' as date format 'yyyy-mm-dd');
      EXCEPTION
         WHEN OTHERS THEN
            lv_mig_errorcode = -1;
      END;

      IF lv_mig_errorcode <> 0 THEN
           RAISE EXCEPTION '12';
      END IF;

   END;

.. _en-us_topic_0000001233922185__en-us_topic_0238518364_en-us_topic_0237362468_section102601577415:

Object Names Starting with $
----------------------------

This section describes the migration of object names starting with $.

The migration behavior for object names starting with $ is explained in the following table. Use the :ref:`tdMigrateDollar <en-us_topic_0000001233922159__en-us_topic_0218440346_li4899115763212>` configuration parameter to configure the behavior.

For details, see: :ref:`IN / NOT IN conversion <en-us_topic_0000001234200637__en-us_topic_0238518365_en-us_topic_0237362248_section102601577415>`

.. table:: **Table 2** Migration of object names starting with $

   +-------------------------+-------------------------------------+---------------------------------------------------------+
   | tdMigrateDollar Setting | Object Name                         | Migrated to                                             |
   +=========================+=====================================+=========================================================+
   | true                    | $V_SQL                              | "$V_SQL"                                                |
   |                         |                                     |                                                         |
   |                         | Static object name starting with $  |                                                         |
   +-------------------------+-------------------------------------+---------------------------------------------------------+
   | true                    | ${V_SQL}                            | ${V_SQL}                                                |
   |                         |                                     |                                                         |
   |                         | Dynamic object name starting with $ | No change: Dynamic object names not supported           |
   +-------------------------+-------------------------------------+---------------------------------------------------------+
   | false                   | $V_SQL                              | $V_SQL                                                  |
   |                         |                                     |                                                         |
   |                         | Static object name starting with $  | No change: Configuration parameter is set to **false**. |
   +-------------------------+-------------------------------------+---------------------------------------------------------+
   | false                   | ${V_SQL}                            | ${V_SQL}                                                |
   |                         |                                     |                                                         |
   |                         | Dynamic object name starting with $ | No change: Configuration parameter is set to **false**. |
   +-------------------------+-------------------------------------+---------------------------------------------------------+

.. note::

   Any variable starting with $ is considered as a Value. The tool will migrate this by adding ARRAY.

**Input - OBJECT STARTING WITH $**

::

   SELECT $C1 from p11 where $C1 NOT LIKE ANY ($sql1);

**Output** (tdMigrateDollar to TRUE)

::

   SELECT
           "$C1"
   FROM
           p11
   WHERE
           "$C1" NOT LIKE ANY (
           ARRAY[ "$sql1" ]
           )
   ;

**Output** (tdMigrateDollar to FALSE)

::

   SELECT
           $C1
   FROM
           p11
   WHERE
           $C1 NOT LIKE ANY (
           ARRAY[ $sql1 ]
           )
   ;

**Input - Value starting with $ in LIKEALL/LIKE ANY**

::

   SELECT * FROM T1
   WHERE T1.Event_Dt>=ADD_MONTHS(CAST('${OUT_DATE}' AS DATE FORMAT 'YYYYMMDD')+1,(-1)*CAST(T7.Tm_Range_Month AS INTEGER))
      AND T1.Event_Dt<=CAST('${OUT_DATE}' AS DATE FORMAT 'YYYYMMDD')
      AND T1.Cntpty_Acct_Name NOT LIKE ALL ( SELECT Tx_Cntpty_Name_Key FROM TEMP_NAME )
      AND T1.Cntpty_Acct_Name NOT LIKE ANY ( SELECT Tx_Cntpty_Name_Key FROM TEMP_NAME )
      AND T1.Cntpty_Acct_Name LIKE ALL ( SELECT Tx_Cntpty_Name_Key FROM TEMP_NAME )
      AND T1.Cntpty_Acct_Name LIKE ANY ( SELECT Tx_Cntpty_Name_Key FROM TEMP_NAME )
      AND T1.Col1 NOT LIKE ANY ($sql1)
      AND T1.Col1 NOT LIKE ALL ($sql1)
      AND T1.Col1 LIKE ANY ($sql1)
      AND T1.Col1 LIKE ALL ($sql1);

**Output**

::

   SELECT
             *
        FROM
             T1
        WHERE
             T1.Event_Dt >= ADD_MONTHS (CAST( '${OUT_DATE}' AS DATE ) + 1 ,(- 1 ) * CAST( T7.Tm_Range_Month AS INTEGER ))
             AND T1.Event_Dt <= CAST( '${OUT_DATE}' AS DATE )
             AND T1.Cntpty_Acct_Name NOT LIKE ALL (
                  SELECT
                            Tx_Cntpty_Name_Key
                       FROM
                            TEMP_NAME
             )
             AND T1.Cntpty_Acct_Name NOT LIKE ANY (
                  SELECT
                            Tx_Cntpty_Name_Key
                       FROM
                            TEMP_NAME
             )
             AND T1.Cntpty_Acct_Name LIKE ALL (
                  SELECT
                            Tx_Cntpty_Name_Key
                       FROM
                            TEMP_NAME
             )
             AND T1.Cntpty_Acct_Name LIKE ANY (
                  SELECT
                            Tx_Cntpty_Name_Key
                       FROM
                            TEMP_NAME
             )
             AND T1.Col1 NOT LIKE ANY (
                  ARRAY[ "$sql1" ]
             )
             AND T1.Col1 NOT LIKE ALL (
                  ARRAY[ "$sql1" ]
             )
             AND T1.Col1 LIKE ANY (
                  ARRAY[ "$sql1" ]
             )
             AND T1.Col1 LIKE ALL (
                  ARRAY[ "$sql1" ]
             )
   ;

**QUALIFY, CASE, and ORDER BY**

**Input**

.. code-block::

   select
      a.Cust_UID as Cust_UID          /*   UID */
     ,a.Rtl_Usr_Id as Ini_CM          /*        */
     ,a.Cntr_Aprv_Dt as Aprv_Pass_Tm          /*        */
     ,a.Blg_Org_Id as CM_BRN_Nbr          /*         */
     ,a.Mng_Chg_Typ_Cd  as MNG_CHG_TYP_CD          /*          */
     ,case when a.Blg_Org_Id = b.BRN_Org_Id and a.Mng_Chg_Typ_Cd= 'PMD' and a.Pst_Id in ('PB0101','PB0104') then 'Y' ----        ,
          when a.Blg_Org_Id = b.BRN_Org_Id and a.Mng_Chg_Typ_Cd= 'DEVPMD' and a.Pst_Id ='PB0106'  then 'Y' ----
          when a.Blg_Org_Id = b.BRN_Org_Id and a.Mng_Chg_Typ_Cd= 'DMD' and a.Pst_Id in ('PB0201','PB0204') then 'Y' ----        ,
         when a.Blg_Org_Id = b.BRN_Org_Id and a.Mng_Chg_Typ_Cd= 'DEVDMD' and a.Pst_Id ='PB0109'  then 'Y' ----            ,
          else ''
   end  as Pst_Flg          /*      */
     ,a.Pst_Id as Pst_Id          /*      */
     ,a.BBK_Org_Id  as BBK_Org_Id          /*        */
   from VT_CUID_MND_NMN_CHG_INF as a          /* VT_         */
   LEFT OUTER JOIN ${BRTL_VCOR}.BRTL_EM_USR_PST_REL_INF_S as b          /* EM_           */
     on  a.Rtl_Usr_Id = b.Rtl_Usr_Id
     AND a.Blg_Org_Id  = b.BRN_Org_Id
     AND a.Pst_Id = b.Pst_Id
     AND b.Sys_Id = 'privatebanking'
     AND b.pst_sts IN ('1','0','-2') /*     1   -2   0  */
     AND b.DW_Snsh_Dt =  cast('${v_Trx_Dt}' as date format 'yyyy-mm-dd')
   qualify   row_number() over(partition by a.Cust_UID,a.bbk_org_id order by
   case when ( a.Mng_Chg_Typ_Cd= 'PMD'    and   a.Pst_Id in ('PB0101','PB0104')) or ( a.Mng_Chg_Typ_Cd= 'DEVPMD'    and   a.Pst_Id ='PB0106')
   then 0 when  (a.Mng_Chg_Typ_Cd= 'DMD' and a.Pst_Id in ('PB0201','PB0204')) or    (a.Mng_Chg_Typ_Cd= 'DEVDMD' and a.Pst_Id ='PB0109 ')   then 0 else 1 end  asc )  = 1
   ;

**Output**

.. code-block::

    SELECT
                        Cust_UID AS Cust_UID /*   UID */
                        ,Ini_CM /*        */
                        ,Aprv_Pass_Tm /*        */
                        ,CM_BRN_Nbr /*         */
                        ,MNG_CHG_TYP_CD /*          */
                        ,Pst_Flg /*      */
                        ,Pst_Id AS Pst_Id /*      */
                        ,BBK_Org_Id AS BBK_Org_Id /*        */
                   FROM
                        ( SELECT
                             a.Cust_UID AS Cust_UID /*   UID */
                             ,a.Rtl_Usr_Id AS Ini_CM /*        */
                             ,a.Cntr_Aprv_Dt AS Aprv_Pass_Tm /*        */
                             ,a.Blg_Org_Id AS CM_BRN_Nbr /*         */
                             ,a.Mng_Chg_Typ_Cd AS MNG_CHG_TYP_CD /*          */
                             ,CASE WHEN a.Blg_Org_Id = b.BRN_Org_Id AND a.Mng_Chg_Typ_Cd = 'PMD' AND a.Pst_Id IN ( 'PB0101' ,'PB0104' )
                                       THEN 'Y' /*         , */
                                  WHEN a.Blg_Org_Id = b.BRN_Org_Id AND a.Mng_Chg_Typ_Cd = 'DEVPMD' AND a.Pst_Id = 'PB0106'
                                       THEN 'Y' /*  */
                                  WHEN a.Blg_Org_Id = b.BRN_Org_Id AND a.Mng_Chg_Typ_Cd = 'DMD' AND a.Pst_Id IN ( 'PB0201' ,'PB0204' )
                                       THEN 'Y' /*         , */
                                  WHEN a.Blg_Org_Id = b.BRN_Org_Id AND a.Mng_Chg_Typ_Cd = 'DEVDMD' AND a.Pst_Id = 'PB0109'
                                       THEN 'Y' /*             , */
                             ELSE
                                  ''
                             END AS Pst_Flg /*      */
                             ,a.Pst_Id AS Pst_Id /*      */
                             ,a.BBK_Org_Id AS BBK_Org_Id /*        */
                             ,row_number( ) over( partition BY a.Cust_UID
                             ,a.bbk_org_id
                        ORDER BY
                             CASE WHEN( a.Mng_Chg_Typ_Cd = 'PMD' AND Q1.Pst_Id IN ( 'PB0101' ,'PB0104' ) ) OR( Q1.Mng_Chg_Typ_Cd = 'DEVPMD' AND a.Pst_Id = 'PB0106' )
                                       THEN 0
                                  WHEN( a.Mng_Chg_Typ_Cd = 'DMD' AND Q1.Pst_Id IN ( 'PB0201' ,'PB0204' ) ) OR( Q1.Mng_Chg_Typ_Cd = 'DEVDMD' AND a.Pst_Id = 'PB0109 ' )
                                       THEN 0
                             ELSE
                                  1
                             END ASC ) AS ROW_NUM1
                        FROM
                             VT_CUID_MND_NMN_CHG_INF AS a /* VT_         */
                             LEFT OUTER JOIN BRTL_VCOR.BRTL_EM_USR_PST_REL_INF_S AS b /* EM_           */
                                  ON a.Rtl_Usr_Id = b.Rtl_Usr_Id
                             AND a.Blg_Org_Id = b.BRN_Org_Id
                             AND a.Pst_Id = b.Pst_Id
                             AND b.Sys_Id = 'privatebanking'
                             AND b.pst_sts IN ( '1' ,'0' ,'-2' ) /*     1   -2   0  */
                             AND b.DW_Snsh_Dt = CAST( lv_mig_v_Trx_Dt AS DATE ) ) Q1
                        WHERE
                             Q1.ROW_NUM1 = 1 ;
