:original_name: dws_07_8121.html

.. _dws_07_8121:

DSQL
====

DSQL Variable: With AS
----------------------

**Input**

.. code-block::

   /* VARIABLE INPUT */
   select cast(cast('${TX_DATE}' as date format 'yyyymmdd') as date format 'yyyy-mm-dd') as v_Trx_Dt;
   .IF ERRORCODE <> 0 THEN .QUIT 12 ;

   insert into VT_RTL_ACT_ORG_INF          /* VT_         */
   (
      Act_Id          /*      */
     ,Act_Mdf          /*       */
     ,Act_Agr_Typ_Cd          /*          */
     ,Opn_BBK          /*      */
     ,Opn_BRN          /*      */
   )
   select
      a.Agr_Id as Act_Id          /*      */
     ,a.Agr_Mdf as Act_Mdf          /*       */
     ,a.Agr_Typ_Cd as Act_Agr_Typ_Cd          /*          */
     ,a.BBK_Org_Id as Opn_BBK          /*      */
     ,a.Opn_Org_Id as Opn_BRN          /*      */
   from ${PDM_VIEW}.T03_AGR_TRSR_BND_ACT as a          /*   _     */
   where  a.DW_Start_Dt <= cast('${v_Trx_Dt}' as date format 'yyyy-mm-dd')
              AND a.DW_End_Dt > cast('${v_Trx_Dt}' as date format 'yyyy-mm-dd')
              AND a.Agr_Typ_Cd IN ('20301')          /*      */
   ;
   .IF ERRORCODE <> 0 THEN .QUIT 12 ;

**Output**

.. code-block::

   DECLARE lv_mig_obj_exists_check    NUMBER ( 1 ) ;
    lv_mig_errorcode           NUMBER ( 4 ) ;
    lv_mig_v_Trx_Dt            TEXT ;

   BEGIN
         /*VARIABLE INPUT */
        BEGIN
             SELECT CAST( CAST( '${TX_DATE}' AS DATE ) AS DATE )
        INTO lv_mig_v_Trx_Dt ;

      lv_mig_errorcode := 0 ;

        EXCEPTION
             WHEN OTHERS THEN
                 lv_mig_errorcode := -1;

        END;
        IF lv_mig_errorcode <> 0 THEN
             RAISE EXCEPTION '12' ;
        END IF ;

        BEGIN
             INSERT INTO VT_RTL_ACT_ORG_INF /* VT_         */
             (
                  Act_Id /*      */
                  ,Act_Mdf /*       */
                  ,Act_Agr_Typ_Cd /*          */
                  ,Opn_BBK /*      */
                  ,Opn_BRN /*      */
             )
             SELECT     a.Agr_Id AS Act_Id /*      */
                       ,a.Agr_Mdf AS Act_Mdf /*       */
                       ,a.Agr_Typ_Cd AS Act_Agr_Typ_Cd /*          */
                       ,a.BBK_Org_Id AS Opn_BBK /*      */
                       ,a.Opn_Org_Id AS Opn_BRN /*      */
                  FROM ${PDM_VIEW}.T03_AGR_TRSR_BND_ACT AS a /*   _     */
                 WHERE a.DW_Start_Dt <= CAST( lv_mig_v_Trx_Dt AS DATE )
                       AND a.DW_End_Dt > CAST( lv_mig_v_Trx_Dt AS DATE )
                       AND a.Agr_Typ_Cd IN ( '20301' ) /*      */
                       ;
                       lv_mig_errorcode := 0 ;

        EXCEPTION
             WHEN OTHERS THEN
                 lv_mig_errorcode := -1;

        END ;
        IF lv_mig_errorcode <> 0 THEN
             RAISE EXCEPTION '12' ;
        END IF ;

   END ;
   /

DSQL Variable: Without AS
-------------------------

**Input**

.. code-block::

   /* SESSION INPUT */
   select Session ;
   .IF ERRORCODE <> 0 THEN .QUIT 12 ;

   select username,clientsystemuserid,clientipaddress,clientprogramname
     from dbc.sessioninfoV
    where sessionno = '$Session' ;
   .IF ERRORCODE <> 0 THEN .QUIT 12 ;

**Output**

.. code-block::

   DECLARE lv_mig_obj_exists_check    NUMBER ( 1 ) ;
    lv_mig_errorcode           NUMBER ( 4 ) ;
    lv_mig_Session             TEXT ;

   BEGIN
       /*SESSION INPUT */
        BEGIN
             SELECT pg_backend_pid ( )
        INTO lv_mig_Session ;

      lv_mig_errorcode := 0 ;

        EXCEPTION
            WHEN OTHERS THEN
               lv_mig_errorcode := - 1 ;
        END ;

        IF lv_mig_errorcode <> 0 THEN
             RAISE EXCEPTION '12' ;
        END IF ;

        BEGIN
             SELECT COUNT( * ) INTO lv_mig_obj_exists_check
               FROM ( SELECT username
                            ,clientsystemuserid
                            ,clientipaddress
                            ,clientprogramname
                       FROM dbc.sessioninfoV
                      WHERE sessionno = lv_mig_Session )
        LIMIT 1 ;

        lv_mig_errorcode := 0 ;

        EXCEPTION
            WHEN OTHERS THEN
                lv_mig_errorcode := - 1 ;
        END ;

        IF lv_mig_errorcode <> 0 THEN
             RAISE EXCEPTION '12' ;
        END IF ;

   END ;
   /

Specifying Variables in BTEQ Statements
---------------------------------------

**Input**

.. code-block::

   select case when cast('${v_Trx_Dt}' as date format'yyyy-mm-dd') = cast('${v_Tx_Mon_End_Date}' as date format'yyyy-mm-dd')         then '0'         else '1' end  as v_IsLastDay;
   .IF ERRORCODE <> 0 THEN .QUIT 12 ;
   insert into VT_AS_CUST_WLTH_GRP          /* VT_       */
   (
      Cust_UID          /*   UID */
     ,BBK_Org_Id          /*        */
     ,Wlth_grp_Cd          /*        */
     ,Card_Grd_Cd          /*      */
   )
   select
      a.Cust_UID as Cust_UID          /*   UID */
     ,a.BBK_Org_Id as BBK_Org_Id          /*        */
     ,'11' as Wlth_grp_Cd          /*        */
     ,coalesce(b.Card_Grd_Cd,'') as Card_Grd_Cd          /*      */
   from VT_CUST_WLTH_GRP_LAST as a          /* VT_           */
   LEFT OUTER JOIN ${BRTL_VEXT}.BRTL_AS_CUST_CARD_GRD_S as b          /* AS_         */
     on  b.Cust_Uid = a.Cust_Uid
     AND b.BBK_Org_Id = a.BBK_Org_Id
     AND b.Card_Sts_Scp_Cd = '001'          /* 001-         */
     AND b.Card_Grd_Cd IN ('080','060','040','020','010','000')
     AND b.DW_Snsh_Dt = cast('${v_Trx_Dt}' as date format'yyyy-mm-dd')
   where  b.Cust_UID IS null
   ;
   .IF v_IsLastDay = 1 THEN .GOTO doit

**Output**

.. code-block::

   BEGIN
       select case when cast('${v_Trx_Dt}' as date format'yyyy-mm-dd') = cast('${v_Tx_Mon_End_Date}' as date format'yyyy-mm-dd')     then '0'         else '1' end  INTO lv_mig_v_IsLastDay;
         lv_mig_ERRORCODE = 0;
   EXCEPTION
       WHEN OTHERS THEN
           lv_mig_ERRORCODE = -1;
   END;
   IF lv_mig_ERRORCODE <> 0 THEN
     RAISE EXCEPTION '12';
   END IF;

   insert into VT_AS_CUST_WLTH_GRP          /* VT_       */
   (
      Cust_UID          /*   UID */
     ,BBK_Org_Id          /*        */
     ,Wlth_grp_Cd          /*        */
     ,Card_Grd_Cd          /*      */
   )
   select
      a.Cust_UID as Cust_UID          /*   UID */
     ,a.BBK_Org_Id as BBK_Org_Id          /*        */
     ,'11' as Wlth_grp_Cd          /*        */
     ,coalesce(b.Card_Grd_Cd,'') as Card_Grd_Cd          /*      */
   from VT_CUST_WLTH_GRP_LAST as a          /* VT_           */
   LEFT OUTER JOIN ${BRTL_VEXT}.BRTL_AS_CUST_CARD_GRD_S as b          /* AS_         */
     on  b.Cust_Uid = a.Cust_Uid
     AND b.BBK_Org_Id = a.BBK_Org_Id
     AND b.Card_Sts_Scp_Cd = '001'          /* 001-         */
     AND b.Card_Grd_Cd IN ('080','060','040','020','010','000')
     AND b.DW_Snsh_Dt = cast('${v_Trx_Dt}' as date format'yyyy-mm-dd')
   where  b.Cust_UID IS null
   ;
   IF lv_mig_v_IsLastDay = 1 THEN
              GOTO doit;
   END IF;

Specifying Variables Using SELECT EXTRACT FROM
----------------------------------------------

**Input**

.. code-block::

   select EXTRACT (MONTH FROM cast('${TX_DATE}' as  date format 'YYYYMMDD')) as CURR_MON;
   .if ERRORCODE <> 0 then .quit 12;
   --
   .IF CURR_MON <> 1 THEN .goto NON_JAN;
   .if ERRORCODE <> 0 then .quit 12;

**Output**

.. code-block::

   BEGIN
      select EXTRACT (MONTH FROM cast('${TX_DATE}' as  date format 'YYYYMMDD')) INTO CURR_MON;
   …
   END;
   …

DATE Type Casting: Specifying DSQL Variables
--------------------------------------------

**Input**

.. code-block::

   select
      case when a.DW_Stat_Dt in
   (date'${v_Tx_Pre_1_Mon_End_Date}'
   ,date'${v_Tx_Pre_2_Mon_End_Date}'
   ,date'${v_Tx_Pre_3_Mon_End_Date}')
   then '1' else '2' end as Quarter_Flg          /* PK-     1--    2--     3--   */
     ,a.BBK_Org_Id as BBK_Org_Id          /* PK-         */
     ,a.Cust_UID as Cust_UID          /* PK-  UID */
     ,a.Entp_Cust_Id as Entp_Cust_Id          /* PK-       */
     ,coalesce(b.BBK_Nbr,'') as Agn_BBK_Org_Id          /* PK-       */
   from ${BRTL_VEXT}.BRTL_AS_AGN_ENTP_CUID_TRX_SR as a          /* AS_      UID     */
   LEFT OUTER JOIN ${BRTL_VCOR}.BRTL_OR_RTL_ORG_INF_S as b          /* OR_         */
     on  a.Agn_BRN_Org_Id = b.Rtl_Org_Id
   and b.DW_Snsh_Dt = cast('${v_Trx_Dt}' as date format 'yyyy-mm-dd')
   where  a.DW_Stat_Dt IN (date'${v_Tx_Pre_1_Mon_End_Date}'
   ,date'${v_Tx_Pre_2_Mon_End_Date}'
   ,date'${v_Tx_Pre_3_Mon_End_Date}'
   ,date'${v_Tx_Pre_4_Mon_End_Date}'
   ,date'${v_Tx_Pre_5_Mon_End_Date}'
   ,date'${v_Tx_Pre_6_Mon_End_Date}')
              AND a.Stat_Prd_Cd = 'M001'
   group by Quarter_Flg, a.BBK_Org_Id, a.Cust_UID, a.Entp_Cust_Id, coalesce(b.BBK_Nbr,'')
   ;
   should be migrated as below:
              SELECT
                        CASE WHEN a.DW_Stat_Dt IN
           ( CAST(lv_mig_v_Tx_Pre_1_Mon_End_Date AS DATE)
                         , CAST(lv_mig_v_Tx_Pre_2_Mon_End_Date AS DATE)
                         , CAST(lv_mig_v_Tx_Pre_3_Mon_End_Date AS DATE) )
                             THEN '1'
                           ELSE '2'
                        END AS Quarter_Flg /* PK-     1--    2--     3--   */
                        ,a.BBK_Org_Id AS BBK_Org_Id /* PK-         */
                        ,a.Cust_UID AS Cust_UID /* PK-  UID */
                        ,a.Entp_Cust_Id AS Entp_Cust_Id /* PK-       */
                        ,COALESCE( b.BBK_Nbr ,'' ) AS Agn_BBK_Org_Id /* PK-       */
                   FROM
                        BRTL_VEXT.BRTL_AS_AGN_ENTP_CUID_TRX_SR AS a /* AS_      UID     */
                        LEFT OUTER JOIN BRTL_VCOR.BRTL_OR_RTL_ORG_INF_S AS b /* OR_         */
                             ON a.Agn_BRN_Org_Id = b.Rtl_Org_Id
                        AND b.DW_Snsh_Dt = CAST( lv_mig_v_Trx_Dt AS DATE )
                   WHERE
                        a.DW_Stat_Dt IN ( CAST(lv_mig_v_Tx_Pre_1_Mon_End_Date AS DATE)
                        ,CAST(lv_mig_v_Tx_Pre_2_Mon_End_Date AS DATE)
                        ,CAST(lv_mig_v_Tx_Pre_3_Mon_End_Date AS DATE)
                        ,CAST(lv_mig_v_Tx_Pre_4_Mon_End_Date AS DATE)
                        ,CAST(lv_mig_v_Tx_Pre_5_Mon_End_Date AS DATE)
                        ,CAST(lv_mig_v_Tx_Pre_6_Mon_End_Date AS DATE) )
                        AND a.Stat_Prd_Cd = 'M001'
                   GROUP BY Quarter_Flg, a.BBK_Org_Id, a.Cust_UID, a.Entp_Cust_Id, COALESCE( b.BBK_Nbr ,'' )
     ;

**Output**

.. code-block::

    SELECT
                        CASE WHEN a.DW_Stat_Dt IN
           ( CAST(lv_mig_v_Tx_Pre_1_Mon_End_Date AS DATE)
                         , CAST(lv_mig_v_Tx_Pre_2_Mon_End_Date AS DATE)
                         , CAST(lv_mig_v_Tx_Pre_3_Mon_End_Date AS DATE) )
                             THEN '1'
                           ELSE '2'
                        END AS Quarter_Flg /* PK-     1--    2--     3--   */
                        ,a.BBK_Org_Id AS BBK_Org_Id /* PK-         */
                        ,a.Cust_UID AS Cust_UID /* PK-  UID */
                        ,a.Entp_Cust_Id AS Entp_Cust_Id /* PK-       */
                        ,COALESCE( b.BBK_Nbr ,'' ) AS Agn_BBK_Org_Id /* PK-       */
                   FROM
                        BRTL_VEXT.BRTL_AS_AGN_ENTP_CUID_TRX_SR AS a /* AS_      UID     */
                        LEFT OUTER JOIN BRTL_VCOR.BRTL_OR_RTL_ORG_INF_S AS b /* OR_         */
                             ON a.Agn_BRN_Org_Id = b.Rtl_Org_Id
                        AND b.DW_Snsh_Dt = CAST( lv_mig_v_Trx_Dt AS DATE )
                   WHERE
                        a.DW_Stat_Dt IN ( CAST(lv_mig_v_Tx_Pre_1_Mon_End_Date AS DATE)
                        ,CAST(lv_mig_v_Tx_Pre_2_Mon_End_Date AS DATE)
                        ,CAST(lv_mig_v_Tx_Pre_3_Mon_End_Date AS DATE)
                        ,CAST(lv_mig_v_Tx_Pre_4_Mon_End_Date AS DATE)
                        ,CAST(lv_mig_v_Tx_Pre_5_Mon_End_Date AS DATE)
                        ,CAST(lv_mig_v_Tx_Pre_6_Mon_End_Date AS DATE) )
                        AND a.Stat_Prd_Cd = 'M001'
                   GROUP BY Quarter_Flg, a.BBK_Org_Id, a.Cust_UID, a.Entp_Cust_Id, COALESCE( b.BBK_Nbr ,'' )
     ;
