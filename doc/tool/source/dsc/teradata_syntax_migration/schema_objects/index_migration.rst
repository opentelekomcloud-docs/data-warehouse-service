:original_name: dws_mt_0064.html

.. _dws_mt_0064:

Index Migration
===============

The sequence of **CREATE INDEX** columns and table names in Teradata is different from that in GaussDB(DWS). Use the configuration parameter :ref:`distributeByHash <en-us_topic_0000001233922159__en-us_topic_0218440346_li20873348324>` to configure how the data is distributed across the cluster nodes. The tool will not add DISTRIBUTE BY HASH which is used to create a table with Primary Key and Non-Unique Primary Index.

**Input - Primary key is not superset of primary index and only one column is matched**

::

   CREATE TABLE      good_5 (
                   column_1 INTEGER NOT NULL PRIMARY KEY
                  ,column_2 INTEGER
                  ,column_3 INTEGER NOT NULL
                  ,column_4 INTEGER
             ) PRIMARY INDEX (column _1,column_2);

**Output**

::

   CREATE TABLE      good_5 (
                   column_1 INTEGER NOT NULL PRIMARY KEY
                  ,column_2 INTEGER
                  ,column_3 INTEGER NOT NULL
                  ,column_4 INTEGER
             )
   ;

**Input - Primary key is not superset of primary index and no column is matched**

::

   CREATE SET  TABLE  DP_SEDW.T_170UT_HOLDER_ACCT
              ,NO FALLBACK
              ,NO BEFORE JOURNAL
              ,NO AFTER JOURNAL (
                  BUSINESSDATE VARCHAR( 10 )
                  ,SOURCESYSTEM VARCHAR( 5 )
                  ,UPLOADCODE VARCHAR( 1 )
                  ,HOLDER_NO VARCHAR( 7 ) NOT NULL
                  ,POSTAL_ADD_4 VARCHAR( 40 )
                  ,EPF_IND CHAR( 1 )
                  ,PRIMARY KEY (  UPLOADCODE  ,HOLDER_NO   )
   ) PRIMARY INDEX ( SOURCESYSTEM,EPF_IND  );

**Output**

::

   CREATE TABLE   DP_SEDW.T_170UT_HOLDER_ACCT (
                  BUSINESSDATE VARCHAR( 10 )
                  ,SOURCESYSTEM VARCHAR( 5 )
                  ,UPLOADCODE VARCHAR( 1 )
                  ,HOLDER_NO VARCHAR( 7 ) NOT NULL
                  ,POSTAL_ADD_4 VARCHAR( 40 )
                  ,EPF_IND CHAR( 1 )
                  ,PRIMARY KEY (UPLOADCODE ,HOLDER_NO   ) );

**Input - No primary key and unique index has index name**

::

   CREATE SET TABLE "DP_TEDW"."T0409_INTERNAL_ORG_GRP_FUNCT",
        NO FALLBACK, NO BEFORE JOURNAL,
        NO AFTER JOURNAL
        ( Organization_Party_Id        INTEGER                NOT NULL
        , Function_Code                 SMALLINT               NOT NULL
        , Intern_Funct_Strt_Date     DATE FORMAT 'YYYY-MM-DD'  NOT NULL
        , Intern_Funct_End_Date     DATE FORMAT 'YYYY-MM-DD'
        )
   PRIMARY INDEX ( Organization_Party_Id )
   UNIQUE INDEX ux_t0409_intr_fn_1 ( Function_Code, Intern_Funct_Strt_Date )
   UNIQUE INDEX ( Organization_Party_Id, Intern_Funct_Strt_Date );

**Output**

::

   CREATE TABLE "DP_TEDW"."T0409_INTERNAL_ORG_GRP_FUNCT"
            ( Organization_Party_Id        INTEGER             NOT NULL
            , Function_Code                 SMALLINT            NOT NULL
            , Intern_Funct_Strt_Date     DATE                  NOT NULL
            , Intern_Funct_End_Date     DATE
            )
   DISTRIBUTE BY HASH ( Organization_Party_Id );
   CREATE INDEX ux_t0409_intr_fn_1 ON "DP_TEDW"."T0409_INTERNAL_ORG_GRP_FUNCT" ( Function_Code, Intern_Funct_Strt_Date );
   CREATE UNIQUE INDEX ON "DP_TEDW"."T0409_INTERNAL_ORG_GRP_FUNCT" ( Organization_Party_Id, Intern_Funct_Strt_Date );

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
