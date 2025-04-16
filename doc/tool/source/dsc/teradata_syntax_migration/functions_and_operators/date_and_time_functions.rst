:original_name: dws_16_0046.html

.. _dws_16_0046:

.. _en-us_topic_0000001813598892:

Date and Time Functions
=======================

.. _en-us_topic_0000001813598892__en-us_topic_0000001434630645_section1364584752615:

DATE
----

DSC supports the migration of Teradata SELECT statements that contain DATE FORMAT, using TO_CHAR to display the date in the source format. This conversion is not done if the date format is an expression (example: Start_Dt + 30) or if the WHERE statement contains an expression (Example: WHERE Start_Dt > End_Dt).

For details, see: :ref:`Type Casting to DATE without DATE Keyword <en-us_topic_0000001860318709__en-us_topic_0000001384071376_section1042495513341>`

.. note::

   -  Migration is supported for SELECT statements with and without column alias.

   -  Date formatting is not supported in the sub-levels and in inner queries. It is supported only at the outer query level.

   -  For date formatting, if a table is created with SCHEMA name, subsequent SELECT statements must also include the schema name. In the following example, the table TEMP_TBL in the SELECT statement will not be migrated and the table retained as it was.

      ::

         CREATE TABLE ${SCH}.TEMP_TBL
                  (C1 INTEGER
                  ,C2 DATE FORMAT 'YYYY-MM-DD')
         PRIMARY INDEX(C1,C2);

         SELECT ${SCH}.TEMP_TBL.C2 FROM TEMP_TBL where ${SCH}.TEMP_TBL.C2 is not null;

**Input: DATE FORMAT**

::

   SELECT
             CASE
                  WHEN SUBSTR( CAST( CAST( SUBSTR( '20180631' ,1 ,6 ) || '01' AS DATE FORMAT 'YYYYMMDD' ) + abc_day - 1 AS FORMAT 'YYYYMMDD' ) ,1 ,6 ) = SUBSTR( '20180631' ,1 ,6 )
                  THEN 1
                  ELSE 0
             END
        FROM
             tab1
   ;

**Output:**

::

   SELECT
             CASE
                  WHEN SUBSTR( TO_CHAR( CAST( SUBSTR( '20180631' ,1 ,6 ) || '01' AS DATE ) + abc_day - 1 ,'YYYYMMDD' ) ,1 ,6 ) = SUBSTR( '20180631' ,1 ,6 )
                  THEN 1
                  ELSE 0
             END
        FROM
             tab1
   ;

DSC supports migration of the date value. If the input DATE is followed by "YYYY-MM-DD", then the date is not changed in the output. The following examples show conversion of DATE to CURRENT_DATE.

**Input: DATE**

::

   SELECT
           t1.c1
           ,t2.c2
     FROM
           $schema.tab1 t1
           ,$schema.tab2 t2
     WHERE
           t1.c3 ^ = t1.c3
           AND t2.c4 GT DATE
   ;

**Output:**

::

   SELECT
           t1.c1
           ,t2.c2
     FROM
           "$schema".tab1 t1
           ,"$schema".tab2 t2
    WHERE
           t1.c3 <> t1.c3
           AND t2.c4 > CURRENT_DATE
   ;

**Input: DATE with "YYYY-MM-DD"**

::

   ALTER TABLE
        $abc . tab1 ADD (
             col_date DATE DEFAULT DATE '2000-01-01'
        )
   ;

**Output**

::

   ALTER TABLE
        "$abc" . tab1 ADD (
             col_date DATE DEFAULT DATE '2000-01-01'
        )
   ;

**Input: DATE subtraction**

::

   SELECT
             CAST( T1.Buyback_Mature_Dt - CAST( '${gsTXDate}' AS DATE FORMAT 'YYYYMMDD' ) AS CHAR( 5 ) )
        FROM
             tab1 T1
        WHERE
             T1.col1 > 10
   ;

**Output:**

::

   SELECT
             CAST( EXTRACT( 'DAY' FROM ( T1.Buyback_Mature_Dt - CAST( '${gsTXDate}' AS DATE ) ) ) AS CHAR( 5 ) )
        FROM
             tab1 T1
        WHERE
             T1.col1 > 10
   ;

ADD_MONTHS
----------

**Input:**

.. code-block::

   ADD_MONTHS(CAST(substr(T1.GRANT_DATE,1,8)||'01'AS DATE FORMAT 'YYYY-MM-DD'),1)-1

**Output:**

.. code-block::

   mig_td_ext.ADD_MONTHS(CAST(substr(T1.GRANT_DATE,1,8)||'01'AS DATE FORMAT 'YYYY-MM-DD'),1)-1

.. _en-us_topic_0000001813598892__en-us_topic_0000001434630645_section746313461190:

TIMESTAMP
---------

**Input: TIMESTAMP**

.. code-block::

   select CAST('20190811'||' '||'01:00:00'
   AS TIMESTAMP(0)
   FORMAT 'YYYYMMDDBHH:MI:SS'
   ) ;

**Output:**

::

   SELECT TO_TIMESTAMP( '20190811' || ' ' || '01:00:00' ,'YYYYMMDD HH24:MI:SS' ) ;

TIME FORMAT
-----------

**Input:**

.. code-block::

   COALESCE(t3.Crt_Tm , CAST('00:00:00' AS TIME FORMAT 'HH:MI:SS'))
   COALESCE(LI07_F3EABCTLP.CTLREGTIM,CAST('${NULL_TIME}' AS TIME FORMAT 'HH:MI:sS'))
   trim(cast(cast(a.Ases_Orig_Tm as time format'hhmiss') as varchar(10)))

**Output:**

.. code-block::

   CAST('00:00:00' AS TIME FORMAT 'HH:MI:SS')
   should be migrated as
   SELECT CAST(TO_TIMESTAMP('00:00:00', 'HH24:MI:SS') AS TIME)
   ---
   CAST(abc AS TIME FORMAT 'HH:MI:sS')
   =>
   CAST(TO_TIMESTAMP(abc, 'HH24:MI:SS') AS TIME)
   ---
   CAST(abc AS TIME FORMAT 'HH:MI:sS')
   =>
   CAST(TO_TIMESTAMP(abc, 'HH24:MI:SS') AS TIME)

TIMESTAMP FORMAT
----------------

**Input:**

.. code-block::

   select
      a.Org_Id as Brn_Org_Id          /*        */
     ,a.Evt_Id as Vst_Srl_Nbr          /*       */
     ,a.EAC_Id as EAC_Id          /*    */
     ,cast(cast(cast(Prt_Tm as timestamp format 'YYYY-MM-DDBHH:MI:SS' ) as varchar(19) )as timestamp(0)) as Tsk_Start_Tm          /*        */
   from ${BRTL_VCOR}.BRTL_BC_SLF_TMN_RTL_PRT_JNL as a          /* BC_           */
   where   a.DW_Dat_Dt  = CAST('${v_Trx_Dt}' AS DATE FORMAT 'YYYY-MM-DD')  ;

**Output:**

.. code-block::

   SELECT
             a.Org_Id AS Brn_Org_Id /*        */
             ,a.Evt_Id AS Vst_Srl_Nbr /*       */
             ,a.EAC_Id AS EAC_Id /*    */
             ,CAST( CAST( TO_TIMESTAMP( Prt_Tm ,'YYYY-MM-DD HH24:MI:SS' ) AS VARCHAR( 19 ) ) AS TIMESTAMP ( 0 ) ) AS Tsk_Start_Tm /*        */
        FROM ${BRTL_VCOR}.BRTL_BC_SLF_TMN_RTL_PRT_JNL AS a /* BC_           */
        WHERE
             a.DW_Dat_Dt = CAST( '${v_Trx_Dt}' AS DATE ) ;

TIMESTAMP(n) FORMAT
-------------------

**Input:**

.. code-block::

   select
      cast('${v_Trx_Dt}' as date format 'yyyy-mm-dd') as DW_Snsh_Dt          /*      */
     ,coalesce(a.CRE_DAT,cast('0001-01-01 00:00:01' as timestamp(6) format 'yyyy-mm-ddbhh:mi:ssds(6)')) as Crt_Tm          /*      */
     ,cast('${v_Trx_Dt}' as date format 'yyyy-mm-dd') as DW_ETL_Dt          /*      */
     ,cast(current_date as date format 'yyyy-mm-dd') as DW_Upd_Dt          /*      */
     ,current_time(0) as DW_Upd_Tm          /*      */
     ,1 as DW_Job_Seq          /*      */
   from ${NDS_VIEW}.NLV65_MGM_GLDCUS_INF_NEW as a          /*    MGM    */
   ;
   -----------
   cast('0001-01-01 00:00:00' as timestamp(6) format 'yyyy-mm-ddbhh:mi:ssds(6)')
   TO_TIMESTAMP('0001-01-01 00:00:00', 'yyyy-mm-dd HH24:MI:SS.US' )
   ----------
   cast('0001-01-01 00:00:00.000000' as timestamp(6))
   cast('0001-01-01 00:00:00.000000' as timestamp(6))
   ----------
   CAST('0001-01-01 00:00:00.000000' AS TIMESTAMP(6) FORMAT 'YYYY-MM-DDBHH:MI:SS.S(6)')
   TO_TIMESTAMP('0001-01-01 00:00:00.000000', 'yyyy-mm-dd HH24:MI:SS.US' )
   ----------
   cast(LA02_USERLOG_M.LOGTIME as TIMESTAMP(6) FORMAT 'YYYY-MM-DD HH:MI:SS.S(0)' )
   TO_TIMESTAMP(LA02_USERLOG_M.LOGTIME, 'YYYY-MM-DD HH24:MI:SS' )
   ----------
   cast('0001-01-01 00:00:00' as timestamp(3) format 'yyyy-mm-ddbhh:mi:ssds(3)')
   TO_TIMESTAMP('0001-01-01 00:00:00', 'yyyy-mm-dd HH24:MI:SS.MS' )
   -----------
   CAST( '0001-01-01 00:00:01.000000' AS TIMESTAMP ( 6 ) format 'yyyy-mm-ddbhh:mi:ssds(6)' )
   TO_TIMESTAMP('0001-01-01 00:00:01.000000', 'yyyy-mm-dd HH24:MI:SS.US' )

**Output:**

.. code-block::

   cast('0001-01-01 00:00:00' as timestamp(6) format 'yyyy-mm-ddbhh:mi:ssds(6)')
   TO_TIMESTAMP('0001-01-01 00:00:00', 'yyyy-mm-dd HH24:MI:SS.US' )
   ----------
   cast('0001-01-01 00:00:00.000000' as timestamp(6))
   cast('0001-01-01 00:00:00.000000' as timestamp(6))
   ----------
   CAST('0001-01-01 00:00:00.000000' AS TIMESTAMP(6) FORMAT 'YYYY-MM-DDBHH:MI:SS.S(6)')
   TO_TIMESTAMP('0001-01-01 00:00:00.000000', 'yyyy-mm-dd HH24:MI:SS.US' )
   ----------
   cast(LA02_USERLOG_M.LOGTIME as TIMESTAMP(6) FORMAT 'YYYY-MM-DD HH:MI:SS.S(0)' )
   TO_TIMESTAMP(LA02_USERLOG_M.LOGTIME, 'YYYY-MM-DD HH24:MI:SS' )
   ----------
   cast('0001-01-01 00:00:00' as timestamp(3) format 'yyyy-mm-ddbhh:mi:ssds(3)')
   TO_TIMESTAMP('0001-01-01 00:00:00', 'yyyy-mm-dd HH24:MI:SS.MS' )
   -----------
   CAST( '0001-01-01 00:00:01.000000' AS TIMESTAMP ( 6 ) format 'yyyy-mm-ddbhh:mi:ssds(6)' )
   TO_TIMESTAMP('0001-01-01 00:00:01.000000', 'yyyy-mm-dd HH24:MI:SS.US' )

trunc(<date>, 'MM') trunc(<date>, 'YY')
---------------------------------------

**Input:**

.. code-block::

   select
      cast('${v_Trx_Dt}' as date format 'yyyy-mm-dd') as DW_Stat_Dt          /*  */
     ,coalesce(d.IAC_Id,'') as IAC_Id          /*  */
     ,coalesce(d.IAC_Mdf,'') as IAC_Mdf          /*  */
     ,coalesce(c.Rtl_Wlth_Prod_Id,'') as Rtl_Wlth_Prod_Id          /*  */
     ,coalesce(c.Ccy_Cd,'') as Ccy_Cd          /*  */
     ,0 as Lot_Bal          /*  */
     ,cast(sum(case when s2.Nvld_Dt > cast('${v_Trx_Dt}' as date format 'yyyy-mm-dd') then s2.Pos_Amt else 0 end) as decimal(18,2)) as NP_Occy_TMKV          /*          */
     ,cast(
           sum(s2.Pos_Amt *
             ((case when s2.Nvld_Dt > cast('${v_Trx_Dt}' as date format 'yyyy-mm-dd')
                       then cast('${v_Trx_Dt}' as date format 'yyyy-mm-dd') else s2.Nvld_Dt - 1 end)
             -
              (case when s2.Eft_Dt > trunc(cast('${v_Trx_Dt}' as date format 'yyyy-mm-dd'),'MM')
                 then s2.Eft_Dt else trunc(cast('${v_Trx_Dt}' as date format 'yyyy-mm-dd'),'MM')
              end)
             + 1
             )
              )
   /

**Output:**

.. code-block::

   date_trunc('month', cast('${v_Trx_Dt}' as date))
   date_trunc('year', cast('${v_Trx_Dt}' as date))

.. _en-us_topic_0000001813598892__en-us_topic_0000001434630645_section756419279914:

NEXT
----

**Input: NEXT**

.. code-block::

   SELECT c1, c2
     FROM tab1
    WHERE NEXT(c3) = CAST('2004-01-04' AS DATE FORMAT 'YYYY-MM-DD');

**Output:**

::

    SELECT c1, c2
     FROM tab1
    WHERE c3 + 1 = CAST('2004-01-04' AS DATE);
