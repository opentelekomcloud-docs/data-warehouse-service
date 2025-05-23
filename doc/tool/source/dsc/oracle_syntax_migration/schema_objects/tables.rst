:original_name: dws_mt_0108.html

.. _dws_mt_0108:

.. _en-us_topic_0000002076353953:

Tables
======

CREATE TABLE
------------

The **CREATE TABLE** statement in Oracle databases is used to create new tables. GaussDB(DWS) also supports this statement. So it does not need to be migrated.

ALTER TABLE
-----------

The **ALTER TABLE** statement in Oracle databases is used to add, rename, modify, or drop/delete columns in a table. GaussDB(DWS) also supports this statement. It does not need to be migrated.

PRIMARY KEY
-----------

The **ALTER TABLE** statement in Oracle databases is used to add table names when the primary key appears in a different file other than the CREATE table statement.

**Input - PRIMARY KEY**

::

   CREATE TABLE CTP_ARM_CONFIG
       ( HOSTNAME VARCHAR2(50),
     OPNAME VARCHAR2(50),
     PARAMTYPE VARCHAR2(2),
     PARAMVALUE NUMBER(*,0),
     MODIFYDATE DATE
       ) SEGMENT CREATION DEFERRED
      PCTFREE 10 PCTUSED 0 INITRANS 1 MAXTRANS 255
     NOCOMPRESS LOGGING
      STORAGE( PCTINCREASE 0
      BUFFER_POOL DEFAULT FLASH_CACHE DEFAULT CELL_FLASH_CACHE DEFAULT)
      TABLESPACE SPMS_DATA ;

    ALTER TABLE CTP_ARM_CONFIG ADD CONSTRAINT PKCTP_ARM_CONFIG PRIMARY KEY (HOSTNAME, OPNAME)
      USING INDEX PCTFREE 10 INITRANS 2 MAXTRANS 255 COMPUTE STATISTICS
      STORAGE( PCTINCREASE 0
      BUFFER_POOL DEFAULT FLASH_CACHE DEFAULT CELL_FLASH_CACHE DEFAULT)
      TABLESPACE SPMS_DATA  ENABLE;

**Output**

::

   CREATE
         TABLE
              CTP_ARM_CONFIG (
                   HOSTNAME VARCHAR2 (50)
                   ,OPNAME VARCHAR2 (50)
                   ,PARAMTYPE VARCHAR2 (2)
                   ,PARAMVALUE NUMBER (
                        38
                        ,0
                   )
                   ,MODIFYDATE DATE
                   ,CONSTRAINT PKCTP_ARM_CONFIG PRIMARY KEY (
                        HOSTNAME
                        ,OPNAME
                   )
              ) /*SEGMENT CREATION DEFERRED*/
              /*PCTFREE 10*/
              /*PCTUSED 0*/
              /*INITRANS 1*/
              /*MAXTRANS 255*/
              /*NOCOMPRESS*/
              /*LOGGING*/
              /*STORAGE(  BUFFER_POOL DEFAULT FLASH_CACHE DEFAULT CELL_FLASH_CACHE DEFAULT)*/
              /*TABLESPACE SPMS_DATA */
    ;

**UNIQUE** **constraint**

The **ALTER TABLE** query contains the **UNIQUE** constraint. If it is directly executed in GaussDB(DWS), the following error shows: "Cannot create index whose evaluation cannot be enforced to remote nodes".

Similar to **PRIMARY KEY**, if the **PRIMARY KEY**/**UNIQUE** constraint already exists, you do not need to migrate it.

**Input**

::

   CREATE
        TABLE
             GCC_PLAN.T1033 (
                  ROLLOUT_PLAN_LINE_ID NUMBER NOT NULL
                  ,UDF_FIELD_VALUE_ID NUMBER NOT NULL
             ) ;
   ALTER TABLE
        GCC_PLAN.T1033 ADD CONSTRAINT UDF_FIELD_VALUE_ID_PK UNIQUE (UDF_FIELD_VALUE_ID) ;

**Output**

::

   CREATE TABLE
           GCC_PLAN.T1033
                                (
                                  ROLLOUT_PLAN_LINE_ID NUMBER NOT NULL
                                  ,UDF_FIELD_VALUE_ID NUMBER NOT NULL
                                  ,CONSTRAINT UDF_FIELD_VALUE_ID_PK UNIQUE (UDF_FIELD_VALUE_ID)
                                 ) ;

**NULL Constraint**

NULL constraint during local variable declaration is not supported in packages -that is L_CONTRACT_DISTRIBUTE_STATUS SAD_DISTRIBUTION_HEADERS_T.STATUS%TYPE NULL ;

**Input**

::

   CREATE OR REPLACE FUNCTION CONTRACT_DISTRIBUTE_STATUS_S2(PI_CONTRACT_NUMBER IN VARCHAR2)
     RETURN VARCHAR2 IS
     L_CONTRACT_DISTRIBUTE_STATUS BAS_SUBTYPE_PKG.STATUS NULL;

   BEGIN

     FOR CUR_CONTRACT IN (SELECT HT.CONTRACT_STATUS
                            FROM SAD_CONTRACTS_V HT
                           WHERE HT.HTH = PI_CONTRACT_NUMBER)
     LOOP
       IF CUR_CONTRACT.CONTRACT_STATUS = 0 THEN
         L_CONTRACT_DISTRIBUTE_STATUS := 'Cancel';
       ELSE
         L_CONTRACT_DISTRIBUTE_STATUS := BAS_SUBTYPE_PKG.G_HEADER_WAITING_SPLIT_STATUS;
       END IF;
     END LOOP;

     RETURN L_CONTRACT_DISTRIBUTE_STATUS;

   END CONTRACT_DISTRIBUTE_STATUS_S2;
   /

**Output**

::

   CREATE OR REPLACE FUNCTION CONTRACT_DISTRIBUTE_STATUS_S2
     ( PI_CONTRACT_NUMBER IN VARCHAR2 )
   RETURN VARCHAR2
   PACKAGE
   IS
    L_CONTRACT_DISTRIBUTE_STATUS BAS_SUBTYPE_PKG.STATUS /*NULL*/;
   BEGIN
        FOR CUR_CONTRACT IN ( SELECT HT.CONTRACT_STATUS
           FROM SAD_CONTRACTS_V HT
          WHERE HT.HTH = PI_CONTRACT_NUMBER )
     LOOP
               IF CUR_CONTRACT.CONTRACT_STATUS = 0 THEN
                  L_CONTRACT_DISTRIBUTE_STATUS := 'Cancel' ;

      ELSE
       L_CONTRACT_DISTRIBUTE_STATUS := BAS_SUBTYPE_PKG.G_HEADER_WAITING_SPLIT_STATUS ;

      END IF ;

        END LOOP ;

        RETURN L_CONTRACT_DISTRIBUTE_STATUS ;
   END ;
   /

NO INDEX CREATED
----------------

If the **INDEX** or **STORAGE** parameter is used in **ALTER TABLE**, delete the parameter. Add constraints to **CREATE TABLE**.

**Input - PRIMARY KEY**

::

   CREATE TABLE CTP_ARM_CONFIG
   ( HOSTNAME VARCHAR2(50),
   OPNAME VARCHAR2(50),
   PARAMTYPE VARCHAR2(2),
   PARAMVALUE NUMBER(*,0),
   MODIFYDATE DATE
   ) SEGMENT CREATION DEFERRED
   PCTFREE 10 PCTUSED 0 INITRANS 1 MAXTRANS 255
   NOCOMPRESS LOGGING
   STORAGE( PCTINCREASE 0
   BUFFER_POOL DEFAULT FLASH_CACHE DEFAULT CELL_FLASH_CACHE DEFAULT)
   TABLESPACE SPMS_DATA ;
   ALTER TABLE CTP_ARM_CONFIG ADD CONSTRAINT PKCTP_ARM_CONFIG PRIMARY KEY
   (HOSTNAME, OPNAME)
   USING INDEX PCTFREE 10 INITRANS 2 MAXTRANS 255 COMPUTE STATISTICS
   STORAGE( PCTINCREASE 0
   BUFFER_POOL DEFAULT FLASH_CACHE DEFAULT CELL_FLASH_CACHE DEFAULT)
   TABLESPACE SPMS_DATA ENABLE;

**Output**

::

   CREATE TABLE
   CTP_ARM_CONFIG (
   HOSTNAME VARCHAR2 (50)
   ,OPNAME VARCHAR2 (50)
   ,PARAMTYPE VARCHAR2 (2)
   ,PARAMVALUE NUMBER (
   38
   ,0
   )
   ,MODIFYDATE DATE
   ,CONSTRAINT PKCTP_ARM_CONFIG PRIMARY KEY (
   HOSTNAME
   ,OPNAME
   )
   ) /*SEGMENT CREATION DEFERRED*/
   /*PCTFREE 10*/
   /*PCTUSED 0*/
   /*INITRANS 1*/
   /*MAXTRANS 255*/
   /*NOCOMPRESS*/
   /*LOGGING*/
   /*STORAGE( BUFFER_POOL DEFAULT FLASH_CACHE DEFAULT CELL_FLASH_CACHE
   DEFAULT)*/
   /*TABLESPACE SPMS_DATA */
   ;

PARTITIONS
----------

Maintenance of large tables and indexes can become very time and resource consuming. At the same time, data access performance can reduce drastically for these objects. Partitioning of tables and indexes can benefit the performance and maintenance in several ways.


.. figure:: /_static/images/en-us_image_0000001658025270.png
   :alt: **Figure 1** Partitioning and sub-partitioning of tables

   **Figure 1** Partitioning and sub-partitioning of tables

DSC supports migration of range partition.

The tool does not support the following partitions/subpartitions and these are commented in the migrated scripts:

-  List partition
-  Hash partition
-  Range subpartition
-  List subpartition
-  Hash subpartition

The unsupported partitions/subpartitions may be supported in the future. Configuration parameters have been provided to enable/disable commenting of the unsupported statements. For details, see :ref:`Configuration Parameters for Oracle Features <en-us_topic_0000001813438716__en-us_topic_0218440495_table15565195515413>`.

-  **PARTITION BY HASH**

   Hash partitioning is a partitioning technique where a hash algorithm is used to distribute rows evenly across the different partitions (sub-tables). This is typically used where ranges are not appropriate, for example employee ID, product ID, and so on. DSC does not support PARTITION and SUBPARTITION by HASH and will comment these statements.

   **Input - HASH PARTITION**

   .. code-block::

      CREATE TABLE dept (deptno NUMBER, deptname VARCHAR(32)) PARTITION BY HASH(deptno) PARTITIONS 16;

   **Output**

   .. code-block::

      CREATE TABLE dept ( deptno NUMBER ,deptname VARCHAR( 32 ) ) /* PARTITION BY HASH(deptno) PARTITIONS 16 */ ;

   **Input - HASH PARTITION without partition names**

   .. code-block::

      CREATE TABLE dept (deptno NUMBER, deptname VARCHAR(32))
            PARTITION BY HASH(deptno) PARTITIONS 16;

   **Output**

   .. code-block::

      CREATE TABLE dept (deptno NUMBER, deptname VARCHAR(32))
        /*    PARTITION BY HASH(deptno) PARTITIONS 16 */;

   **Input - HASH SUBPARTITION**

   ::

      CREATE TABLE sales
         ( prod_id       NUMBER(6)
         , cust_id       NUMBER
         , time_id       DATE
         , channel_id    CHAR(1)
         , promo_id      NUMBER(6)
         , quantity_sold NUMBER(3)
         , amount_sold   NUMBER(10,2)
         )
        PARTITION BY RANGE (time_id) SUBPARTITION BY HASH (cust_id)
         SUBPARTITIONS 8STORE IN (ts1, ts2, ts3, ts4)
        ( PARTITION sales_q1_2006 VALUES LESS THAN (TO_DATE('01-APR-2006','dd-MON-yyyy'))
        , PARTITION sales_q2_2006 VALUES LESS THAN (TO_DATE('01-JUL-2006','dd-MON-yyyy'))
        , PARTITION sales_q3_2006 VALUES LESS THAN (TO_DATE('01-OCT-2006','dd-MON-yyyy'))
        , PARTITION sales_q4_2006 VALUES LESS THAN (TO_DATE('01-JAN-2007','dd-MON-yyyy'))
        );

   **Output**

   ::

      CREATE TABLE sales
         ( prod_id       NUMBER(6)
         , cust_id       NUMBER
         , time_id       DATE
         , channel_id    CHAR(1)
         , promo_id      NUMBER(6)
         , quantity_sold NUMBER(3)
         , amount_sold   NUMBER(10,2)
         )
        PARTITION BY RANGE (time_id) /*SUBPARTITION BY HASH (cust_id)
         SUBPARTITIONS 8  STORE IN (ts1, ts2, ts3, ts4) */
        ( PARTITION sales_q1_2006 VALUES LESS THAN (TO_DATE('01-APR-2006','dd-MON-yyyy'))
        , PARTITION sales_q2_2006 VALUES LESS THAN (TO_DATE('01-JUL-2006','dd-MON-yyyy'))
        , PARTITION sales_q3_2006 VALUES LESS THAN (TO_DATE('01-OCT-2006','dd-MON-yyyy'))
        , PARTITION sales_q4_2006 VALUES LESS THAN (TO_DATE('01-JAN-2007','dd-MON-yyyy'))
        );

-  **PARTITION BY LIST**

   List partitioning is a partitioning technique where you specify a list of discrete values for the partitioning key in the description for each partition. DSC does not support PARTITION and SUBPARTITION by LIST and will comment these statements.

   **Input - LIST PARTITION**

   ::

      CREATE TABLE sales_by_region (item# INTEGER, qty INTEGER, store_name VARCHAR(30), state_code VARCHAR(2), sale_date DATE) STORAGE(INITIAL 10K NEXT 20K) TABLESPACE tbs5 PARTITION BY LIST (state_code) ( PARTITION region_east VALUES ('MA','NY','CT','NH','ME','MD','VA','PA','NJ') STORAGE (INITIAL 8M) TABLESPACE tbs8, PARTITION region_west VALUES ('CA','AZ','NM','OR','WA','UT','NV','CO') NOLOGGING, PARTITION region_south VALUES ('TX','KY','TN','LA','MS','AR','AL','GA'), PARTITION region_central VALUES ('OH','ND','SD','MO','IL','MI','IA'), PARTITION region_null VALUES (NULL), PARTITION region_unknown VALUES (DEFAULT) );

   **Output**

   ::

      CREATE UNLOGGED TABLE sales_by_region ( item# INTEGER ,qty INTEGER ,store_name VARCHAR( 30 ) ,state_code VARCHAR( 2 ) ,sale_date DATE ) TABLESPACE tbs5 /* PARTITION BY LIST(state_code)(PARTITION region_east VALUES('MA','NY','CT','NH','ME','MD','VA','PA','NJ')  TABLESPACE tbs8, PARTITION region_west VALUES('CA','AZ','NM','OR','WA','UT','NV','CO') , PARTITION region_south VALUES('TX','KY','TN','LA','MS','AR','AL','GA'), PARTITION region_central VALUES('OH','ND','SD','MO','IL','MI','IA'), PARTITION region_null VALUES(NULL), PARTITION region_unknown VALUES(DEFAULT) ) */ ;

   **Input - LIST PARTITION** (With Storage Parameters)

   ::

      CREATE TABLE store_master
                ( Store_id NUMBER
                , Store_address VARCHAR2 (40)
                , City VARCHAR2 (30)
                , State VARCHAR2 (2)
                , zip VARCHAR2 (10)
                , manager_id NUMBER
                )
          /*TABLESPACE users*/
          STORAGE ( INITIAL 100 k NEXT 100 k
           PCTINCREASE 0 )
         PARTITION BY LIST (city)
              ( PARTITION south_florida
                VALUES ( 'MIA', 'ORL' )
                /*TABLESPACE users*/
                STORAGE ( INITIAL 100 k NEXT 100
                k PCTINCREASE 0 )
              , PARTITION north_florida
                VALUES ( 'JAC', 'TAM', 'PEN' )
                /*TABLESPACE users*/
                STORAGE ( INITIAL 100 k NEXT 100
                k PCTINCREASE 0 )
              , PARTITION south_georga VALUES
                ( 'BRU', 'WAY', 'VAL' )
                /*TABLESPACE users*/
                STORAGE ( INITIAL 100 k NEXT 100
                k PCTINCREASE 0 )
              , PARTITION north_georgia
                VALUES ( 'ATL', 'SAV', NULL )
              );

   **Output**

   ::

      CREATE TABLE store_master
                ( Store_id NUMBER
                , Store_address VARCHAR2 (40)
                , City VARCHAR2 (30)
                , State VARCHAR2 (2)
                , zip VARCHAR2 (10)
                , manager_id NUMBER
                )
          /*TABLESPACE users*/
          STORAGE ( INITIAL 100 k NEXT 100 k );

   **Input: LIST PARTITION** **TABLE from another TABLE**

   .. code-block::

      CREATE TABLE tab1_list
            PARTITION BY LIST (col1)
               ( partition part1 VALUES ( 1 )
                , partition part2 VALUES ( 2,
                 3, 4 )
                , partition part3 VALUES
                (DEFAULT)
                )
       AS
       SELECT *
         FROM  tab1;

   **Output**

   ::

      CREATE TABLE tab1_list
       AS
       ( SELECT *
         FROM  tab1 );

   **Input - LIST PARTITION** **with SUBPARTITIONS**

   ::

      CREATE TABLE big_t_list PARTITION BY LIST(n10) (partition part1 VALUES (1) ,partition part2 VALUES (2,3,4) ,partition part3 VALUES (DEFAULT)) AS SELECT * FROM big_t;

   **Output**

   ::

      CREATE TABLE big_t_list /* PARTITION BY LIST(n10)(partition part1 VALUES(1) ,partition part2 VALUES(2,3,4) ,partition part3 VALUES(DEFAULT))  */ AS ( SELECT * FROM big_t ) ;

   **Input - LIST PARTITION** **with SUBPARTITION TEMPLATE**

   ::

      CREATE TABLE q1_sales_by_region
                ( deptno NUMBER
                , deptname varchar2 (20)
                , quarterly_sales NUMBER
                (10,2)
                , state varchar2 (2)
                )
         PARTITION BY LIST (state)
               SUBPARTITION BY RANGE
               (quarterly_sales)
               SUBPARTITION TEMPLATE
               ( SUBPARTITION original VALUES
               LESS THAN (1001)
               , SUBPARTITION acquired VALUES
               LESS THAN (8001)
               , SUBPARTITION recent VALUES
               LESS THAN (MAXVALUE)
               )
          ( PARTITION q1_northwest VALUES
           ( 'OR', 'WA' )
          , PARTITION q1_southwest VALUES
           ( 'AZ', 'UT', 'NM' )
          , PARTITION q1_northeast VALUES
           ( 'NY', 'VM', 'NJ' )
          , PARTITION q1_southcentral VALUES
           ( 'OK', 'TX' )
          );

   **Output**

   ::

      CREATE TABLE q1_sales_by_region
                ( deptno NUMBER
                , deptname varchar2 (20)
                , quarterly_sales NUMBER (10,2)
                , state varchar2 (2)
                );

-  **PARTITION BY RANGE**

   Range partitioning is a partitioning technique where data of different ranges is stored separately in different sub-tables. Range partitioning is useful when you have distinct ranges of data you want to store together, for example the date field. DSC supports PARTITION by RANGE. It does not support SUBPARTITION by RANGE and will comment these statements.

   **Input - RANGE PARTITION** (With STORAGE Parameters)

   ::

      CREATE
           TABLE
                CCM_TA550002_H (
                     STRU_ID VARCHAR2 (10)
                     ,ORGAN1_NO VARCHAR2 (10)
                     ,ORGAN2_NO VARCHAR2 (10)
                ) partition BY range (ORGAN2_NO) (
                     partition CCM_TA550002_01
                     VALUES LESS than ('00100') /* TABLESPACE users */
                     /*pctfree 10*/
                     /*initrans 1*/
                     /*storage(initial 256 K NEXT 256 K minextents 1 maxextents unlimited  )*/
                     ,partition CCM_TA550002_02
                     VALUES LESS than ('00200') /* TABLESPACE users */
                     /*pctfree 10*/
                     /*initrans 1*/
                     /* storage ( initial 256 K NEXT
      256K  minextents 1
      maxextents unlimited
      pctincrease 0 )*/

   **Output**

   ::

      CREATE TABLE CCM_TA550002_H
                ( STRU_ID VARCHAR2 (10)
                , ORGAN1_NO VARCHAR2 (10)
                , ORGAN2_NO VARCHAR2 (10)
                )
          partition BY range (ORGAN2_NO)
                   ( partition CCM_TA550002_01 VALUES LESS
                     than ('00100')
                     /*TABLESPACE users*/
                   , partition CCM_TA550002_02 VALUES LESS
                     than ('00200')
                     /*TABLESPACE users*/
                   );

   **Input - RANGE PARTITION** **with SUBPARTITIONS**

   ::

      CREATE TABLE composite_rng_list (
      cust_id     NUMBER(10),
      cust_name   VARCHAR2(25),
      cust_state  VARCHAR2(2),
      time_id     DATE)
      PARTITION BY RANGE(time_id)
      SUBPARTITION BY LIST (cust_state)
      SUBPARTITION TEMPLATE(
      SUBPARTITION west VALUES ('OR', 'WA') TABLESPACE part1,
      SUBPARTITION east VALUES ('NY', 'CT') TABLESPACE part2,
      SUBPARTITION cent VALUES ('OK', 'TX') TABLESPACE part3) (
      PARTITION per1 VALUES LESS THAN (TO_DATE('01/01/2000','DD/MM/YYYY')),
      PARTITION per2 VALUES LESS THAN (TO_DATE('01/01/2005','DD/MM/YYYY')),
      PARTITION per3 VALUES LESS THAN (TO_DATE('01/01/2010','DD/MM/YYYY')),
      PARTITION future VALUES LESS THAN(MAXVALUE));

   **Output**

   ::

      CREATE TABLE composite_rng_list (
      cust_id     NUMBER(10),
      cust_name   VARCHAR2(25),
      cust_state  VARCHAR2(2),
      time_id     DATE)
      PARTITION BY RANGE(time_id)
      /*SUBPARTITION BY LIST (cust_state)
      SUBPARTITION TEMPLATE(
      SUBPARTITION west VALUES ('OR', 'WA') TABLESPACE part1,
      SUBPARTITION east VALUES ('NY', 'CT') TABLESPACE part2,
      SUBPARTITION cent VALUES ('OK', 'TX') TABLESPACE part3)*/ (
      PARTITION per1 VALUES LESS THAN (TO_DATE('01/01/2000','DD/MM/YYYY')),
      PARTITION per2 VALUES LESS THAN (TO_DATE('01/01/2005','DD/MM/YYYY')),
      PARTITION per3 VALUES LESS THAN (TO_DATE('01/01/2010','DD/MM/YYYY')),
      PARTITION future VALUES LESS THAN(MAXVALUE));

   **Input - RANGE PARTITION** **with SUBPARTITION TEMPLATE**

   ::

      CREATE TABLE composite_rng_rng (
      cust_id     NUMBER(10),
      cust_name   VARCHAR2(25),
      cust_state  VARCHAR2(2),
      time_id     DATE)
      PARTITION BY RANGE(time_id)
      SUBPARTITION BY RANGE (cust_id)
      SUBPARTITION TEMPLATE(
      SUBPARTITION original VALUES LESS THAN (1001) TABLESPACE part1,
      SUBPARTITION acquired VALUES LESS THAN (8001) TABLESPACE part2,
      SUBPARTITION recent VALUES LESS THAN (MAXVALUE) TABLESPACE part3) (
      PARTITION per1 VALUES LESS THAN (TO_DATE('01/01/2000','DD/MM/YYYY')),
      PARTITION per2 VALUES LESS THAN (TO_DATE('01/01/2005','DD/MM/YYYY')),
      PARTITION per3 VALUES LESS THAN (TO_DATE('01/01/2010','DD/MM/YYYY')),
      PARTITION future VALUES LESS THAN (MAXVALUE));

   **Output**

   ::

      CREATE TABLE composite_rng_rng (
      cust_id     NUMBER(10),
      cust_name   VARCHAR2(25),
      cust_state  VARCHAR2(2),
      time_id     DATE)
      PARTITION BY RANGE(time_id)
      /*SUBPARTITION BY RANGE (cust_id)
      SUBPARTITION TEMPLATE(
      SUBPARTITION original VALUES LESS THAN (1001) TABLESPACE part1,
      SUBPARTITION acquired VALUES LESS THAN (8001) TABLESPACE part2,
      SUBPARTITION recent VALUES LESS THAN (MAXVALUE) TABLESPACE part3)*/ (
      PARTITION per1 VALUES LESS THAN (TO_DATE('01/01/2000','DD/MM/YYYY')),
      PARTITION per2 VALUES LESS THAN (TO_DATE('01/01/2005','DD/MM/YYYY')),
      PARTITION per3 VALUES LESS THAN (TO_DATE('01/01/2010','DD/MM/YYYY')),
      PARTITION future VALUES LESS THAN (MAXVALUE));

   **PRIMARY KEY/UNIQUE Constraint for Partitioned Table**

   If the CREATE TABLE statement contains range/hash/list partitioning, the following error is reported:

   Invalid PRIMARY KEY/UNIQUE constraint for partitioned table

   Note: Columns of the PRIMARY KEY/UNIQUE constraint must contain PARTITION KEY.

   Scripts: wo_integrate_log_t.sql, wo_change_log_t.sql

   **Input:**

   ::

      CREATE TABLE SD_WO.WO_INTEGRATE_LOG_T
      (
      LOG_ID            NUMBER not null,
      PROJECT_NUMBER    VARCHAR2(40),
      MESSAGE_ID        VARCHAR2(100),
      BUSINESS_ID       VARCHAR2(100),
      BUSINESS_TYPE     VARCHAR2(100),
      INTEGRATE_CONTENT CLOB,
      OPERATION_RESULT  VARCHAR2(100),
      FAILED_MSG        VARCHAR2(4000),
      HOST_NAME         VARCHAR2(100) not null,
      CREATED_BY        NUMBER not null,
      CREATION_DATE     DATE not null,
      LAST_UPDATED_BY   NUMBER not null,
      LAST_UPDATE_DATE  DATE not null,
      SOURCE_CODE       VARCHAR2(100),
      TENANT_ID         NUMBER
      )
      partition by range (CREATION_DATE)
      (
      partition P2018 values less than (TO_DATE(' 2018-10-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS', 'NLS_CALENDAR=GREGORIAN'))
      tablespace SDWO_DATA,
      partition SYS_P53873 values less than (TO_DATE(' 2018-11-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS', 'NLS_CALENDAR=GREGORIAN'))
      tablespace SDWO_DATA,
      partition SYS_P104273 values less than (TO_DATE(' 2018-12-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS', 'NLS_CALENDAR=GREGORIAN'))
      tablespace SDWO_DATA,
      partition SYS_P105533 values less than (TO_DATE(' 2019-01-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS', 'NLS_CALENDAR=GREGORIAN'))
      tablespace SDWO_DATA,
      partition SYS_P108153 values less than (TO_DATE(' 2019-02-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS', 'NLS_CALENDAR=GREGORIAN'))
      tablespace SDWO_DATA,
      partition SYS_P127173 values less than (TO_DATE(' 2019-03-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS', 'NLS_CALENDAR=GREGORIAN'))
      tablespace SDWO_DATA,
      partition SYS_P130313 values less than (TO_DATE(' 2019-04-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS', 'NLS_CALENDAR=GREGORIAN'))
      tablespace SDWO_DATA
      );
      alter table SD_WO.WO_INTEGRATE_LOG_T
      add constraint WO_INTEGRATE_LOG_PK primary key (LOG_ID);
      create index SD_WO.WO_INTEGRATE_LOG_N1 on SD_WO.WO_INTEGRATE_LOG_T (BUSINESS_ID);
      create index SD_WO.WO_INTEGRATE_LOG_N2 on SD_WO.WO_INTEGRATE_LOG_T (CREATION_DATE, BUSINESS_TYPE);
      create index SD_WO.WO_INTEGRATE_LOG_N3 on SD_WO.WO_INTEGRATE_LOG_T (PROJECT_NUMBER, BUSINESS_TYPE);

   **Output:**

   ::

      CREATE TABLE
      SD_WO.WO_INTEGRATE_LOG_T (
      LOG_ID NUMBER NOT NULL
      ,PROJECT_NUMBER VARCHAR2 (40)
      ,MESSAGE_ID VARCHAR2 (100)
      ,BUSINESS_ID VARCHAR2 (100)
      ,BUSINESS_TYPE VARCHAR2 (100)
      ,INTEGRATE_CONTENT CLOB
      ,OPERATION_RESULT VARCHAR2 (100)
      ,FAILED_MSG VARCHAR2 (4000)
      ,HOST_NAME VARCHAR2 (100) NOT NULL
      ,CREATED_BY NUMBER NOT NULL
      ,CREATION_DATE DATE NOT NULL
      ,LAST_UPDATED_BY NUMBER NOT NULL
      ,LAST_UPDATE_DATE DATE NOT NULL
      ,SOURCE_CODE VARCHAR2 (100)
      ,TENANT_ID NUMBER
      ,CONSTRAINT WO_INTEGRATE_LOG_PK PRIMARY KEY (LOG_ID)
      ) partition BY range (CREATION_DATE) (
      partition P2018
      VALUES LESS than (
      TO_DATE( ' 2018-10-01 00:00:00' ,'SYYYY-MM-DD HH24:MI:SS'/*, 'NLS_CALENDAR=GREGORIAN'*/ )
      ) /* tablespace SDWO_DATA */
      ,partition SYS_P53873
      VALUES LESS than (
      TO_DATE( ' 2018-11-01 00:00:00' ,'SYYYY-MM-DD HH24:MI:SS'/*, 'NLS_CALENDAR=GREGORIAN'*/ )
      ) /* tablespace SDWO_DATA */
      ,partition SYS_P104273
      VALUES LESS than (
      TO_DATE( ' 2018-12-01 00:00:00' ,'SYYYY-MM-DD HH24:MI:SS'/*, 'NLS_CALENDAR=GREGORIAN'*/ )
      ) /* tablespace SDWO_DATA */
      ,partition SYS_P105533
      VALUES LESS than (
      TO_DATE( ' 2019-01-01 00:00:00' ,'SYYYY-MM-DD HH24:MI:SS'/*, 'NLS_CALENDAR=GREGORIAN'*/ )
      ) /* tablespace SDWO_DATA */
      ,partition SYS_P108153
      VALUES LESS than (
      TO_DATE( ' 2019-02-01 00:00:00' ,'SYYYY-MM-DD HH24:MI:SS'/*, 'NLS_CALENDAR=GREGORIAN'*/ )
      ) /* tablespace SDWO_DATA */
      ,partition SYS_P127173
      VALUES LESS than (
      TO_DATE( ' 2019-03-01 00:00:00' ,'SYYYY-MM-DD HH24:MI:SS'/*, 'NLS_CALENDAR=GREGORIAN'*/ )
      ) /* tablespace SDWO_DATA */
      ,partition SYS_P130313
      VALUES LESS than (
      TO_DATE( ' 2019-04-01 00:00:00' ,'SYYYY-MM-DD HH24:MI:SS'/*, 'NLS_CALENDAR=GREGORIAN'*/ )
      ) /* tablespace SDWO_DATA */
      ) ;
      CREATE
      index WO_INTEGRATE_LOG_N1
      ON SD_WO.WO_INTEGRATE_LOG_T (BUSINESS_ID) LOCAL ;
      CREATE
      index WO_INTEGRATE_LOG_N2
      ON SD_WO.WO_INTEGRATE_LOG_T (
      CREATION_DATE
      ,BUSINESS_TYPE
      ) LOCAL ;
      CREATE
      index WO_INTEGRATE_LOG_N3
      ON SD_WO.WO_INTEGRATE_LOG_T (
      PROJECT_NUMBER
      ,BUSINESS_TYPE
      ) LOCAL ;

   **Input**:

   ::

      CREATE TABLE SD_WO.WO_INTEGRATE_LOG_T
      (
        LOG_ID            NUMBER not null,
        PROJECT_NUMBER    VARCHAR2(40),
        MESSAGE_ID        VARCHAR2(100),
        BUSINESS_ID       VARCHAR2(100),
        BUSINESS_TYPE     VARCHAR2(100),
        INTEGRATE_CONTENT CLOB,
        OPERATION_RESULT  VARCHAR2(100),
        FAILED_MSG        VARCHAR2(4000),
        HOST_NAME         VARCHAR2(100) not null,
        CREATED_BY        NUMBER not null,
        CREATION_DATE     DATE not null,
        LAST_UPDATED_BY   NUMBER not null,
        LAST_UPDATE_DATE  DATE not null,
        SOURCE_CODE       VARCHAR2(100),
        TENANT_ID         NUMBER
      )
      partition by range (CREATION_DATE)
      (
        partition P2018 values less than (TO_DATE(' 2018-10-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS', 'NLS_CALENDAR=GREGORIAN'))
          tablespace SDWO_DATA,
        partition SYS_P53873 values less than (TO_DATE(' 2018-11-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS', 'NLS_CALENDAR=GREGORIAN'))
          tablespace SDWO_DATA,
        partition SYS_P104273 values less than (TO_DATE(' 2018-12-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS', 'NLS_CALENDAR=GREGORIAN'))
          tablespace SDWO_DATA,
        partition SYS_P105533 values less than (TO_DATE(' 2019-01-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS', 'NLS_CALENDAR=GREGORIAN'))
          tablespace SDWO_DATA,
        partition SYS_P108153 values less than (TO_DATE(' 2019-02-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS', 'NLS_CALENDAR=GREGORIAN'))
          tablespace SDWO_DATA,
        partition SYS_P127173 values less than (TO_DATE(' 2019-03-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS', 'NLS_CALENDAR=GREGORIAN'))
          tablespace SDWO_DATA,
        partition SYS_P130313 values less than (TO_DATE(' 2019-04-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS', 'NLS_CALENDAR=GREGORIAN'))
          tablespace SDWO_DATA
      );

      alter table SD_WO.WO_INTEGRATE_LOG_T
        add constraint WO_INTEGRATE_LOG_PK primary key (LOG_ID);
      create index SD_WO.WO_INTEGRATE_LOG_N1 on SD_WO.WO_INTEGRATE_LOG_T (BUSINESS_ID);
      create index SD_WO.WO_INTEGRATE_LOG_N2 on SD_WO.WO_INTEGRATE_LOG_T (CREATION_DATE, BUSINESS_TYPE);
      create index SD_WO.WO_INTEGRATE_LOG_N3 on SD_WO.WO_INTEGRATE_LOG_T (PROJECT_NUMBER, BUSINESS_TYPE);

   **Output**:

   ::

      CREATE TABLE SD_WO.WO_INTEGRATE_LOG_T
      (
        LOG_ID            NUMBER not null,
        PROJECT_NUMBER    VARCHAR2(40),
        MESSAGE_ID        VARCHAR2(100),
        BUSINESS_ID       VARCHAR2(100),
        BUSINESS_TYPE     VARCHAR2(100),
        INTEGRATE_CONTENT CLOB,
        OPERATION_RESULT  VARCHAR2(100),
        FAILED_MSG        VARCHAR2(4000),
        HOST_NAME         VARCHAR2(100) not null,
        CREATED_BY        NUMBER not null,
        CREATION_DATE     DATE not null,
        LAST_UPDATED_BY   NUMBER not null,
        LAST_UPDATE_DATE  DATE not null,
        SOURCE_CODE       VARCHAR2(100),
        TENANT_ID         NUMBER
      )
      partition by range (CREATION_DATE)
      (
        partition P2018 values less than (TO_DATE(' 2018-10-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS', 'NLS_CALENDAR=GREGORIAN'))
          tablespace SDWO_DATA,
        partition SYS_P53873 values less than (TO_DATE(' 2018-11-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS', 'NLS_CALENDAR=GREGORIAN'))
          tablespace SDWO_DATA,
        partition SYS_P104273 values less than (TO_DATE(' 2018-12-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS', 'NLS_CALENDAR=GREGORIAN'))
          tablespace SDWO_DATA,
        partition SYS_P105533 values less than (TO_DATE(' 2019-01-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS', 'NLS_CALENDAR=GREGORIAN'))
          tablespace SDWO_DATA,
        partition SYS_P108153 values less than (TO_DATE(' 2019-02-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS', 'NLS_CALENDAR=GREGORIAN'))
          tablespace SDWO_DATA,
        partition SYS_P127173 values less than (TO_DATE(' 2019-03-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS', 'NLS_CALENDAR=GREGORIAN'))
          tablespace SDWO_DATA,
        partition SYS_P130313 values less than (TO_DATE(' 2019-04-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS', 'NLS_CALENDAR=GREGORIAN'))
          tablespace SDWO_DATA
      );

      ALTER TABLE SD_WO.WO_INTEGRATE_LOG_T
        add constraint WO_INTEGRATE_LOG_PK primary key (LOG_ID);
      create index SD_WO.WO_INTEGRATE_LOG_N1 on SD_WO.WO_INTEGRATE_LOG_T (BUSINESS_ID);
      create index SD_WO.WO_INTEGRATE_LOG_N2 on SD_WO.WO_INTEGRATE_LOG_T (CREATION_DATE, BUSINESS_TYPE);
      create index SD_WO.WO_INTEGRATE_LOG_N3 on SD_WO.WO_INTEGRATE_LOG_T (PROJECT_NUMBER, BUSINESS_TYPE);

Data Type
---------

Remove the BYTE keyword from the data type.

+-----------------------------------+-----------------------------------+
| Oracle Syntax                     | Syntax After Migration            |
+===================================+===================================+
| ::                                | ::                                |
|                                   |                                   |
|    CREATE TABLE TBL_ORACLE        |    CREATE TABLE  TBL_ORACLE       |
|     (                             |     (                             |
|       ID     Number,              |         ID NUMBER                 |
|       Name   VARCHAR2(100 BYTE),  |         ,Name VARCHAR2 (100)      |
|      ADDRESS VARCHAR2(200 BYTE)   |         ,ADDRESS VARCHAR2 (200)   |
|      );                           |     ) ;                           |
+-----------------------------------+-----------------------------------+

Partition (Comment Partition)
-----------------------------

In configuration parameter for oracle "#Unique or primary key constraint for partitioned table" to comment_partition.

+----------------------------------------------------------------------+---------------------------------------------------------------------------------------------------+
| Oracle Syntax                                                        | Syntax After Migration                                                                            |
+======================================================================+===================================================================================================+
| ::                                                                   | ::                                                                                                |
|                                                                      |                                                                                                   |
|    CREATE TABLE TBL_ORACLE                                           |    CREATE UNLOGGED TABLE  TBL_ORACLE                                                              |
|    (                                                                 |    (                                                                                              |
|       ID     Number,                                                 |                   ID NUMBER                                                                       |
|       Name   VARCHAR2(100 BYTE),                                     |                   ,Name VARCHAR2 (100)                                                            |
|      ADDRESS VARCHAR2(200 BYTE)                                      |                   ,ADDRESS VARCHAR2 (200)                                                         |
|      )                                                               |                   ,CONSTRAINT SAMPLE_PK PRIMARY KEY (ID)                                          |
|    TABLESPACE space1                                                 |    )                                                                                              |
|    PCTUSED    40                                                     |     TABLESPACE space1                                                                             |
|    PCTFREE    0                                                      |              /*PCTUSED 40*/                                                                       |
|    INITRANS   1                                                      |              PCTFREE 0                                                                            |
|    MAXTRANS   255                                                    |              INITRANS 1                                                                           |
|    NOLOGGING                                                         |              MAXTRANS 255                                                                         |
|    PARTITION BY RANGE (ID)                                           |     /* PARTITION BY RANGE(ID)(PARTITION PART_2010 VALUES LESS THAN(10) ,                          |
|    (                                                                 |    PARTITION PART_2011 VALUES LESS THAN(20)  , PARTITION PART_2012 VALUES LESS THAN(MAXVALUE)  )  |
|      PARTITION PART_2010 VALUES LESS THAN (10)                       |    ENABLE ROW MOVEMENT */                                                                         |
|        NOLOGGING,                                                    |     ;                                                                                             |
|      PARTITION PART_2011 VALUES LESS THAN (20)                       |                                                                                                   |
|        NOLOGGING ,                                                   |                                                                                                   |
|      PARTITION PART_2012 VALUES LESS THAN (MAXVALUE)                 |                                                                                                   |
|        NOLOGGING                                                     |                                                                                                   |
|    )                                                                 |                                                                                                   |
|    ENABLE ROW MOVEMENT;                                              |                                                                                                   |
|                                                                      |                                                                                                   |
|                                                                      |                                                                                                   |
|    ALTER TABLE TBL_ORACLE ADD CONSTRAINT SAMPLE_PK PRIMARY KEY (ID); |                                                                                                   |
+----------------------------------------------------------------------+---------------------------------------------------------------------------------------------------+

Partition (Comment Constraint)
------------------------------

In configuration parameter for oracle "#Unique or primary key constraint for partitioned table" to comment_unique.

+----------------------------------------------------------------------+-----------------------------------------------------------+
| Oracle Syntax                                                        | Syntax After Migration                                    |
+======================================================================+===========================================================+
| ::                                                                   | ::                                                        |
|                                                                      |                                                           |
|    CREATE TABLE TBL_ORACLE                                           |    CREATE UNLOGGED TABLE TBL_ORACLE                       |
|    (                                                                 |    (                                                      |
|       ID     Number,                                                 |                   ID NUMBER                               |
|       Name   VARCHAR2(100 BYTE),                                     |                   ,Name VARCHAR2 (100)                    |
|      ADDRESS VARCHAR2(200 BYTE)                                      |                   ,ADDRESS VARCHAR2 (200)                 |
|      )                                                               |    /*,CONSTRAINT SAMPLE_PK PRIMARY KEY (ID)*/             |
|    TABLESPACE space1                                                 |    )                                                      |
|    PCTUSED    40                                                     |     TABLESPACE space1                                     |
|    PCTFREE    0                                                      |              /*PCTUSED 40*/                               |
|    INITRANS   1                                                      |              PCTFREE 0                                    |
|    MAXTRANS   255                                                    |              INITRANS 1                                   |
|    NOLOGGING                                                         |              MAXTRANS 255                                 |
|    PARTITION BY RANGE (ID)                                           |    PARTITION BY RANGE (ID)                                |
|    (                                                                 |    (                                                      |
|      PARTITION PART_2010 VALUES LESS THAN (10)                       |          PARTITION PART_2010  VALUES LESS THAN (10)       |
|        NOLOGGING,                                                    |         ,PARTITION PART_2011  VALUES LESS THAN (20)       |
|      PARTITION PART_2011 VALUES LESS THAN (20)                       |         ,PARTITION PART_2012  VALUES LESS THAN (MAXVALUE) |
|        NOLOGGING ,                                                   |              ) ENABLE ROW MOVEMENT ;                      |
|      PARTITION PART_2012 VALUES LESS THAN (MAXVALUE)                 |                                                           |
|        NOLOGGING                                                     |                                                           |
|    )                                                                 |                                                           |
|    ENABLE ROW MOVEMENT;                                              |                                                           |
|                                                                      |                                                           |
|                                                                      |                                                           |
|    ALTER TABLE TBL_ORACLE ADD CONSTRAINT SAMPLE_PK PRIMARY KEY (ID); |                                                           |
+----------------------------------------------------------------------+-----------------------------------------------------------+

Partition (I)
-------------

Comment ALTER TABLE TRUNCATE PARTITION for non-partitioned tables.

+--------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------+
| Oracle Syntax                                                                                                | Syntax After Migration                                                                                       |
+==============================================================================================================+==============================================================================================================+
| ::                                                                                                           | ::                                                                                                           |
|                                                                                                              |                                                                                                              |
|    CREATE TABLE product_range                                                                                |    CREATE TABLE product_range                                                                                |
|    (                                                                                                         |    (                                                                                                         |
|      product_id      VARCHAR2(20),                                                                           |      product_id      VARCHAR2(20),                                                                           |
|      Product_Name    VARCHAR2(50),                                                                           |      Product_Name    VARCHAR2(50),                                                                           |
|      Year_Manufacture DATE                                                                                   |      Year_Manufacture DATE                                                                                   |
|    )                                                                                                         |    )                                                                                                         |
|    partition by range (Year_Manufacture)                                                                     |    partition by range (Year_Manufacture)                                                                     |
|    (                                                                                                         |    (                                                                                                         |
|      partition Year_Manufacture values less than (TO_DATE(' 2007-01-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS')) |      partition Year_Manufacture values less than (TO_DATE(' 2007-01-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS')) |
|        pctfree 10                                                                                            |        pctfree 10                                                                                            |
|        initrans 1                                                                                            |        initrans 1                                                                                            |
|    );                                                                                                        |    );                                                                                                        |
|                                                                                                              |                                                                                                              |
|    CREATE TABLE product_list                                                                                 |    CREATE TABLE product_list                                                                                 |
|    (                                                                                                         |    (                                                                                                         |
|      product_id      VARCHAR2(20),                                                                           |      product_id      VARCHAR2(20),                                                                           |
|      Product_Name    VARCHAR2(50),                                                                           |      Product_Name    VARCHAR2(50),                                                                           |
|      Year_Manufacture vARCHAR2(10)                                                                           |      Year_Manufacture vARCHAR2(10)                                                                           |
|    )                                                                                                         |    )                                                                                                         |
|    partition by list (Year_Manufacture)                                                                      |    /*partition by list (Year_Manufacture)                                                                    |
|    (                                                                                                         |    (                                                                                                         |
|      partition P_2020 VALUES (2020)                                                                          |      partition P_2020 VALUES (2020)                                                                          |
|        pctfree 10                                                                                            |        pctfree 10                                                                                            |
|        initrans 1                                                                                            |        initrans 1                                                                                            |
|    );                                                                                                        |    )*/;                                                                                                      |
|                                                                                                              |                                                                                                              |
|                                                                                                              |                                                                                                              |
|    CREATE OR REPLACE PROCEDURE Range_test                                                                    |    CREATE OR REPLACE PROCEDURE Range_test                                                                    |
|    IS                                                                                                        |    IS                                                                                                        |
|    V_ID VARCHAR2(10);                                                                                        |    V_ID VARCHAR2(10);                                                                                        |
|    BEGIN                                                                                                     |    BEGIN                                                                                                     |
|    EXECUTE IMMEDIATE 'ALTER TABLE product TRUNCATE PARTITION PART'||V_ID;                                    |    EXECUTE IMMEDIATE 'ALTER TABLE product TRUNCATE PARTITION PART'||V_ID;                                    |
|    NULL;                                                                                                     |    NULL;                                                                                                     |
|    END;                                                                                                      |    END;                                                                                                      |
|    /                                                                                                         |    /                                                                                                         |
|    CREATE OR REPLACE PROCEDURE List_test                                                                     |    CREATE OR REPLACE PROCEDURE List_test                                                                     |
|    IS                                                                                                        |    IS                                                                                                        |
|    V_ID VARCHAR2(10);                                                                                        |    V_ID VARCHAR2(10);                                                                                        |
|    BEGIN                                                                                                     |    BEGIN                                                                                                     |
|    EXECUTE IMMEDIATE 'ALTER TABLE product TRUNCATE PARTITION PART'||V_ID;                                    |    /*EXECUTE IMMEDIATE 'ALTER TABLE product TRUNCATE PARTITION PART'||V_ID;*/                                |
|    NULL;                                                                                                     |    NULL;                                                                                                     |
|    END;                                                                                                      |    END;                                                                                                      |
|    /                                                                                                         |    /                                                                                                         |
+--------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------+

Partition (II)
--------------

Delete data for ALTER TABLE TRUNCATE PARTITION for non-partitioned tables.

+------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------+
| Oracle Syntax                                                                      | Syntax After Migration                                                                                             |
+====================================================================================+====================================================================================================================+
| ::                                                                                 | ::                                                                                                                 |
|                                                                                    |                                                                                                                    |
|    CREATE TABLE product_list                                                       |    CREATE TABLE product_list                                                                                       |
|    (                                                                               |    (                                                                                                               |
|      product_id      VARCHAR2(20),                                                 |      product_id      VARCHAR2(20),                                                                                 |
|      Product_Name    VARCHAR2(50),                                                 |      Product_Name    VARCHAR2(50),                                                                                 |
|      Year_Manufacture vARCHAR2(10)                                                 |      Year_Manufacture vARCHAR2(10)                                                                                 |
|    )                                                                               |    )                                                                                                               |
|    partition by list (Year_Manufacture)                                            |    /*partition by list (Year_Manufacture)                                                                          |
|    ( partition PART_2015 VALUES (2011,2012,2013,2014,2015) pctfree 10 initrans 1 , |    ( partition PART_2015 VALUES (2011,2012,2013,2014,2015) pctfree 10 initrans 1 ,                                 |
|     partition PART_2016 VALUES (2016) pctfree 10 initrans 1 ,                      |     partition PART_2016 VALUES (2016) pctfree 10 initrans 1 ,                                                      |
|     partition PART_2017 VALUES (2017) pctfree 10 initrans 1 ,                      |     partition PART_2017 VALUES (2017) pctfree 10 initrans 1 ,                                                      |
|     partition PART_2018 VALUES (2018) pctfree 10 initrans 1 ,                      |     partition PART_2018 VALUES (2018) pctfree 10 initrans 1 ,                                                      |
|     partition PART_2019 VALUES (2019) pctfree 10 initrans 1 ,                      |     partition PART_2019 VALUES (2019) pctfree 10 initrans 1 ,                                                      |
|     partition PART_2020 VALUES (2020) pctfree 10 initrans 1 ,                      |     partition PART_2020 VALUES (2020) pctfree 10 initrans 1 ,                                                      |
|     PARTITION PART_unknown VALUES (DEFAULT) );                                     |     PARTITION PART_unknown VALUES (DEFAULT) )*/;                                                                   |
|                                                                                    |                                                                                                                    |
|    CREATE OR REPLACE PROCEDURE List_test                                           |                                                                                                                    |
|    IS                                                                              |    CREATE OR REPLACE PROCEDURE List_test                                                                           |
|    V_ID VARCHAR2(10);                                                              |    IS                                                                                                              |
|    BEGIN                                                                           |    V_ID VARCHAR2(10);                                                                                              |
|     EXECUTE IMMEDIATE 'ALTER TABLE product_list TRUNCATE PARTITION PART_2020;      |    BEGIN                                                                                                           |
|     NULL;                                                                          |     EXECUTE IMMEDIATE 'ALTER TABLE product_list TRUNCATE PARTITION PART_' || V_ID;                                 |
|    END;                                                                            |     NULL;                                                                                                          |
|    /                                                                               |    END;                                                                                                            |
|                                                                                    |    /                                                                                                               |
|    CREATE OR REPLACE PROCEDURE List_test                                           |                                                                                                                    |
|    IS                                                                              |    CREATE OR REPLACE PROCEDURE List_test                                                                           |
|    V_ID VARCHAR2(10);                                                              |    IS                                                                                                              |
|    BEGIN                                                                           |    V_ID VARCHAR2(10);                                                                                              |
|     EXECUTE IMMEDIATE 'ALTER TABLE product_list TRUNCATE PARTITION PART_' || V_ID; |    BEGIN                                                                                                           |
|     NULL;                                                                          |     /* EXECUTE IMMEDIATE 'ALTER TABLE product_list TRUNCATE PARTITION PART_' || V_ID; */                           |
|    END;                                                                            |     IF 'PART_' || V_ID = 'PART_2015' THEN                                                                          |
|    /                                                                               |        DELETE FROM product_list WHERE Year_Manufacture IN (2011,2012,2013,2014,2015);                              |
|                                                                                    |     ELSIF 'PART_' || V_ID = 'PART_2016' THEN                                                                       |
|                                                                                    |        DELETE FROM product_list WHERE Year_Manufacture IN (2016);                                                  |
|                                                                                    |     ELSIF 'PART_' || V_ID = 'PART_2017' THEN                                                                       |
|                                                                                    |        DELETE FROM product_list WHERE Year_Manufacture IN (2017);                                                  |
|                                                                                    |     ELSIF 'PART_' || V_ID = 'PART_2018' THEN                                                                       |
|                                                                                    |        DELETE FROM product_list WHERE Year_Manufacture IN (2018);                                                  |
|                                                                                    |     ELSIF 'PART_' || V_ID = 'PART_2019' THEN                                                                       |
|                                                                                    |        DELETE FROM product_list WHERE Year_Manufacture IN (2019);                                                  |
|                                                                                    |     ELSIF 'PART_' || V_ID = 'PART_2020' THEN                                                                       |
|                                                                                    |        DELETE FROM product_list WHERE Year_Manufacture IN (2020);                                                  |
|                                                                                    |     ELSE                                                                                                           |
|                                                                                    |        DELETE FROM product_list WHERE Year_Manufacture NOT IN (2011,2012,2013,2014,2015,2016,2017,2018,2019,2020); |
|                                                                                    |     END IF;                                                                                                        |
|                                                                                    |     NULL;                                                                                                          |
|                                                                                    |    END;                                                                                                            |
|                                                                                    |    /                                                                                                               |
+------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------+

SEGMENT CREATION
----------------

**SEGMENT CREATION { IMMEDIATE \| DEFERRED }** is not supported in GaussDB(DWS), hence it is commented out in the migrated output. You need to set **commentStorageParameter** to **true**.

**Input - TABLE with** **SEGMENT CREATION**

::

   CREATE TABLE T1
      ( MESSAGE_CODE VARCHAR2(50),
    MAIL_TITLE VARCHAR2(1000),
    MAIL_BODY VARCHAR2(1000),
    MAIL_ADDRESS VARCHAR2(1000),
    MAIL_ADDRESS_CC VARCHAR2(1000)
      ) SEGMENT CREATION DEFERRED
     PCTFREE 10 PCTUSED 0 INITRANS 1 MAXTRANS 255
    NOCOMPRESS LOGGING
     STORAGE( INITIAL 65536 NEXT 1048576 MINEXTENTS 1 MAXEXTENTS 2147483645
     PCTINCREASE 0
     BUFFER_POOL DEFAULT FLASH_CACHE DEFAULT CELL_FLASH_CACHE DEFAULT)
     TABLESPACE Test ;

**Output**

::

   CREATE TABLE T1
      ( MESSAGE_CODE VARCHAR2(50),
    MAIL_TITLE VARCHAR2(1000),
    MAIL_BODY VARCHAR2(1000),
    MAIL_ADDRESS VARCHAR2(1000),
    MAIL_ADDRESS_CC VARCHAR2(1000)
      ) /*SEGMENT CREATION DEFERRED */
     /*PCTFREE 10*/
   /* PCTUSED 0 */
   /*INITRANS 1 */
   /*MAXTRANS 255 */
   /* NOCOMPRESS LOGGING*/
   /*  STORAGE( INITIAL 65536 NEXT 1048576 MINEXTENTS 1 MAXEXTENTS 2147483645
     PCTINCREASE 0
     BUFFER_POOL DEFAULT FLASH_CACHE DEFAULT CELL_FLASH_CACHE DEFAULT)*/
   /*  TABLESPACE Test */;

STORAGE
-------

Storage parameters including **BUFFER_POOL** and **MAXEXTENTS** are not supported in GaussDB(DWS). If **comment_storage_parameter** is set to **TRUE**, these parameters that appear in tables or indexes will be commented out during migration.

**Input - TABLE with** **STORAGE**

::

   CREATE UNIQUE INDEX PK_BASE_APPR_STEP_DEF ON BASE_APPR_STEP_DEF (FLOW_ID, NODE_ID, STEP_ID)
      PCTFREE 10 INITRANS 2 MAXTRANS 255 COMPUTE STATISTICS
      STORAGE(INITIAL 65536 NEXT 1048576 MINEXTENTS 1 MAXEXTENTS 2147483645
      PCTINCREASE 0 FREELISTS 1 FREELIST GROUPS 1
      BUFFER_POOL DEFAULT FLASH_CACHE DEFAULT CELL_FLASH_CACHE DEFAULT)
      TABLESPACE SPMS_DATA ;

    CREATE TABLE UFP_MAIL
       ( MAIL_ID NUMBER(*,0),
     MAIL_TITLE VARCHAR2(1000),
     MAIL_BODY VARCHAR2(4000),
     STATUS VARCHAR2(50),
     CREATE_TIME DATE,
     SEND_TIME DATE,
     MAIL_ADDRESS CLOB,
     MAIL_CC CLOB,
     BASE_ID VARCHAR2(20),
     BASE_STATUS VARCHAR2(50),
     BASE_VERIFY VARCHAR2(20),
     BASE_LINK VARCHAR2(4000),
     MAIL_TYPE VARCHAR2(20),
     BLIND_COPY_TO CLOB,
     FILE_NAME VARCHAR2(4000),
     FULL_FILEPATH VARCHAR2(4000)
       ) SEGMENT CREATION IMMEDIATE
      PCTFREE 10 PCTUSED 0 INITRANS 1 MAXTRANS 255
     NOCOMPRESS LOGGING
      STORAGE(INITIAL 65536 NEXT 1048576 MINEXTENTS 1 MAXEXTENTS 2147483645
      PCTINCREASE 0 FREELISTS 1 FREELIST GROUPS 1
      BUFFER_POOL DEFAULT FLASH_CACHE DEFAULT CELL_FLASH_CACHE DEFAULT)
      TABLESPACE SPMS_DATA
     LOB (MAIL_ADDRESS) STORE AS BASICFILE (
      TABLESPACE SPMS_DATA ENABLE STORAGE IN ROW CHUNK 8192 RETENTION
      NOCACHE LOGGING
      STORAGE(INITIAL 65536 NEXT 1048576 MINEXTENTS 1 MAXEXTENTS 2147483645
      PCTINCREASE 0 FREELISTS 1 FREELIST GROUPS 1
      BUFFER_POOL DEFAULT FLASH_CACHE DEFAULT CELL_FLASH_CACHE DEFAULT))
     LOB (MAIL_CC) STORE AS BASICFILE (
      TABLESPACE SPMS_DATA ENABLE STORAGE IN ROW CHUNK 8192 RETENTION
      NOCACHE LOGGING
      STORAGE(INITIAL 65536 NEXT 1048576 MINEXTENTS 1 MAXEXTENTS 2147483645
      PCTINCREASE 0 FREELISTS 1 FREELIST GROUPS 1
      BUFFER_POOL DEFAULT FLASH_CACHE DEFAULT CELL_FLASH_CACHE DEFAULT))
     LOB (BLIND_COPY_TO) STORE AS BASICFILE (
      TABLESPACE SPMS_DATA ENABLE STORAGE IN ROW CHUNK 8192 RETENTION
      NOCACHE LOGGING
      STORAGE(INITIAL 65536 NEXT 1048576 MINEXTENTS 1 MAXEXTENTS 2147483645
      PCTINCREASE 0 FREELISTS 1 FREELIST GROUPS 1
      BUFFER_POOL DEFAULT FLASH_CACHE DEFAULT CELL_FLASH_CACHE DEFAULT)) ;

**Output**

::

     CREATE
         UNIQUE INDEX PK_BASE_APPR_STEP_DEF
              ON BASE_APPR_STEP_DEF (
              FLOW_ID
              ,NODE_ID
              ,STEP_ID
         ) /*PCTFREE 10*/
         /*INITRANS 2*/
         /*MAXTRANS 255*/
         /*COMPUTE STATISTICS*/
         /*STORAGE(INITIAL 65536 NEXT 1048576 MINEXTENTS 1 MAXEXTENTS 2147483645  FREELISTS 1 FREELIST GROUPS 1 BUFFER_POOL DEFAULT FLASH_CACHE DEFAULT CELL_FLASH_CACHE DEFAULT)*/
         /*TABLESPACE SPMS_DATA */
    ;

.. note::

   If **comment_storage_parameter** is set **TRUE**, then storage parameters are commented.

STORE
-----

The **STORE** keyword for the LOB column is not supported in GaussDB(DWS). Therefore, it is commented out in the migrated output.

**Input - TABLE with** **STORE**

::

   CREATE TABLE CTP_PROC_LOG
       ( PORC_NAME VARCHAR2(100),
     LOG_TIME VARCHAR2(100),
     LOG_INFO CLOB
       ) SEGMENT CREATION IMMEDIATE
      PCTFREE 10 PCTUSED 0 INITRANS 1 MAXTRANS 255
     NOCOMPRESS LOGGING
      STORAGE(INITIAL 65536 NEXT 1048576 MINEXTENTS 1 MAXEXTENTS 2147483645
      PCTINCREASE 0 FREELISTS 1 FREELIST GROUPS 1
      BUFFER_POOL DEFAULT FLASH_CACHE DEFAULT CELL_FLASH_CACHE DEFAULT)
      TABLESPACE SPMS_DATA
     LOB (LOG_INFO) STORE AS BASICFILE (
      TABLESPACE SPMS_DATA ENABLE STORAGE IN ROW CHUNK 8192 RETENTION
      NOCACHE LOGGING
      STORAGE(INITIAL 65536 NEXT 1048576 MINEXTENTS 1 MAXEXTENTS 2147483645
      PCTINCREASE 0 FREELISTS 1 FREELIST GROUPS 1
      BUFFER_POOL DEFAULT FLASH_CACHE DEFAULT CELL_FLASH_CACHE DEFAULT)) ;

**Output**

::

    CREATE
         TABLE
              CTP_PROC_LOG (
                   PORC_NAME VARCHAR2 (100)
                   ,LOG_TIME VARCHAR2 (100)
                   ,LOG_INFO CLOB
              ) /*SEGMENT CREATION IMMEDIATE*/
              /*PCTFREE 10*/
              /*PCTUSED 0*/
              /*INITRANS 1*/
              /*MAXTRANS 255*/
              /*NOCOMPRESS*/
              /*LOGGING*/
              /*STORAGE(INITIAL 65536 NEXT 1048576 MINEXTENTS 1 MAXEXTENTS 2147483645  FREELISTS 1 FREELIST GROUPS 1 BUFFER_POOL DEFAULT FLASH_CACHE DEFAULT CELL_FLASH_CACHE DEFAULT)*/
              /*TABLESPACE SPMS_DATA */
              /*LOB (LOG_INFO) STORE AS BASICFILE ( TABLESPACE SPMS_DATA ENABLE STORAGE IN ROW CHUNK 8192 RETENTION NOCACHE LOGGING STORAGE(INITIAL 65536 NEXT 1048576 MINEXTENTS 1 MAXEXTENTS 2147483645  FREELISTS 1 FREELIST GROUPS 1 BUFFER_POOL DEFAULT FLASH_CACHE DEFAULT CELL_FLASH_CACHE DEFAULT))*/
    ;

PCTINCREASE
-----------

The storage parameter **PCTINCREASE** is not supported for all the tables. In addition, all storage parameters (like pctfree, minextents, maxextents) are not allowed for partitioned tables.

**Input - TABLE with PCTINCREASE**

::

   CREATE TABLE tab1 (
                col1 < datatype >
              , col2 < datatype >
              , ...
              , colN < datatype > )
             TABLESPACE testts
             PCTFREE 10 INITRANS 1 MAXTRANS
             255
             /* STORAGE (
             INITIAL 5 M NEXT 5 M MINEXTENTS 1 MAXEXTENTS UNLIMITED PCTINCREASE 0 );*/

**Output**

::

   CREATE TABLE tab1 (
                col1 < datatype >
              , col2 < datatype >
              , ...
              , colN < datatype >  )
                TABLESPACE testts
                PCTFREE 10 INITRANS 1 MAXTRANS 255
                /* STORAGE (
                INITIAL 5 M NEXT 5 M MINEXTENTS 1 MAXEXTENTS
                UNLIMITED );*/

FOREIGN KEY
-----------

A foreign key is a way to enforce referential integrity within an Oracle database. A foreign key means that values in one table must also appear in another table. The referenced table is called the parent table while the table with the foreign key is called the child table. The foreign key in the child table will generally reference a primary key in the parent table. A foreign key can be defined in either a CREATE TABLE statement or an ALTER TABLE statement.

A foreign key constraint must be established with the REFERENCE clause. An inline constraint clause appears as part of the column definition clause or the object properties clause. An out-of-line constraint appears as part of a relational properties clause or the object properties clause.

If the configuration parameter :ref:`foreignKeyHandler <en-us_topic_0000001813438716__en-us_topic_0218440495_li19969157459>` is set to **true** (default value), then the tool will migrate these statements into commented statements.

DSC supports inline and out-of-line foreign key constraints as shown in the following examples.

**Input - Foreign Key with inline constraint in CREATE TABLE**

::

   CREATE TABLE orders (
       order_no    INT  NOT NULL PRIMARY KEY,
       order_date   DATE  NOT NULL,
       cust_id    INT
    [CONSTRAINT fk_orders_cust]
       REFERENCES customers(cust_id)
      [ON DELETE SET NULL]
    [INITIALLY DEFERRED]
    [ENABLE NOVALIDATE]
   );

**Output**

::

   CREATE TABLE orders (
       order_no    INT  NOT NULL PRIMARY KEY,
       order_date   DATE  NOT NULL,
       cust_id    INT
   /*
     [CONSTRAINT fk_orders_cust]
      REFERENCES customers(cust_id)
      [ON DELETE SET NULL]
      [INITIALLY DEFERRED]
      [ENABLE NOVALIDATE] */
   );

**Input - Foreign Key with out-of-line constraint in CREATE TABLE**

::

   CREATE TABLE customers (
           cust_id   INT   NOT NULL,
          cust_name  VARCHAR(64) NOT NULL,
          cust_addr  VARCHAR(256),
    cust_contact_no  VARCHAR(16),
       PRIMARY KEY (cust_id)
   );

   CREATE TABLE orders (
       order_no    INT  NOT NULL,
       order_date  DATE  NOT NULL,
       cust_id     INT  NOT NULL,
       PRIMARY KEY (order_no),
        CONSTRAINT fk_orders_cust
       FOREIGN KEY (cust_id)
        REFERENCES customers(cust_id)
         ON DELETE CASCADE
   );

**Output**

::

   CREATE TABLE customers (
           cust_id   INT   NOT NULL,
          cust_name  VARCHAR(64) NOT NULL,
          cust_addr  VARCHAR(256),
    cust_contact_no  VARCHAR(16),
       PRIMARY KEY (cust_id)
   );

   CREATE TABLE orders (
         order_no  INT  NOT NULL,
       order_date  DATE  NOT NULL,
          cust_id  INT  NOT NULL,
       PRIMARY KEY (order_no) /*,
        CONSTRAINT fk_orders_cust
       FOREIGN KEY (cust_id)
        REFERENCES customers(cust_id)
         ON DELETE CASCADE */
   );

LONG Data Type
--------------

Columns defined as LONG can store variable-length character data containing up to two gigabytes of information. The tool supports LONG data types in TABLE structure and PL/SQL.

**Input - LONG data type in table structure**

::

   CREATE TABLE project ( proj_cd INT
                          , proj_name VARCHAR2(32)
                          , dept_no INT
                          , proj_det LONG );

**Output**

::

   CREATE TABLE project ( proj_cd INT
                           , proj_name VARCHAR2(32)
                           , dept_no INT
                           , proj_det TEXT );

**Input - LONG data type in PL/SQL**

::

   CREATE OR REPLACE FUNCTION fn_proj_det
                             ( i_proj_cd INT )
   RETURN LONG
   IS
      v_proj_det LONG;
   BEGIN
       SELECT proj_det
         INTO v_proj_det
         FROM project
        WHERE proj_cd = i_proj_cd;

      RETURN v_proj_det;
   END;
   /

**Output**

::

   CREATE OR REPLACE FUNCTION fn_proj_det
                             ( i_proj_cd INT )
   RETURN TEXT
   IS
      v_proj_det TEXT;
   BEGIN
       SELECT proj_det
         INTO v_proj_det
         FROM project
        WHERE proj_cd = i_proj_cd;

      RETURN v_proj_det;
   END;
   /

TYPE
----

MDSYS.MBRCOORDLIST should be replaced with CLOB.

+-----------------------------------+-----------------------------------+
| Oracle Syntax                     | Syntax After Migration            |
+===================================+===================================+
| ::                                | ::                                |
|                                   |                                   |
|    create table product_part      |    CREATE TABLE product_part      |
|    (                              |    (                              |
|      partid    VARCHAR2(24),      |      partid    VARCHAR2(24),      |
|      mbrcoords MDSYS.MBRCOORDLIST |      mbrcoords CLOB               |
|    );                             |    );                             |
+-----------------------------------+-----------------------------------+

MDSYS.SDO_GEOMETRY should be replaced with CLOB.

+-----------------------------------+-----------------------------------+
| Oracle Syntax                     | Syntax After Migration            |
+===================================+===================================+
| ::                                | ::                                |
|                                   |                                   |
|    create table product_part      |    CREATE TABLE product_part      |
|    (                              |    (                              |
|      partid    VARCHAR2(24),      |      partid    VARCHAR2(24),      |
|      shape MDSYS.SDO_GEOMETRY     |      shape CLOB                   |
|    );                             |    );                             |
+-----------------------------------+-----------------------------------+

GEOMETRY should be replaced with CLOB.

+-----------------------------------+-----------------------------------+
| Oracle Syntax                     | Syntax After Migration            |
+===================================+===================================+
| ::                                | ::                                |
|                                   |                                   |
|    create table product_part      |    CREATE TABLE product_part      |
|    (                              |    (                              |
|      partid    VARCHAR2(24),      |      partid    VARCHAR2(24),      |
|      shape GEOMETRY               |      shape CLOB                   |
|    );                             |    );                             |
+-----------------------------------+-----------------------------------+

Columns
-------

**xmax**, **xmin**, **left**, **right** and **maxvalue** are GaussDB(DWS) keywords and should be concatenated with double quotes in upper case.

+-----------------------------------+-----------------------------------+
| Oracle Syntax                     | Syntax After Migration            |
+===================================+===================================+
| ::                                | ::                                |
|                                   |                                   |
|    create table product           |    CREATE TABLE product1          |
|    (                              |    (                              |
|      xmax     VARCHAR2(20),       |      "XMAX"     VARCHAR2(20),     |
|      xmin     VARCHAR2(50),       |      "XMIN"     VARCHAR2(50),     |
|      left     VARCHAR2(50),       |      "LEFT"     VARCHAR2(50),     |
|      right    VARCHAR2(50),       |      "RIGHT"    VARCHAR2(50),     |
|      maxvalue VARCHAR2(50)        |      "MAXVALUE" VARCHAR2(50)      |
|    );                             |    );                             |
+-----------------------------------+-----------------------------------+

Interval Partition
------------------

Partition should be commented for interval partition.

+------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------+
| Oracle Syntax                                                                                                          | Syntax After Migration                                                                                                   |
+========================================================================================================================+==========================================================================================================================+
| ::                                                                                                                     | ::                                                                                                                       |
|                                                                                                                        |                                                                                                                          |
|    CREATE TABLE product                                                                                                |    CREATE TABLE product                                                                                                  |
|    (                                                                                                                   |    (                                                                                                                     |
|      product_id     VARCHAR2(20),                                                                                      |      product_id     VARCHAR2(20),                                                                                        |
|      product_name   VARCHAR2(50),                                                                                      |      product_name   VARCHAR2(50),                                                                                        |
|      manufacture_month DATE                                                                                            |      manufacture_month DATE                                                                                              |
|    )                                                                                                                   |    )                                                                                                                     |
|    partition by range (manufacture_month) interval (NUMTODSINTERVAL (1, 'MONTH'))                                      |    /*partition by range (manufacture_month) interval (NUMTODSINTERVAL (1, 'MONTH'))                                      |
|    (                                                                                                                   |    (                                                                                                                     |
|      partition T_PARTITION_2018_11_LESS values less than (TO_DATE(' 2018-11-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS'))); |      partition T_PARTITION_2018_11_LESS values less than (TO_DATE(' 2018-11-01 00:00:00', 'SYYYY-MM-DD HH24:MI:SS')))*/; |
+------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------+
