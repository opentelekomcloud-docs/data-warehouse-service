:original_name: dws_16_0069.html

.. _dws_16_0069:

.. _en-us_topic_0000001819336117:

Indexes
=======

The CREATE TABLE statement supports creation of an index. Migration tool supports the TABLE statement with PRIMARY INDEX and UNIQUE INDEX.

The tool will not add DISTRIBUTE BY HASH which is used to create a table with PRIMARY KEY and Non-Unique PRIMARY INDEX.

**Input: CREATE TABLE with INDEX**

::

   CREATE SET TABLE DP_TEDW.B0381_ACCOUNT_OBTAINED_MAP,
         NO FALLBACK, NO BEFORE JOURNAL,
         NO AFTER JOURNAL, CHECKSUM = DEFAULT
    ( Ranked_Id            INTEGER  NOT NULL
    , Source_System_Code   SMALLINT NOT NULL
    , Operational_Acc_Obtained_Id VARCHAR(100)
      CHARACTER SET LATIN NOT CASESPECIFIC FORMAT 'X(50)'
    , Mapped_Id               INTEGER  NOT NULL
    )
   PRIMARY INDEX B0381_ACCOUNT_OBTAINED_idx_PR ( Ranked_Id )
   UNIQUE INDEX B0381_ACCT_OBT_MAP__idx_SCD ( Source_System_Code )
   INDEX B0381_ACCT_OBT_MAP__idx_OPID ( Operational_Acc_Obtained_Id );

**Output**

::

   CREATE TABLE DP_TEDW.B0381_ACCOUNT_OBTAINED_MAP
     ( Ranked_Id INTEGER NOT NULL
     , Source_System_Code SMALLINT NOT NULL
     , Operational_Acc_Obtained_Id VARCHAR( 100 )
     , Mapped_Id INTEGER NOT NULL
     )
   DISTRIBUTE BY HASH ( Ranked_Id );

   CREATE INDEX B0381_ACCT_OBT_MAP__idx_SCD ON DP_TEDW.B0381_ACCOUNT_OBTAINED_MAP ( Source_System_Code );
   CREATE INDEX B0381_ACCT_OBT_MAP__idx_OPID ON DP_TEDW.B0381_ACCOUNT_OBTAINED_MAP ( Operational_Acc_Obtained_Id );

.. note::

   UNIQUE is removed in the index since index column list (organic_name) is not a super set of DISTRIBUTE BY column list (serial_no, organic_name).

**Input - CREATE TABLE with Primary Key and Non-Unique Primary Index** (DISTRIBUTE BY HASH is not added)

::

   CREATE TABLE employee
    (
      EMP_NO INTEGER
    , DEPT_NO INTEGER
    , FIRST_NAME VARCHAR(20)
    , LAST_NAME CHAR(20)
    , SALARY DECIMAL(10,2)
    , ADDRESS VARCHAR(100)
    , CONSTRAINT pk_emp PRIMARY KEY ( EMP_NO )
      ) PRIMARY INDEX ( DEPT_NO ) ;

**Output**

::

   CREATE TABLE employee
    (
      EMP_NO INTEGER
    , DEPT_NO INTEGER
    , FIRST_NAME VARCHAR(20)
    , LAST_NAME CHAR(20)
    , SALARY DECIMAL(10,2)
    , ADDRESS VARCHAR(100)
    , CONSTRAINT pk_emp PRIMARY KEY ( EMP_NO )
     )
   ;

Creating a Partition Table with an Index
----------------------------------------

If **tdMigrateRANGE_N** is set to **true**,

+----------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------+
| Input                                                                                                          | Output                                                                                                                                 |
+================================================================================================================+========================================================================================================================================+
| .. code-block::                                                                                                | .. code-block::                                                                                                                        |
|                                                                                                                |                                                                                                                                        |
|    CREATE SET TABLE SC.TAB , NO FALLBACK,                                                                      |    CREATE                                                                                                                              |
|    NO BEFORE JOURNAL,                                                                                          |         TABLE                                                                                                                          |
|    NO AFTER JOURNAL,                                                                                           |              SC.TAB (                                                                                                                  |
|    CHECKSUM=DEFAULT,                                                                                           |                   ACCOUNT_NUM VARCHAR( 255 ) /* CHARACTER SET LATIN*/                                                                  |
|    DEFAULT MERGEBLOCKRATIO                                                                                     |                    /* NOT CASESPECIFIC*/               NOT NULL                                                                        |
|    (                                                                                                           |                   ,ACCOUNT_MODIFIER_NUM CHAR( 18 ) /* CHARACTER SET LATIN*/               /* NOT CASESPECIFIC*/               NOT NULL |
|    ACCOUNT_NUM VARCHAR(255) CHARACTER SET LATIN NOT CASESPECIFIC NOT NULL                                      |                   ,END_DT DATE                                                                                                         |
|    ,ACCOUNT_MODIFIER_NUM CHAR(18) CHARACTER SET LATIN NOT CASESPECIFIC NOT NULL                                |                   ,UPD_TXF_BATCHTD INTEGER /* COMPRESS */                                                                              |
|    ,END_DT DATE FORMAT 'YYYY-MM-DD'                                                                            |              ) DISTRIBUTE BY HASH (                                                                                                    |
|    ,UPD_TXF_BATCHTD INTEGER COMPRESS                                                                           |                   ACCOUNT_NUM                                                                                                          |
|    )                                                                                                           |                   ,ACCOUNT_MODIFIER_NUM                                                                                                |
|    PRIMARY INDEX XPKT0300_AGREEMENT (ACCOUNT_NUM,ACCOUNT_MODIFIER_NUM)                                         |              ) PARTITION BY RANGE (END_DT) (                                                                                           |
|    PARTITION BY RANGE_N(END_DT BETWEEN '2001-01-01' AND '2020-12-31' EACH INTERVAL '1' DAY, NO RANGE ,UNKNOWN) |                   PARTITION TAB_1 start ('2001-01-01')                                                                                 |
|    INDEX (UPD_TXF_BATCHTD)                                                                                     |              END ('2020-12-31') EVERY (                                                                                                |
|    ;                                                                                                           |                   INTERVAL '1' DAY )                                                                                                   |
|                                                                                                                |              ) ;                                                                                                                       |
|                                                                                                                |    CREATE INDEX ON SC.TAB (UPD_TXF_BATCHTD) LOCAL;                                                                                     |
+----------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------+
