:original_name: dws_16_0072.html

.. _dws_16_0072:

.. _en-us_topic_0000001819416149:

PARTITION
=========

The tool does not support migration of partitions/subpartitions and the partition/subpartition keywords are commented in the migrated scripts:

-  Range partition/subpartition
-  List partition/subpartition
-  Hash partition/subpartition

Scenario 1: Assume that the configuration parameters (:ref:`•tdMigrateCASE_N <en-us_topic_0000001819416085__en-us_topic_0000001706224349_en-us_topic_0000001432527901_li33711169269>` and :ref:`•tdMigrateRANGE_N <en-us_topic_0000001819416085__en-us_topic_0000001706224349_en-us_topic_0000001432527901_li143711916152611>`) are set to **comment** or **range** respectively.

The following is a Teradata CREATE TABLE script with nested partitions.

**Input - PARTITION BY RANGE\_N**

.. code-block::

   CREATE TABLE tab1
        ( entry_id           integer   not null
             , oper_id            integer   not null
             , source_system_cd   varchar(5)
             , entry_dt           date
             , file_id            integer
              , load_id            integer
             , contract_id        varchar(50)
             , contract_type_cd   varchar(50)
             )
   PRIMARY INDEX (entry_id, oper_id, source_system_cd)
   PARTITION BY ( CASE_N( source_system_cd = '00000'
                                                   , source_system_cd = '00002'
                                                   , source_system_cd = '00006'
                                                   , source_system_cd = '00018'
                                                   , NO CASE )
                                , RANGE_N(  entry_dt BETWEEN DATE '2012-01-01' AND DATE '2025-12-31' EACH INTERVAL '1' DAY, NO RANGE )
                                );

**Output**

.. code-block::

   CREATE TABLE tab1
        ( entry_id           integer   not null
             , oper_id            integer   not null
             , source_system_cd   varchar(5)
             , entry_dt           date
             , file_id            integer
              , load_id            integer
             , contract_id        varchar(50)
             , contract_type_cd   varchar(50)
             )
   DISTRIBUTE BY HASH (entry_id, oper_id, source_system_cd)
   PARTITION BY RANGE (entry_dt) ( PARTITION tab1_p1 START (CAST('2012-01-01' AS DATE))
                                                                                END (CAST('2025-12-31' AS DATE))
                                                                                EVERY (INTERVAL '1' DAY) );

Scenario 2: Assume that the configuration parameters (:ref:`tdMigrateCASE_N <en-us_topic_0000001819416085__en-us_topic_0000001706224349_en-us_topic_0000001432527901_li33711169269>` and :ref:`tdMigrateRANGE_N <en-us_topic_0000001819416085__en-us_topic_0000001706224349_en-us_topic_0000001432527901_li143711916152611>`) are set to **comment** or **range** respectively.

The following is a Teradata CREATE TABLE script with nested partitions.

**Input**

.. code-block::

   CREATE TABLE tab2
        ( entry_id           integer   not null
             , oper_id            integer   not null
             , source_system_cd   varchar(5)
             , entry_dt           date
             , file_id            integer
              , load_id            integer
             , contract_id        varchar(50)
             , contract_type_cd   varchar(50)
             )
   PRIMARY INDEX (entry_id, oper_id, source_system_cd)
   PARTITION BY ( RANGE_N(  entry_dt BETWEEN DATE '2012-01-01' AND DATE '2025-12-31' EACH INTERVAL '1' DAY, NO RANGE )
                                , CASE_N( source_system_cd = '00000'
                                                   , source_system_cd = '00002'
                                                   , source_system_cd = '00006'
                                                   , source_system_cd = '00018'
                                                   , NO CASE )
                                );

**Output**

.. code-block::

   CREATE TABLE tab2
        ( entry_id           integer   not null
             , oper_id            integer   not null
             , source_system_cd   varchar(5)
             , entry_dt           date
             , file_id            integer
              , load_id            integer
             , contract_id        varchar(50)
             , contract_type_cd   varchar(50)
             )
   DISTRIBUTE BY HASH (entry_id, oper_id, source_system_cd)
   PARTITION BY RANGE (entry_dt) ( PARTITION tab2_p1 START (CAST('2012-01-01' AS DATE))
                                                                                END (CAST('2025-12-31' AS DATE))
                                                                                EVERY (INTERVAL '1' DAY) );

Scenario 3: Assume that the configuration parameters (:ref:`tdMigrateCASE_N <en-us_topic_0000001819416085__en-us_topic_0000001706224349_en-us_topic_0000001432527901_li33711169269>` and :ref:`tdMigrateRANGE_N <en-us_topic_0000001819416085__en-us_topic_0000001706224349_en-us_topic_0000001432527901_li143711916152611>`) are set to values other than **comment** or **range** respectively.

Partition syntax will not be commented and the remaining syntax will be migrated.

**Input**

.. code-block::

   CREATE TABLE tab1
        ( entry_id           integer   not null
             , oper_id            integer   not null
             , source_system_cd   varchar(5)
             , entry_dt           date
             , file_id            integer
              , load_id            integer
             , contract_id        varchar(50)
             , contract_type_cd   varchar(50)
             )
   PRIMARY INDEX (entry_id, oper_id, source_system_cd)
   PARTITION BY ( CASE_N( source_system_cd = '00000'
                                                   , source_system_cd = '00002'
                                                   , source_system_cd = '00006'
                                                   , source_system_cd = '00018'
                                                   , NO CASE )
                                , RANGE_N(  entry_dt BETWEEN DATE '2012-01-01' AND DATE '2025-12-31' EACH INTERVAL '1' DAY, NO RANGE )
                                );

**Output**

.. code-block::

   CREATE TABLE tab2
        ( entry_id           integer   not null
             , oper_id            integer   not null
             , source_system_cd   varchar(5)
             , entry_dt           date
             , file_id            integer
              , load_id            integer
             , contract_id        varchar(50)
             , contract_type_cd   varchar(50)
             )
   DISTRIBUTE BY HASH (entry_id, oper_id, source_system_cd)
   /* PARTITION BY ( CASE_N( source_system_cd = '00000'
                                                   , source_system_cd = '00002'
                                                   , source_system_cd = '00006'
                                                   , source_system_cd = '00018'
                                                   , NO CASE )
                                , RANGE_N(  entry_dt BETWEEN DATE '2012-01-01' AND DATE '2025-12-31' EACH INTERVAL '1' DAY, NO RANGE )
                                ) */
   ;

Scenario 4: Assume that the configuration parameters (:ref:`tdMigrateCASE_N <en-us_topic_0000001819416085__en-us_topic_0000001706224349_en-us_topic_0000001432527901_li33711169269>` and :ref:`tdMigrateRANGE_N <en-us_topic_0000001819416085__en-us_topic_0000001706224349_en-us_topic_0000001432527901_li143711916152611>`) are set to any value.

The following is another TD create table script with RANGE_N partition (without nested partitions).

**Input**

.. code-block::

   CREATE TABLE tab4
        ( entry_id           integer   not null
             , oper_id            integer   not null
             , source_system_cd   varchar(5)
             , entry_dt           date
             , file_id            integer
              , load_id            integer
             , contract_id        varchar(50)
             , contract_type_cd   varchar(50)
             )
   PRIMARY INDEX (entry_id, oper_id, source_system_cd)
   PARTITION BY (RANGE_N(  entry_dt BETWEEN DATE '2012-01-01' AND DATE '2025-12-31' EACH INTERVAL '1' DAY, NO RANGE )
                          );

**Output**

.. code-block::

   CREATE TABLE tab4
        ( entry_id           integer   not null
             , oper_id            integer   not null
             , source_system_cd   varchar(5)
             , entry_dt           date
             , file_id            integer
              , load_id            integer
             , contract_id        varchar(50)
             , contract_type_cd   varchar(50)
             )
   DISTRIBUTE BY HASH (entry_id, oper_id, source_system_cd)
   PARTITION BY RANGE (entry_dt) ( PARTITION tab4_p1 START (CAST('2012-01-01' AS DATE))
                                                                                END (CAST('2025-12-31' AS DATE))
                                                                                EVERY (INTERVAL '1' DAY) );

Scenario 5: Assume that the configuration parameters (:ref:`tdMigrateCASE_N <en-us_topic_0000001819416085__en-us_topic_0000001706224349_en-us_topic_0000001432527901_li33711169269>` and :ref:`tdMigrateRANGE_N <en-us_topic_0000001819416085__en-us_topic_0000001706224349_en-us_topic_0000001432527901_li143711916152611>`) are set to **comment** or **range** respectively.

The following is another teradata create table script with CASE_N partition (without nested partitions).

**Input**

.. code-block::

   CREATE TABLE tab5
        ( entry_id           integer   not null
             , oper_id            integer   not null
             , source_system_cd   varchar(5)
             , entry_dt           date
             , file_id            integer
              , load_id            integer
             , contract_id        varchar(50)
             , contract_type_cd   varchar(50)
             )
   PRIMARY INDEX (entry_id, oper_id, source_system_cd)
   PARTITION BY ( CASE_N( source_system_cd = '00000'
                                                   , source_system_cd = '00002'
                                                   , source_system_cd = '00006'
                                                   , source_system_cd = '00018'
                                                   , NO CASE )
                        );

**Output**

.. code-block::

   CREATE TABLE tab5
        ( entry_id           integer   not null
             , oper_id            integer   not null
             , source_system_cd   varchar(5)
             , entry_dt           date
             , file_id            integer
              , load_id            integer
             , contract_id        varchar(50)
             , contract_type_cd   varchar(50)
             )
   DISTRIBUTE BY HASH (entry_id, oper_id, source_system_cd)
   /* PARTITION BY ( CASE_N( source_system_cd = '00000'
                                                   , source_system_cd = '00002'
                                                   , source_system_cd = '00006'
                                                   , source_system_cd = '00018'
                                                   , NO CASE )
                        ) */
   ;

Partition of RANGE_N in the String Columns
------------------------------------------

+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------+
| Input                                                                                                                                                                                                                              | Output                                                                                                                                 |
+====================================================================================================================================================================================================================================+========================================================================================================================================+
| .. code-block::                                                                                                                                                                                                                    | .. code-block::                                                                                                                        |
|                                                                                                                                                                                                                                    |                                                                                                                                        |
|    CREATE SET TABLE SC.TAB , NO FALLBACK,                                                                                                                                                                                          |    CREATE                                                                                                                              |
|    NO BEFORE JOURNAL,                                                                                                                                                                                                              |         TABLE                                                                                                                          |
|    NO AFTER JOURNAL,                                                                                                                                                                                                               |              SC.TAB (                                                                                                                  |
|    CHECKSUM=DEFAULT,                                                                                                                                                                                                               |                   ACCOUNT_NUM VARCHAR( 255 ) /* CHARACTER SET LATIN*/       /* NOT CASESPECIFIC*/               NOT NULL               |
|    DEFAULT MERGEBLOCKRATIO                                                                                                                                                                                                         |                   ,ACCOUNT_MODIFIER_NUM CHAR( 18 ) /* CHARACTER SET LATIN*/               /* NOT CASESPECIFIC*/               NOT NULL |
|    (                                                                                                                                                                                                                               |                   ,DATA_SOURCE_ID CHAR( 10 ) /* CHARACTER SET LATIN*/               /* NOT CASESPECIFIC*/                              |
|    ACCOUNT_NUM VARCHAR(255) CHARACTER SET LATIN NOT CASESPECIFIC NOT NULL                                                                                                                                                          |                   ,END_DT DATE                                                                                                         |
|    ,ACCOUNT_MODIFIER_NUM CHAR(18) CHARACTER SET LATIN NOT CASESPECIFIC NOT NULL                                                                                                                                                    |                   ,UPD_TXF_BATCHTD INTEGER /* COMPRESS */                                                                              |
|    ,DATA_SOURCE_ID CHAR(10) CHARACTER SET LATIN NOT CASESPECIFIC                                                                                                                                                                   |              ) DISTRIBUTE BY HASH (                                                                                                    |
|    ,END_DT DATE FORMAT 'YYYY-MM-DD'                                                                                                                                                                                                |                   ACCOUNT_NUM                                                                                                          |
|    ,UPD_TXF_BATCHTD INTEGER COMPRESS                                                                                                                                                                                               |                   ,ACCOUNT_MODIFIER_NUM                                                                                                |
|    )                                                                                                                                                                                                                               |              )/* PARTITION BY (                                                                                                        |
|    PRIMARY INDEX XPKT0300_AGREEMENT (ACCOUNT_NUM,ACCOUNT_MODIFIER_NUM)                                                                                                                                                             |                   RANGE_N (                                                                                                            |
|    PARTITION BY ( RANGE_N(DATA_SOURCE_ID BETWEEN 'A','B','C','D','E','F','G','H','I','J','K','L','M','N','O','P','Q','R','S','T','U','V','W','X','Y','Z' AND 'ZZ', NO RANGE ,UNKNOWN) ,CASE_N(END_DT IS NULL , NO CASE , UNKNOWN)) |                        DATA_SOURCE_ID BETWEEN 'A'                                                                                      |
|    ;                                                                                                                                                                                                                               |                        ,'B'                                                                                                            |
|                                                                                                                                                                                                                                    |                        ,'C'                                                                                                            |
|                                                                                                                                                                                                                                    |                        ,'D'                                                                                                            |
|                                                                                                                                                                                                                                    |                        ,'E'                                                                                                            |
|                                                                                                                                                                                                                                    |                        ,'F'                                                                                                            |
|                                                                                                                                                                                                                                    |                        ,'G'                                                                                                            |
|                                                                                                                                                                                                                                    |                        ,'H'                                                                                                            |
|                                                                                                                                                                                                                                    |                        ,'I'                                                                                                            |
|                                                                                                                                                                                                                                    |                        ,'J'                                                                                                            |
|                                                                                                                                                                                                                                    |                        ,'K'                                                                                                            |
|                                                                                                                                                                                                                                    |                        ,'L'                                                                                                            |
|                                                                                                                                                                                                                                    |                        ,'M'                                                                                                            |
|                                                                                                                                                                                                                                    |                        ,'N'                                                                                                            |
|                                                                                                                                                                                                                                    |                        ,'O'                                                                                                            |
|                                                                                                                                                                                                                                    |                        ,'P'                                                                                                            |
|                                                                                                                                                                                                                                    |                        ,'Q'                                                                                                            |
|                                                                                                                                                                                                                                    |                        ,'R'                                                                                                            |
|                                                                                                                                                                                                                                    |                        ,'S'                                                                                                            |
|                                                                                                                                                                                                                                    |                        ,'T'                                                                                                            |
|                                                                                                                                                                                                                                    |                        ,'U'                                                                                                            |
|                                                                                                                                                                                                                                    |                        ,'V'                                                                                                            |
|                                                                                                                                                                                                                                    |                        ,'W'                                                                                                            |
|                                                                                                                                                                                                                                    |                        ,'X'                                                                                                            |
|                                                                                                                                                                                                                                    |                        ,'Y'                                                                                                            |
|                                                                                                                                                                                                                                    |                        ,'Z' AND 'ZZ'                                                                                                   |
|                                                                                                                                                                                                                                    |                        ,NO RANGE                                                                                                       |
|                                                                                                                                                                                                                                    |                        ,UNKNOWN                                                                                                        |
|                                                                                                                                                                                                                                    |                   )                                                                                                                    |
|                                                                                                                                                                                                                                    |                   ,*/                                                                                                                  |
|                                                                                                                                                                                                                                    |    /* CASE_N(END_DT IS NULL , NO CASE , UNKNOWN))  */                                                                                  |
|                                                                                                                                                                                                                                    |                   ;                                                                                                                    |
+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------+

RANGE_N with different partition INTERVAL
-----------------------------------------

+------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------+
| Input                                                                                    | Output                                                                                                 |
+==========================================================================================+========================================================================================================+
| .. code-block::                                                                          | .. code-block::                                                                                        |
|                                                                                          |                                                                                                        |
|    CREATE MULTISET TABLE tab1                                                            |    CREATE TABLE tab1                                                                                   |
|         ( TICD                  VARCHAR(10)                                              |         ( TICD                  VARCHAR( 10 )                                                          |
|         , TCIT                   VARCHAR(10)                                             |         , TCIT                   VARCHAR( 10 )                                                         |
|         , TCCM                  VARCHAR(50)                                              |         , TCCM                  VARCHAR( 50 )                                                          |
|         , DW_Stat_Dt         DATE                                                        |         , DW_Stat_Dt         DATE                                                                      |
|        )                                                                                 |         )                                                                                              |
|    PRIMARY INDEX ( TICD )                                                                |    DISTRIBUTE BY HASH (TICD)                                                                           |
|    PARTITION BY RANGE_N                                                                  |    PARTITION BY RANGE (DW_Stat_Dt)                                                                     |
|      ( DW_Stat_Dt BETWEEN DATE '0001-01-01' AND DATE '0001-01-04' EACH INTERVAL '1' DAY, |      ( PARTITION tab1_0 START (DATE '0001-01-01') END (DATE '0001-01-04') EVERY (INTERVAL '1' DAY),    |
|         DATE '0001-01-05' AND DATE '1899-12-31',                                         |         PARTITION tab1_1 START (DATE '0001-01-04') END (DATE '1899-12-31'),                            |
|         DATE '1900-01-01' AND DATE '1900-01-01',                                         |         PARTITION tab1_2 START (DATE '1899-12-31') END (DATE '1900-01-01'),                            |
|         DATE '1900-01-02' AND DATE '1999-12-31',                                         |         PARTITION tab1_ 3 START (DATE '1900-01-01') END (DATE '1999-12-31'),                           |
|         DATE '2000-01-01' AND DATE '2009-12-31' EACH INTERVAL '1' YEAR,                  |         PARTITION tab1_4 START (DATE '1999-12-31') END (DATE '2009-12-31') EVERY (INTERVAL '1' YEAR) , |
|         DATE '2010-01-01' AND DATE '2021-12-31' EACH INTERVAL '1' DAY,                   |         PARTITION tab1_5 START (DATE '2009-12-31') END (DATE '2021-12-31') EVERY (INTERVAL '1' DAY) ,  |
|         DATE '9999-12-31' AND DATE '9999-12-31',                                         |         PARTITION tab1_6 START (DATE '2021-12-31') END (DATE '9999-12-31')                             |
|         NO RANGE );                                                                      |      );                                                                                                |
+------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------+

RANGE_N with \* for start-date
------------------------------

+------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------+
| Input                                                                        | Output                                                                                                   |
+==============================================================================+==========================================================================================================+
| .. code-block::                                                              | .. code-block::                                                                                          |
|                                                                              |                                                                                                          |
|    CREATE MULTISET TABLE Orders5 (                                           |    CREATE TABLE Orders5 (                                                                                |
|       StoreNo SMALLINT,                                                      |       StoreNo SMALLINT,                                                                                  |
|       OrderNo INTEGER,                                                       |       OrderNo INTEGER,                                                                                   |
|       OrderDate DATE,                                                        |       OrderDate DATE,                                                                                    |
|       OrderTotal INTEGER                                                     |       OrderTotal INTEGER                                                                                 |
|    )                                                                         |    )                                                                                                     |
|    PRIMARY INDEX(OrderNo)                                                    |    DISTRIBUTE BY HASH (OrderNo)                                                                          |
|    PARTITION BY RANGE_N  (                                                   |    PARTITION BY RANGE (OrderDate)                                                                        |
|       OrderDate BETWEEN DATE * AND DATE '2016-12-31' EACH INTERVAL '1' YEAR, |       ( PARTITION Orders5_0 START (DATE '0001-01-01') END (DATE '2016-12-31') EVERY (INTERVAL '1' YEAR), |
|         DATE '2017-01-01'  EACH INTERVAL '1' MONTH,                          |    PARTITION Orders5_1 START (DATE '2016-12-31') END (DATE '2020-01-01') EVERY (INTERVAL '1' MONTH),     |
|         DATE '2020-01-01' AND DATE '2020-12-31' EACH INTERVAL '1' DAY        |    PARTITION Orders5_2 START (DATE '2020-01-01') END (DATE '2020-12-31') EVERY (INTERVAL '1' DAY)        |
|    );                                                                        |    );                                                                                                    |
+------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------+

RANGE_N with \* for end-date
----------------------------

+------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------+
| Input                                                                              | Output                                                                                                   |
+====================================================================================+==========================================================================================================+
| .. code-block::                                                                    | .. code-block::                                                                                          |
|                                                                                    |                                                                                                          |
|    CREATE SET TABLE Orders4 (                                                      |    CREATE TABLE Orders4 (                                                                                |
|       StoreNo SMALLINT,                                                            |       StoreNo SMALLINT,                                                                                  |
|       OrderNo INTEGER,                                                             |       OrderNo INTEGER,                                                                                   |
|       OrderDate DATE,                                                              |       OrderDate DATE,                                                                                    |
|       OrderTotal INTEGER                                                           |       OrderTotal INTEGER                                                                                 |
|    )                                                                               |    )                                                                                                     |
|    PRIMARY INDEX(OrderNo)                                                          |    DISTRIBUTE BY HASH (OrderNo)                                                                          |
|    PARTITION BY RANGE_N  (                                                         |    PARTITION BY RANGE (OrderDate)                                                                        |
|       OrderDate BETWEEN DATE '2010-01-01' AND '2016-12-31' EACH INTERVAL '1' YEAR, |       ( PARTITION Orders4_0 START (DATE '2010-01-01') END (DATE '2016-12-31') EVERY (INTERVAL '1' YEAR), |
|         DATE '2017-01-01' EACH INTERVAL '1' MONTH,                                 |    PARTITION Orders4_1 START (DATE '2016-12-31') END (DATE '2020-01-01') EVERY (INTERVAL '1' MONTH) ,    |
|         DATE '2019-01-01' AND *                                                    |    PARTITION Orders4_2 START (DATE '2020-01-01') END (MAXVALUE)                                          |
|    );                                                                              |    );                                                                                                    |
+------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------+

RANGE_N with comma seperated values
-----------------------------------

+---------------------------------------------------------------------------+---------------------------------------------+
| Input                                                                     | Output                                      |
+===========================================================================+=============================================+
| .. code-block::                                                           | .. code-block::                             |
|                                                                           |                                             |
|    CREATE TABLE orders10                                                  |    CREATE TABLE orders10                    |
|        (storeid INTEGER NOT NULL                                          |        (storeid INTEGER NOT NULL            |
|        ,productid INTEGER NOT NULL                                        |        ,productid INTEGER NOT NULL          |
|        ,orderdate DATE NOT NULL                                           |        ,orderdate DATE NOT NULL             |
|        ,totalorders INTEGER NOT NULL)                                     |        ,totalorders INTEGER NOT NULL)       |
|        PRIMARY INDEX (storeid, productid)                                 |    DISTRIBUTE BY HASH (storeid, productid)  |
|         PARTITION BY ( RANGE_N(totalorders BETWEEN *, 100, 1000 AND *) ); |    PARTITION BY RANGE (totalorders)         |
|                                                                           |       ( PARTITION Orders10_0 END (100),     |
|                                                                           |         PARTITION Orders10_1 END (1000),    |
|                                                                           |         PARTITION Orders10_2 END (MAXVALUE) |
|                                                                           |       );                                    |
+---------------------------------------------------------------------------+---------------------------------------------+
