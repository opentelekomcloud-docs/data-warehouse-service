:original_name: dws_16_0097.html

.. _dws_16_0097:

ACTIVITYCOUNT
=============

ACTIVITYCOUNT

**Input**

It is a status variable that returns the number of rows affected by an SQL DML statement in an embedded SQL.

::

   SEL tablename
   FROM dbc.tables
   WHERE databasename ='tera_db'
     AND tablename='tab1';

   .IF ACTIVITYCOUNT > 0 THEN .GOTO NXTREPORT;
   CREATE MULTISET TABLE tera_db.tab1
           , NO FALLBACK
           , NO BEFORE JOURNAL
           , NO AFTER JOURNAL
           , CHECKSUM = DEFAULT
             (
                       Tx_Zone_Num CHAR( 4 )
                     , Tx_Org_Num  VARCHAR( 30 )
             )
             PRIMARY INDEX
             (
                       Tx_Org_Num
             )
             INDEX
             (
                       Tx_Teller_Id
             )
   ;

   .LABEL NXTREPORT
   DEL FROM tera_db.tab1;

**Output**

::

   DECLARE v_verify TEXT ;
   v_no_data_found NUMBER ( 1 ) ;

   BEGIN
        BEGIN
             v_no_data_found := 0 ;

             SELECT
                       mig_td_ext.vw_td_dbc_tables.tablename INTO v_verify
                  FROM
                       mig_td_ext.vw_td_dbc_tables
                  WHERE
                       mig_td_ext.vw_td_dbc_tables.schemaname = 'tera_db'
                       AND mig_td_ext.vw_td_dbc_tables.tablename = 'tab1' ;

                  EXCEPTION
                       WHEN NO_DATA_FOUND THEN
                       v_no_data_found := 1 ;

        END ;

        IF
             v_no_data_found = 1 THEN
                  CREATE TABLE tera_db.tab1 (
                       Tx_Zone_Num CHAR( 4 )
                       ,Tx_Org_Num VARCHAR( 30 )
                  ) DISTRIBUTE BY HASH ( Tx_Org_Num ) ;

        CREATE
             INDEX
                  ON tera_db.tab1 ( Tx_Teller_Id ) ;

        END IF ;

        DELETE FROM
             tera_db.tab1 ;

   END ;
   /
