:original_name: dws_07_8120.html

.. _dws_07_8120:

BTEQ Utility Command
====================

The BTEQ utility commands are as follows:

LOGOFF QUIT
-----------

LOGOFF ends the current RDBMS sessions without exiting BTEQ.

QUIT command ends the current Teradata database sessions and exits BTEQ.

**Input**

.. code-block::

   SELECT 'StartDTTM' as a
               ,CURRENT_TIMESTAMP (FORMAT 'HH:MI:SSBMMMBDD,BYYYY') ;
   .LOGOFF;
   .QUIT;

**Output**

.. code-block::

   SELECT 'StartDTTM' as a
               ,CURRENT_TIMESTAMP (FORMAT 'HH:MI:SSBMMMBDD,BYYYY') ;
   .LOGOFF;
   .QUIT;

.. note::

   Gauss does not support, so it is required to comment.

.IF ERRORCODE and .QUIT
-----------------------

BTEQ statements specified with .IF and .QUIT.

**Input**

.. code-block::

   COLLECT STATISTICS
    USING SAMPLE 5.00 PERCENT
    COLUMN ( CDR_TYPE_KEY ) ,
    COLUMN ( PARTITION ) ,
    COLUMN ( SRC ) ,
    COLUMN ( PARTITION,SBSCRPN_KEY )
    ON DT_SDM.FCT_OTGO_NTWK_ACTVY_DAILY ;


**Output**

.. code-block::

   SET
   default_statistics_target = 5.00 ;
   ANALYZE DT_SDM.FCT_OTGO_NTWK_ACTVY_DAILY (CDR_TYPE_KEY) ;
   ANALYZE DT_SDM.FCT_OTGO_NTWK_ACTVY_DAILY (PARTITION) ;
   ANALYZE DT_SDM.FCT_OTGO_NTWK_ACTVY_DAILY (SRC) ;
   ANALYZE DT_SDM.FCT_OTGO_NTWK_ACTVY_DAILY (PARTITION,SBSCRPN_KEY) ;
    RESET default_statistics_target ;

.IF
---

**Input:**

.. code-block::

   .IF END_MONTH_FLAG <> 'Y' THEN .GOTO LABEL_1;

**Output:**

.. code-block::

   IF END_MONTH_FLAG <> 'Y' THEN
                  GOTO LABEL_1 ;
   END IF ;

.QUIT
-----

**Input:**

.. code-block::

   .IF ERRORCODE <> 0 THEN .QUIT 12;

**Output:**

.. code-block::

   IF ERRORCODE <> 0 THEN
             RAISE EXCEPTION '12' ;
   END IF;

.IF & .QUIT
-----------

.IF and .QUIT should be moved inside the block.

+--------------------------------------+-----------------------------------------+
| Oracle Syntax                        | Syntax After Migration                  |
+======================================+=========================================+
| .. code-block::                      | .. code-block::                         |
|                                      |                                         |
|    INSERT INTO EMP                   |    DECLARE                              |
|    SELECT * FROM EMP;                |    lv_mig_errorcode NUMBER ( 4 ) ;      |
|    .IF ERRORCODE <> 0 THEN .QUIT 12; |    BEGIN                                |
|                                      |         BEGIN                           |
|                                      |          INSERT INTO EMP                |
|                                      |         SELECT * FROM EMP;              |
|                                      |         lv_mig_errorcode := 0 ;         |
|                                      |          EXCEPTION                      |
|                                      |              WHEN OTHERS THEN           |
|                                      |               lv_mig_errorcode := - 1 ; |
|                                      |         END ;                           |
|                                      |         IF lv_mig_errorcode <> 0 THEN   |
|                                      |              RAISE EXCEPTION '12' ;     |
|                                      |         END IF ;                        |
|                                      |     END ;                               |
|                                      |    /                                    |
+--------------------------------------+-----------------------------------------+

.RETURN
-------

**Input:**

.. code-block::

   .if ERRORCODE = 0 then .RETURN;

**Output:**

.. code-block::

   IF ERRORCODE = 0 THEN
             RETURN;
   END IF;

.GOTO
-----

**Input:**

.. code-block::

   .IF END_MONTH_FLAG <> 'Y' THEN .GOTO LABEL_1;

**Output:**

.. code-block::

   IF END_MONTH_FLAG <> 'Y' THEN
         GOTO LABEL_1;
   END IF ;

Label
-----

**Input:**

.. code-block::

   .LABEL LABEL_1

**Output:**

.. code-block::

   <<LABEL_1>>

ERRORCODE 3807
--------------

**Input:**

.. code-block::

   SELECT end_mon AS END_MONTH_FLAG
     FROM tab2 ;

   .IF END_MONTH_FLAG <> 'Y' THEN
           .GOTO LABEL_1;

   .IF ERRORCODE = 3807 THEN
          .QUIT 8888;

**Output**

.. code-block::

   DECLARE lv_mig_errorcode NUMBER (4);
         lv_mig_END_MONTH_FLAG TEXT;
   BEGIN
        BEGIN
             SELECT end_mon
                INTO lv_mig_END_MONTH_FLAG
               FROM tab2 ;

               lv_mig_errorcode := 0 ;
        EXCEPTION
              WHEN UNDEFINED_TABLE THEN
                    lv_mig_errorcode := 3807 ;

             WHEN OTHERS THEN
                  lv_mig_errorcode := - 1 ;
        END ;

        IF lv_mig_END_MONTH_FLAG <> 'Y' THEN
                  GOTO LABEL_1 ;
        END IF ;
        IF lv_mig_errorcode = 3807 THEN
             RAISE EXCEPTION '8888' ;
        END IF ;
   END;
   /

BT with BTEQ commands
---------------------

**Input**

.. code-block::

   BT;

   delete from ${BRTL_DCOR}.BRTL_CS_CUST_CID_UID_REL
   where DW_Job_Seq = ${v_Group_No};

   .if ERRORCODE <> 0 then .quit 12;

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

ET with BTEQ Commands
---------------------

**Input**

.. code-block::

   ET;
   BEGIN

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

**Output**

.. code-block::

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

.Export FILE
------------

.EXPORT FILE should be changed to COPY and teradata utilities is set to **false**.

+---------------------------------------------------+----------------------------------------------------------------------------------------------------+
| Oracle Syntax                                     | Syntax After Migration                                                                             |
+===================================================+====================================================================================================+
| .. code-block::                                   | .. code-block::                                                                                    |
|                                                   |                                                                                                    |
|    .export file = $FILENAME;                      |    COPY (select empno, ename from emp where deptno = 1) TO '$FILENAME' CSV HEADER DELIMITER E'\t'; |
|    select empno, ename from emp where deptno = 1; |                                                                                                    |
+---------------------------------------------------+----------------------------------------------------------------------------------------------------+

.EXPORT FILE should be changed to $q$COPY and moved inside the block when teradataUtilities is set to **true**.

+---------------------------------------------------+-------------------------------------------------------------------------------------------------------------------+
| Oracle Syntax                                     | Syntax After Migration                                                                                            |
+===================================================+===================================================================================================================+
| .. code-block::                                   | .. code-block::                                                                                                   |
|                                                   |                                                                                                                   |
|    print BTEQ <<ENDOFINPUT;                       |    DECLARE lv_mig_obj_exists_check NUMBER(1);                                                                     |
|       DATABASE HPBUS;                             |     lv_mig_errorcode NUMBER(4);                                                                                   |
|    .export file = $FILENAME;                      |    BEGIN                                                                                                          |
|    select empno, ename from emp where deptno = 1; |     SET SESSION CURRENT_SCHEMA TO public ;                                                                        |
|    .IF ERRORCODE <> 0 THEN .QUIT 12;              |                                                                                                                   |
|    .LOGOFF;                                       |        BEGIN                                                                                                      |
|    .QUIT 0;                                       |      SELECT COUNT(*) INTO lv_mig_obj_exists_check                                                                 |
|    ENDOFINPUT                                     |        FROM (select empno, ename from emp where deptno = 1);                                                      |
|                                                   |            lv_mig_errorcode := 0;                                                                                 |
|                                                   |                                                                                                                   |
|                                                   |        EXCEPTION                                                                                                  |
|                                                   |            WHEN OTHERS THEN                                                                                       |
|                                                   |                lv_mig_errorcode := -1;                                                                            |
|                                                   |        END ;                                                                                                      |
|                                                   |     EXECUTE $q$COPY (select empno, ename from emp where deptno = 1) TO '$FILENAME' CSV HEADER DELIMITER E'\t'$q$; |
|                                                   |                                                                                                                   |
|                                                   |        IF lv_mig_errorcode <> 0 THEN                                                                              |
|                                                   |              RAISE EXCEPTION '12';                                                                                |
|                                                   |        END IF ;                                                                                                   |
|                                                   |     RAISE EXCEPTION '-99';                                                                                        |
|                                                   |        RETURN ;                                                                                                   |
|                                                   |    END ;                                                                                                          |
|                                                   |    /                                                                                                              |
+---------------------------------------------------+-------------------------------------------------------------------------------------------------------------------+

SQL_Lang & BTEQ
---------------

Multiple tag names should be handled for SQL_Lang.

+-----------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------+
| Oracle Syntax                                                                     | Syntax After Migration                                                                           |
+===================================================================================+==================================================================================================+
| .. code-block::                                                                   | .. code-block::                                                                                  |
|                                                                                   |                                                                                                  |
|    #!/usr/bin/perl                                                                |    #!/usr/bin/perl                                                                               |
|    ######################################################################         |    ######################################################################                        |
|    # BTEQ script in Perl                                                          |    # BTEQ script in Perl                                                                         |
|    # Date Time    : 2013-10-15                                                    |    # Date Time    : 2013-10-15                                                                   |
|    # Table        : TB_NET_LTE_GPRS_CDR (4G GPRS»°µ¥)                             |    # Table        : TB_NET_LTE_GPRS_CDR (4G GPRS)                                                |
|    # Script Name  : XXX00_TB_NET_LTE_GPRS_CDR                                     |    # Script Name  : XXX00_TB_NET_LTE_GPRS_CDR                                                    |
|    # Source Table :  TB_04024-GPRS                                                |    # Source Table :  TB_04024-GPRS                                                               |
|    # Load strategy: S4                                                            |    # Load strategy: S4                                                                           |
|    use strict; # Declare using Perl strict syntax                                 |    use strict; # Declare using Perl strict syntax                                                |
|                                                                                   |                                                                                                  |
|    my $HOME = $ENV{"AUTO_HOME"};                                                  |    my $HOME = $ENV{"AUTO_HOME"};                                                                 |
|    unshift(@INC, "$HOME/bin");                                                    |    unshift(@INC, "$HOME/bin");                                                                   |
|    require etl_pub;                                                               |    require etl_pub;                                                                              |
|                                                                                   |                                                                                                  |
|    # ------------ Variable Section ------------                                   |    # ------------ Variable Section ------------                                                  |
|    # DATABASE NAMES                                                               |    # DATABASE NAMES                                                                              |
|    my $TEMPDB = $ETL::TEMPDB;                                                     |    my $TEMPDB = $ETL::TEMPDB;                                                                    |
|    my $SDATADB = $ETL::SDATADB;                                                   |    my $SDATADB = $ETL::SDATADB;                                                                  |
|    my $PDATADB = $ETL::PDATADB;                                                   |    my $PDATADB = $ETL::PDATADB;                                                                  |
|    my $PARADB = $ETL::PARADB;                                                     |    my $PARADB = $ETL::PARADB;                                                                    |
|    my $PDDL = $ETL::PDDL;                                                         |    my $PDDL = $ETL::PDDL;                                                                        |
|                                                                                   |                                                                                                  |
|    my $PCODE = $ETL::PCODE;                                                       |    my $PCODE = $ETL::PCODE;                                                                      |
|    if ( $#ARGV < 0 ) {                                                            |    if ( $#ARGV < 0 ) {                                                                           |
|       exit(1);                                                                    |       exit(1);                                                                                   |
|    }                                                                              |    }                                                                                             |
|    my $CONTROL_FILE = $ARGV[0];                                                   |    my $CONTROL_FILE = $ARGV[0];                                                                  |
|    my $TX_DATE = substr(${CONTROL_FILE},length(${CONTROL_FILE})-12, 8);           |    my $TX_DATE = substr(${CONTROL_FILE},length(${CONTROL_FILE})-12, 8);                          |
|                                                                                   |                                                                                                  |
|    open(STDERR, ">&STDOUT");                                                      |    open(STDERR, ">&STDOUT");                                                                     |
|    my $TRG_COL_LIST =<<TRGCOLLIST;                                                |    my $TRG_COL_LIST=<<TRGCOLLIST ;                                                               |
|     MSISDN                                                                        |    MSISDN                                                                                        |
|    ,dat_rcd_dt                                                                    |    ,dat_rcd_dt                                                                                   |
|    ,vst_rgn_cd                                                                    |    ,vst_rgn_cd                                                                                   |
|                                                                                   |                                                                                                  |
|    TRGCOLLIST                                                                     |    TRGCOLLIST                                                                                    |
|                                                                                   |                                                                                                  |
|    my $MAP_COL_LIST =<<MAPCOLLIST;                                                |    my $MAP_COL_LIST=<<MAPCOLLIST ;                                                               |
|     TB_04024.msisdn                                                               |    TB_04024.msisdn                                                                               |
|     ,CAST('$TX_DATE' AS DATE FORMAT 'YYYYMMDD')                                   |    ,CAST( '$TX_DATE' AS DATE )                                                                   |
|     ,TB_04024.vst_rgn_cd                                                          |    ,TB_04024.vst_rgn_cd                                                                          |
|    MAPCOLLIST                                                                     |    MAPCOLLIST                                                                                    |
|                                                                                   |                                                                                                  |
|    my $FILTER = "CMCC_Prov_Prvd_Id=$PCODE";                                       |    my $FILTER = "CMCC_Prov_Prvd_Id=$PCODE";                                                      |
|    my $table_today ="${TEMPDB}.TB_04024_${PCODE}";                                |    my $table_today ="${TEMPDB}.TB_04024_${PCODE}";                                               |
|    my $table_target = "Z" . substr($PCODE,1,2) . "NET_LTE_GPRS_CDR";              |    my $table_target = "Z" . substr($PCODE,1,2) . "NET_LTE_GPRS_CDR";                             |
|    my $UNIT_FLAG;                                                                 |    my $UNIT_FLAG;                                                                                |
|                                                                                   |                                                                                                  |
|    sub BTEQ_S4                                                                    |                                                                                                  |
|    {                                                                              |    sub BTEQ_S4                                                                                   |
|     my ($dbh) = @_;                                                               |    {                                                                                             |
|     my $BTEQ_CMD_S4 = <<ENDOFINPUT;                                               |     my ($dbh) = @_;                                                                              |
|                                                                                   |    my $BTEQ_CMD_S4=<<ENDOFINPUT ;                                                                |
|    INSERT INTO ${PDATADB}.$table_target (                                         |                                                                                                  |
|    $TRG_COL_LIST)                                                                 |    DECLARE lv_mig_obj_exists_check NUMBER ( 1 ) ;                                                |
|    SELECT                                                                         |    lv_mig_errorcode NUMBER ( 4 ) ;                                                               |
|    $MAP_COL_LIST                                                                  |                                                                                                  |
|    FROM $SDATADB.TB_${PCODE}_04024_${UNIT_FLAG}_${TX_DATE} TB_04024;              |    BEGIN                                                                                         |
|    .IF ERRORCODE <> 0 THEN .QUIT 12;                                              |                                                                                                  |
|    ENDOFINPUT                                                                     |                                                                                                  |
|     return $BTEQ_CMD_S4;                                                          |                                                                                                  |
|    }                                                                              |         BEGIN                                                                                    |
|    my $unit_num = "04024";                                                        |              INSERT INTO ${PDATADB}.$table_target ($TRG_COL_LIST)                                |
|                                                                                   |              SELECT                                                                              |
|    sub BTEQ_Z1                                                                    |                        $MAP_COL_LIST                                                             |
|    {                                                                              |                   FROM                                                                           |
|     my $BTEQ_CMD_Z1 = <<ENDOFINPUT;                                               |                        $SDATADB.TB_${PCODE}_04024_${UNIT_FLAG}_${TX_DATE} TB_04024 ;             |
|    INSERT INTO ${PDATADB}.$table_target (                                         |                        lv_mig_errorcode := 0 ;                                                   |
|    $TRG_COL_LIST)                                                                 |                                                                                                  |
|    SELECT                                                                         |                   EXCEPTION                                                                      |
|    $MAP_COL_LIST                                                                  |                        WHEN OTHERS THEN                                                          |
|    FROM $SDATADB.TB_${PCODE}_${unit_num}_${UNIT_FLAG}_${TX_DATE} tb_${unit_num};  |                        lv_mig_errorcode := - 1 ;                                                 |
|    .IF ERRORCODE <> 0 THEN .QUIT 12;                                              |                                                                                                  |
|    ENDOFINPUT                                                                     |         END ;                                                                                    |
|     return $BTEQ_CMD_Z1;                                                          |         IF lv_mig_errorcode <> 0 THEN                                                            |
|    }                                                                              |              RAISE EXCEPTION '12' ;                                                              |
|                                                                                   |                                                                                                  |
|    sub main()                                                                     |         END IF ;                                                                                 |
|    {                                                                              |                                                                                                  |
|     my $dbh=ETL::DBconnect();                                                     |    END ;                                                                                         |
|    # SDATA                                                                        |    /                                                                                             |
|     $UNIT_FLAG = ETL::get_UNIT_FLAG($dbh, "TB_04024", $PCODE, $TX_DATE);          |    ENDOFINPUT                                                                                    |
|     my $BTEQCMD = "";                                                             |     return $BTEQ_CMD_S4;                                                                         |
|     if ( $UNIT_FLAG eq " " ) {                                                    |    }                                                                                             |
|      print "Ô!\n";                                                                |    my $unit_num = "04024";                                                                       |
|      ETL::disconnectETL($dbh);                                                    |                                                                                                  |
|      return 1;                                                                    |    sub BTEQ_Z1                                                                                   |
|     } elsif ($UNIT_FLAG eq "S") {                                                 |    {                                                                                             |
|      $BTEQCMD = BTEQ_S4($dbh);                                                    |    my $BTEQ_CMD_Z1=<<ENDOFINPUT ;                                                                |
|     } elsif ($UNIT_FLAG eq "Z") {                                                 |                                                                                                  |
|      $BTEQCMD = BTEQ_Z1($dbh);                                                    |    DECLARE lv_mig_obj_exists_check NUMBER ( 1 ) ;                                                |
|     } else {                                                                      |    lv_mig_errorcode NUMBER ( 4 ) ;                                                               |
|      print "\n";                                                                  |                                                                                                  |
|      return 1;                                                                    |         BEGIN                                                                                    |
|     }                                                                             |              INSERT INTO ${PDATADB}.$table_target ($TRG_COL_LIST)                                |
|     ETL::disconnectETL($dbh);                                                     |              SELECT                                                                              |
|                                                                                   |                        $MAP_COL_LIST                                                             |
|     return ETL::ExecuteBTEQ($BTEQCMD, $TX_DATE);                                  |                   FROM                                                                           |
|    }                                                                              |                        $SDATADB.TB_${PCODE}_${unit_num}_${UNIT_FLAG}_${TX_DATE} tb_${unit_num} ; |
|    my $ret= main();                                                               |                        lv_mig_errorcode := 0 ;                                                   |
|    exit($ret);                                                                    |                                                                                                  |
|                                                                                   |                   EXCEPTION                                                                      |
|                                                                                   |                        WHEN OTHERS THEN                                                          |
|                                                                                   |                        lv_mig_errorcode := - 1 ;                                                 |
|                                                                                   |                                                                                                  |
|                                                                                   |         END ;                                                                                    |
|                                                                                   |         IF lv_mig_errorcode <> 0 THEN                                                            |
|                                                                                   |              RAISE EXCEPTION '12' ;                                                              |
|                                                                                   |                                                                                                  |
|                                                                                   |         END IF ;                                                                                 |
|                                                                                   |                                                                                                  |
|                                                                                   |    END ;                                                                                         |
|                                                                                   |    /                                                                                             |
|                                                                                   |    ENDOFINPUT                                                                                    |
|                                                                                   |     return $BTEQ_CMD_Z1;                                                                         |
|                                                                                   |    }                                                                                             |
|                                                                                   |                                                                                                  |
|                                                                                   |    sub main()                                                                                    |
|                                                                                   |    {                                                                                             |
|                                                                                   |     my $dbh=ETL::DBconnect();                                                                    |
|                                                                                   |    # SDATA                                                                                       |
|                                                                                   |     $UNIT_FLAG = ETL::get_UNIT_FLAG($dbh, "TB_04024", $PCODE, $TX_DATE);                         |
|                                                                                   |     my $BTEQCMD = "";                                                                            |
|                                                                                   |     if ( $UNIT_FLAG eq " " ) {                                                                   |
|                                                                                   |      print "!\n";                                                                                |
|                                                                                   |      ETL::disconnectETL($dbh);                                                                   |
|                                                                                   |      return 1;                                                                                   |
|                                                                                   |     } elsif ($UNIT_FLAG eq "S") {                                                                |
|                                                                                   |      $BTEQCMD = BTEQ_S4($dbh);                                                                   |
|                                                                                   |     } elsif ($UNIT_FLAG eq "Z") {                                                                |
|                                                                                   |      $BTEQCMD = BTEQ_Z1($dbh);                                                                   |
|                                                                                   |     } else {                                                                                     |
|                                                                                   |      print "¸\n";                                                                                |
|                                                                                   |      return 1;                                                                                   |
|                                                                                   |     }                                                                                            |
|                                                                                   |     ETL::disconnectETL($dbh);                                                                    |
|                                                                                   |                                                                                                  |
|                                                                                   |     return ETL::ExecuteBTEQ($BTEQCMD, $TX_DATE);                                                 |
|                                                                                   |    }                                                                                             |
|                                                                                   |    my $ret= main();                                                                              |
|                                                                                   |    exit($ret);                                                                                   |
+-----------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------+
