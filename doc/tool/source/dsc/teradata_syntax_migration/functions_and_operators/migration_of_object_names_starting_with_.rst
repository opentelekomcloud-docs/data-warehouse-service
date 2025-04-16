:original_name: dws_16_0055.html

.. _dws_16_0055:

.. _en-us_topic_0000001813439124:

Migration of Object Names Starting with $
=========================================

This section describes the migration of object names starting with $.

The migration behavior for object names starting with $ is explained in the following table. Use the :ref:`tdMigrateDollar <en-us_topic_0000001813438796__en-us_topic_0000001432527901_li9311162317910>` configuration parameter to configure the behavior.

For details, see **IN and NOT IN Conversion**.

.. table:: **Table 1** Migration of object names starting with $

   +-------------------------+-------------------------------------+---------------------------------------------------------+
   | tdMigrateDollar Setting | Object Name                         | Migrated To                                             |
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

**Output** (**tdMigrateDollar = TRUE**)

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

**Output** (**tdMigrateDollar = FALSE)**

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

**Output:**

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

QUALIFY, CASE, and ORDER BY
---------------------------

**Input:**

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

**Output:**

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
