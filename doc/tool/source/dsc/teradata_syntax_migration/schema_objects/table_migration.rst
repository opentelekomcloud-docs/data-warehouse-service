:original_name: dws_mt_0048.html

.. _dws_mt_0048:

Table Migration
===============

The table-specific keyword **MULTISET VOLATILE** is provided in the input file, but the keyword is not supported by GaussDB(DWS). Therefore, the tool replaces it with the **LOCAL TEMPORARY/UNLOGGED** keyword during the migration process. Use the :ref:`session_mode <en-us_topic_0000001233922159__en-us_topic_0218440346_li9493135323214>` configuration parameter to set the default table type (SET/MULTISET) for CREATE TABLE.

For details, see the following topics:

:ref:`CREATE TALBE <en-us_topic_0000001188362598__en-us_topic_0238518354_en-us_topic_0237362205_section422135632511>`

:ref:`CHARACTER SET and CASESPECIFIC <en-us_topic_0000001188362598__en-us_topic_0238518354_en-us_topic_0237362205_section107711645192617>`

:ref:`VOLATILE <en-us_topic_0000001188362598__en-us_topic_0238518354_en-us_topic_0237362205_section0835779292>`

:ref:`SET <en-us_topic_0000001188362598__en-us_topic_0238518354_en-us_topic_0237362205_section184410717305>`

:ref:`MULTISET <en-us_topic_0000001188362598__en-us_topic_0238518354_en-us_topic_0237362205_section1039193943014>`

:ref:`TITLE <en-us_topic_0000001188362598__en-us_topic_0238518354_en-us_topic_0237362205_section164902010315>`

:ref:`INDEX <en-us_topic_0000001188362598__en-us_topic_0238518354_en-us_topic_0237362205_section897124714316>`

:ref:`CONSTRAINT <en-us_topic_0000001188362598__en-us_topic_0238518354_en-us_topic_0237362205_section5984610123218>`

:ref:`COLUMN STORE <en-us_topic_0000001188362598__en-us_topic_0238518354_en-us_topic_0237362205_section07421933153219>`

:ref:`PARTITION <en-us_topic_0000001188362598__en-us_topic_0238518354_en-us_topic_0237362205_section1312517610339>`

:ref:`ANALYZE <en-us_topic_0000001188362598__en-us_topic_0238518354_en-us_topic_0237362205_section18781737133310>`

:ref:`Data Types <en-us_topic_0000001188362598__en-us_topic_0238518354_en-us_topic_0237362205_section1139515893319>`

:ref:`Support for Specified Columns <en-us_topic_0000001188362598__en-us_topic_0238518354_en-us_topic_0237362205_section883201463413>`

.. _en-us_topic_0000001188362598__en-us_topic_0238518354_en-us_topic_0237362205_section422135632511:

CREATE TALBE
------------

The Teradata **CREATE TABLE** (:ref:`short key <en-us_topic_0000001233922185__en-us_topic_0238518364_en-us_topic_0237362468_section75711541419>` CT) statements are used to create new tables.

**Example:**

**Input: CREATE TABLE**

::

   CT tab1 (
        id INT
   );

Output

::

   CREATE
        TABLE
             tab1 (
                  id INTEGER
             )
   ;

When using CREATE *tab2* AS *tab1*, a new table *tab2* is created with the structure copied from *tab1*. If the CREATE TABLE statement includes WITH DATA operator, then the data from *tab1* is also copied into *tab2*. When using CREATE AS, the behavior of the CONSTRAINT from the source table is retained in the new target table.

-  If :ref:`session_mode <en-us_topic_0000001233922159__en-us_topic_0218440346_li9493135323214>` = *Teradata*, the default table type is **SET** in which duplicate records must be removed. This is done by adding the **MINUS** operator in the migrated scripts.
-  If :ref:`session_mode <en-us_topic_0000001233922159__en-us_topic_0218440346_li9493135323214>` = *ANSI*, the default table type is **MULTISET** in which duplicate records must be allowed.

If the source table has a PRIMARY KEY or a UNIQUE CONSTRAINT, then it will not contain any duplicate records. In this case, the MINUS operator is not required or added to remove duplicate records.

**Example:**

**Input: CREATE TABLE AS with DATA (session_mode=Teradata)**

::

   CREATE TABLE tab2
       AS tab1 WITH DATA;

**Output**

::

   BEGIN
       CREATE TABLE tab2 (
               LIKE tab1 INCLUDING ALL EXCLUDING PARTITION EXCLUDING RELOPTIONS
                         );

       INSERT INTO tab2
       SELECT * FROM tab1
               MINUS SELECT * FROM tab2;
   END
   ;
   /

**Example: Input: CREATE TABLE AS with DATA AND STATISTICS**

::

   CREATE SET VOLATILE TABLE tab2025
    AS ( SELECT * from tab2023 )
    WITH DATA AND STATISTICS
    PRIMARY INDEX (LOGTYPE, OPERSEQ);

**Output**

::

   CREATE LOCAL TEMPORARY TABLE tab2025
    DISTRIBUTE BY HASH ( LOGTYPE, OPERSEQ )
    AS ( SELECT * FROM tab2023 );

    ANALYZE tab2025;

.. _en-us_topic_0000001188362598__en-us_topic_0238518354_en-us_topic_0237362205_section107711645192617:

CHARACTER SET and CASESPECIFIC
------------------------------

CHARACTER SET is used to specify the server character set for a character column. CASESPECIFIC specifies the case for character data comparisons and collations.

Use the :ref:`tdMigrateCharsetCase <en-us_topic_0000001233922159__en-us_topic_0218440346_li245515470479>` configuration parameter to configure migration of CHARACTER SET and CASESPECIFIC. If tdMigrateCharsetCase is set to false, the tool will skip migration of the query and will log a message.

**Input (tdMigrateCharsetCase=True)**

::

   CREATE MULTISET VOLATILE TABLE TAB1
   (
     col1   INTEGER     NOT NULL
     ,col2     INTEGER     NOT NULL
     ,col3    VARCHAR(100)  NOT NULL CHARACTER SET UNICODE CASESPECIFIC )
   PRIMARY INDEX (col1,col2)
   ON COMMIT PRESERVE ROWS
   ;

**Output**

::

   CREATE LOCAL TEMPORARY TABLE TMP_RATING_SYS_PARA
   (
     col1   INTEGER     NOT NULL
     ,col2     INTEGER     NOT NULL
     ,col3    VARCHAR(100)  NOT NULL /* CHARACTER SET UNICODE CASESPECIFIC  */)
   )
   ON COMMIT PRESERVE ROWS
    DISTRIBUTE BY HASH (col1,col2)

   ;

**Input**\ ``-``\ **Migration support for Character-based data type**

In Teradata, the following character sets support character-based length for string data types:

-  LATIN

-  UNICODE

-  GRAPHIC

   However, the KANJISJIS character set support byte-based length for string data types.

   For example, COLUMN_NAME VARCHAR(100) CHARACTER SET UNICODE CASESPECIFIC COLUMN_NAME VARCHAR(100) CHARACTER SET LATIN CASESPECIFIC This can store up to 100 characters (not bytes).

   In GaussDB(DWS), string data types are byte-based (not character-based). VARCHAR (100) and VARCHAR2 (100) can store up to 100 byte (not characters). However, NVARCHAR2 (100) can store up to 100 characters.

   So, if TD's LATIN, UNICODE and GRAPHIC character sets, VARCHAR should be migrated to NVARCHAR.

::

   CREATE TABLE tab1
   (
       col1 VARCHAR(10),
       COL2 CHAR(1)
   );

Output

::

   a)when default_charset = UNICODE/GRAPHIC
   CREATE
        TABLE
             tab1 (
                  col1 NVARCHAR2 (10)
                  ,COL2 NVARCHAR2 (1)
             ) ;

   b)when default_charset = LATIN
   CREATE
        TABLE
             tab1 (
                  col1 VARCHAR2 (10)
                  ,COL2 VARCHAR2 (1)
             ) ;

**Input**

::

   CREATE TABLE tab1
   (
       col1 VARCHAR(10) CHARACTER SET UNICODE,
       COL2 CHAR(1)
   );

**Output**

::

   a) when default_charset = UNICODE/GRAPHIC
   CREATE
        TABLE
             tab1 (
                  col1 NVARCHAR2 (10) /* CHARACTER SET UNICODE*/
                  ,COL2 NVARCHAR2( 1 )
             ) ;

   b) when default_charset = LATIN
   CREATE
        TABLE
             tab1 (
                  col1 NVARCHAR2 (10) /* CHARACTER SET UNICODE*/
                  ,COL2 CHAR(1)
             )

.. _en-us_topic_0000001188362598__en-us_topic_0238518354_en-us_topic_0237362205_section0835779292:

VOLATILE
--------

The table-specific keyword **VOLATILE** is provided in the input file, but the keyword is not supported by GaussDB(DWS). The tool replaces it with the **LOCAL TEMPORARY** keyword during the migration process. Volatile tables are migrated as local temporary or unlogged based on the config input.

**Input: CREATE VOLATILE TABLE**

::

    CREATE VOLATILE TABLE T1 (c1 int ,c2 int);

**Output**

::

   CREATE
       LOCAL TEMPORARY TABLE
       T1 (
            c1 INTEGER
           ,c2 INTEGER
          )
   ;

**Input: CREATE VOLATILE TABLE AS WITH DATA** (session_mode=Teradata)

If the source table has a PRIMARY KEY or a UNIQUE CONSTRAINT, then it will not contain any duplicate records. In this case, the MINUS operator is not required or added to remove duplicate records.

::

   CREATE VOLATILE TABLE tabV1 (
           C1 INTEGER DEFAULT 99
          ,C2 INTEGER
          ,C3 INTEGER
          ,C4 NUMERIC (20,0) DEFAULT NULL (BIGINT)
          ,CONSTRAINT XX1 PRIMARY KEY ( C1, C2 )
          ) PRIMARY INDEX (C1, C3 );

   CREATE TABLE tabV2 AS tabV1 WITH DATA PRIMARY INDEX (C1)
                ON COMMIT PRESERVE ROWS;

**Output**

::

   CREATE LOCAL TEMPORARY TABLE tabV1 (
           C1 INTEGER DEFAULT 99
          ,C2 INTEGER
          ,C3 INTEGER
          ,C4 NUMERIC (20,0) DEFAULT CAST( NULL AS BIGINT )
          ,CONSTRAINT XX1 PRIMARY KEY ( C1, C2 )
          ) DISTRIBUTE BY HASH (C1);

   BEGIN
       CREATE TABLE tabV2 (
                  LIKE tabV1 INCLUDING ALL EXCLUDING PARTITION EXCLUDING RELOPTIONS EXCLUDING DISTRIBUTION
                          ) DISTRIBUTE BY HASH (C1);
       INSERT INTO tabV2 SELECT * FROM tabV1;
   END
   ;
   /

.. _en-us_topic_0000001188362598__en-us_topic_0238518354_en-us_topic_0237362205_section184410717305:

SET
---

**SET** is a unique feature in Teradata. It does not allow duplicate records. It is addressed using the **MINUS** set operator. Migration tool supports MULTISET and SET tables. SET table can be used with VOLATILE.

**Input: SET TABLE**

::

   CREATE SET VOLATILE TABLE tab1 … ;
   INSERT INTO tab1
   SELECT expr1, expr2, …
     FROM tab1, …
    WHERE ….;

**Output**

::

   CREATE LOCAL TEMPORARY TABLE tab1
   … ; INSERT INTO tab1
    SELECT expr1, expr2, …
   FROM tab1, …
   WHERE ….
   MINUS
   SELECT * FROM tab1 ;

.. _en-us_topic_0000001188362598__en-us_topic_0238518354_en-us_topic_0237362205_section1039193943014:

MULTISET
--------

**MULTISET** is a normal table, which is supported by all the DBs. Migration tool supports MULTISET and SET tables.

MULTISET table can be used with VOLATILE.

**Input: CREATE MULTISET TABLE**

::

    CREATE VOLATILE MULTISET TABLE T1 (c1 int ,c2 int);

**Output**

::

   CREATE
       LOCAL TEMPORARY TABLE
       T1 (
            c1 INTEGER
           ,c2 INTEGER
          )
   ;

.. _en-us_topic_0000001188362598__en-us_topic_0238518354_en-us_topic_0237362205_section164902010315:

TITLE
-----

The keyword **TITLE** is supported for Teradata Permanent, Global Temporary and Volatile tables. In the migration process, the TITLE text is migrated as a comment.

.. note::

   If the TITLE text is split across multiple lines, then in the migrated script, the line breaks (ENTER) are replaced with a space.

**Input: CREATE TABLE with TITLE**

::

   CREATE TABLE tab1 (
     c1  NUMBER(2) TITLE 'column_a'
   );

**Output**

::

   CREATE TABLE tab1 (
     c1  NUMBER(2) /* TITLE 'column_a' */
   );

**Input: TABLE with multiline TITLE**

::

   CREATE TABLE tab1 (
     c1  NUMBER(2) TITLE 'This is a
   very long title'
   );

**Output**

::

   CREATE TABLE tab1 (
     c1  NUMBER(2) /* TITLE 'This is a  very long title' */
   );

**Input: TABLE with COLUMN TITLE**

DSC migrates COLUMN TITLE as a new outer query.

::

   SELECT customer_id (TITLE 'cust_id')
   FROM Customer_T
   WHERE cust_id > 10;

**Output**

::

   SELECT
             customer_id  AS "cust_id"
        FROM
             (
                  SELECT
                            customer_id
                       FROM
                            Customer_T
                       WHERE
                            cust_id > 10
             )
   ;

**Input: TABLE with COLUMN TITLE and QUALIFY**

::

   SELECT ord_id
   (TITLE 'Order_Id'), order_date, customer_id
     FROM order_t
   WHERE Order_Id > 100
   QUALIFY ROW_NUMBER() OVER(PARTITION BY customer_id ORDER BY order_date DESC) <= 5;

**Output**

.. code-block::

   SELECT
             "mig_tmp_alias1" AS "Order_Id"
        FROM
             (
                  SELECT
                            ord_id AS "mig_tmp_alias1"
                            ,ROW_NUMBER( ) OVER( PARTITION BY customer_id ORDER BY order_date DESC ) AS ROW_NUM1
                       FROM
                            order_t
                       WHERE
                            Order_Id > 100
             ) Q1
        WHERE
             Q1.ROW_NUM1 <= 5
   ;

#. TITLE with ALIAS

   If the TITLE is accompanied with an ALIAS, the tool will migrate it as follows:

   -  **TITLE with AS**: Tool will migrate it with the AS alias.
   -  **TITLE with NAMED:** Tool will migrate it with NAMED alias.
   -  **TITLE with NAMED and AS**: Tool will migrate it with AS alias.

   **Input: TABLE TITLE with NAMED and AS**

   ::

      SELECT  Acct_ID (TITLE 'Acc Code') (NAMED XYZ)  AS "Account Code"
              ,Acct_Name (TITLE 'Acc Name')
      FROM    GT_JCB_01030_Acct_PBU
      where "Account Code" > 500  group by "Account Code" ,Acct_Name ;

   **Output**

   .. code-block::

      SELECT
                Acct_ID AS "Account Code"
                ,Acct_Name AS "Acc Name"
           FROM
                GT_JCB_01030_Acct_PBU
           WHERE
                Acct_ID > 500
           GROUP BY
                Acct_ID ,Acct_Name
      ;

   .. note::

      Currently the Migration tool supports the migration of the TITLE command included in the initial CREATE/ALTER statement. The subsequent references of the TITLE specified column are not supported. For example, in the CREATE TABLE statement below, the column **eid** with the TITLE Employee ID will be migrated to a comment but the reference of **eid** in the SELECT statement will be retained as it is.

      Input

      ::

         CREATE TABLE tab1 ( eid INT TITLE 'Employee ID');
         SELECT eid FROM tab1;

      Output

      ::

         CREATE TABLE tab1 (eid INT /*TITLE 'Employee ID'*/);
         SELECT eid from tab1;

#. TITLE with CREATE VIEW

   **Input**

   .. code-block::

      REPLACE VIEW ${STG_VIEW}.B971_AUMSUMMARY${TABLE_SUFFIX_INC}
      AS
      LOCK TABLE ${STG_DATA}.B971_AUMSUMMARY${TABLE_SUFFIX_INC} FOR ACCESS
      SELECT   AUM_DATE (TITLE '    ')
            ,CLNTCODE (TITLE '    ')
            ,ACCTYPE (TITLE '    ')
            ,CCY (TITLE '  ')
            ,BAL_AMT (TITLE '  ')
            ,MON_BAL_AMT (TITLE '    ')
            ,HK_CLNTCODE (TITLE '   ')
            ,MNT_DATE (TITLE '    ')
      FROM ${STG_DATA}.B971_AUMSUMMARY${TABLE_SUFFIX_INC};
      it should be migrated as below:
      CREATE OR REPLACE VIEW ${STG_VIEW}.B971_AUMSUMMARY${TABLE_SUFFIX_INC}
      AS
      /*LOCK TABLE ${STG_DATA}.B971_AUMSUMMARY${TABLE_SUFFIX_INC} FOR ACCESS */
      SELECT   AUM_DATE  /* (TITLE '    ') */
            ,CLNTCODE  /* (TITLE '    ') */
            ,ACCTYPE  /* (TITLE '    ') */
            ,CCY  /* (TITLE '  ') */
            ,BAL_AMT  /* (TITLE '  ') */
            ,MON_BAL_AMT  /* (TITLE '    ') */
            ,HK_CLNTCODE  /* (TITLE '   ') */
            ,MNT_DATE  /* (TITLE '    ') */
      FROM ${STG_DATA}.B971_AUMSUMMARY${TABLE_SUFFIX_INC};

   **Output**

   .. code-block::

      CREATE OR REPLACE VIEW ${STG_VIEW}.B971_AUMSUMMARY${TABLE_SUFFIX_INC}
      AS
      /*LOCK TABLE ${STG_DATA}.B971_AUMSUMMARY${TABLE_SUFFIX_INC} FOR ACCESS */
      SELECT   AUM_DATE  /* (TITLE '    ') */
            ,CLNTCODE  /* (TITLE '    ') */
            ,ACCTYPE  /* (TITLE '    ') */
            ,CCY  /* (TITLE '  ') */
            ,BAL_AMT  /* (TITLE '  ') */
            ,MON_BAL_AMT  /* (TITLE '    ') */
            ,HK_CLNTCODE  /* (TITLE '   ') */
            ,MNT_DATE  /* (TITLE '    ') */
      FROM ${STG_DATA}.B971_AUMSUMMARY${TABLE_SUFFIX_INC};

.. _en-us_topic_0000001188362598__en-us_topic_0238518354_en-us_topic_0237362205_section897124714316:

INDEX
-----

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

**Input - CREATE TABLE with Primary Key and Non-Unique Primary Index (DISTRIBUTE BY HASH is not added)**

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

.. _en-us_topic_0000001188362598__en-us_topic_0238518354_en-us_topic_0237362205_section5984610123218:

CONSTRAINT
----------

A table CONSTRAINT is applied to multiple columns. Migration tool supports the following constraints:

-  REFERENCES constraint / FOREIGN KEY: migration currently NOT supported by tool.
-  PRIMARY KEY constraint: migration supported by tool.
-  UNIQUE constraint: migration supported by tool.

**Input: CREATE TABLE with CONSTRAINT**

::

   CREATE SET TABLE DP_SEDW.T_170UT_HOLDER_ACCT, NO FALLBACK,
      NO BEFORE JOURNAL, NO AFTER JOURNAL
    ( BUSINESSDATE   VARCHAR(10)
    , SOURCESYSTEM   VARCHAR(5)
    , UPLOADCODE     VARCHAR(1)
    , HOLDER_NO      VARCHAR(7)  NOT NULL
    , POSTAL_ADD_4   VARCHAR(40)
    , EPF_IND         CHAR(1)
    , CONSTRAINT uq_t_170ut_hldr UNIQUE ( SOURCESYSTEM, UPLOADCODE, HOLDER_NO )
         ) PRIMARY INDEX ( HOLDER_NO, SOURCESYSTEM ) ;

**Output**

::

   CREATE TABLE DP_SEDW.T_170UT_HOLDER_ACCT
      ( BUSINESSDATE      VARCHAR( 10 )
      , SOURCESYSTEM      VARCHAR( 5 )
      , UPLOADCODE        VARCHAR( 1 )
      , HOLDER_NO         VARCHAR( 7 )   NOT NULL
      , POSTAL_ADD_4      VARCHAR( 40 )
      , EPF_IND           CHAR( 1 )
      , CONSTRAINT uq_t_170ut_hldr UNIQUE ( SOURCESYSTEM, UPLOADCODE, HOLDER_NO )
              )
   DISTRIBUTE BY HASH ( HOLDER_NO, SOURCESYSTEM );

**Input**

After table creation, CONSTRAINT can be added to a table column to put some restriction at column level by using ALTER statement.

::

   CREATE TABLE GCC_PLAN.T1033 ( ROLLOUT_PLAN_LINE_ID NUMBER NOT NULL,
                                                               UDF_FIELD_VALUE_ID NUMBER NOT NULL) ;
   ALTER TABLE GCC_PLAN.T1033
   ADD CONSTRAINT UDF_FIELD_VALUE_ID_PK UNIQUE (UDF_FIELD_VALUE_ID) ;

**Output**

::

   CREATE TABLE GCC_PLAN.T1033 ( ROLLOUT_PLAN_LINE_ID NUMBER NOT NULL,
                                                               UDF_FIELD_VALUE_ID NUMBER NOT NULL,
                                                               CONSTRAINT UDF_FIELD_VALUE_ID_PK
                                                               UNIQUE (UDF_FIELD_VALUE_ID) ;

.. note::

   Need to put CONSTRAINT creation syntax inside table creation script after all column declaration.

.. _en-us_topic_0000001188362598__en-us_topic_0238518354_en-us_topic_0237362205_section07421933153219:

COLUMN STORE
------------

The table orientation can be converted from ROW-STORE to COLUMN store using the WITH (ORIENTATION=COLUMN) in the CREATE TABLE statement. This feature can be enabled/disabled using the :ref:`rowstoreToColumnstore <en-us_topic_0000001233922159__en-us_topic_0218440346_li1639915513325>` configuration parameter.

**Input: CREATE TABLE with change orientation to COLUMN STORE**

::

   CREATE MULTISET VOLATILE TABLE tab1
         ( c1 VARCHAR(30) CHARACTER SET UNICODE
         , c2 DATE
         , ...
         )
    PRIMARY INDEX (c1, c2)
    ON COMMIT PRESERVE ROWS;

**Output**

::

   CREATE LOCAL TEMPORARY TABLE tab1
        ( c1 VARCHAR(30)
        , c2 DATE
        , ...
        ) WITH (ORIENTATION = COLUMN)
     ON COMMIT PRESERVE ROWS
     DISTRIBUTE BY HASH (c1, c2);

.. _en-us_topic_0000001188362598__en-us_topic_0238518354_en-us_topic_0237362205_section1312517610339:

PARTITION
---------

The tool does not support migration of partitions/subpartitions and the partition/subpartition keywords are commented in the migrated scripts:

-  Range partition/subpartition
-  List partition/subpartition
-  Hash partition/subpartition

Scenario 1: Assume that the configuration parameters (:ref:`tdMigrateCASE_N <en-us_topic_0000001233922159__en-us_topic_0218440346_li33711169269>` and :ref:`tdMigrateRANGE_N <en-us_topic_0000001233922159__en-us_topic_0218440346_li143711916152611>`) are set to **comment** or **range** respectively.

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

Scenario 2: Assume that the configuration parameters (:ref:`tdMigrateCASE_N <en-us_topic_0000001233922159__en-us_topic_0218440346_li33711169269>` and :ref:`tdMigrateRANGE_N <en-us_topic_0000001233922159__en-us_topic_0218440346_li143711916152611>`) are set to **comment** or **range** respectively.

The following is another Teradata CREATE TABLE script with nested partitions.

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

Scenario 3: Assume that the configuration parameters (:ref:`tdMigrateCASE_N <en-us_topic_0000001233922159__en-us_topic_0218440346_li33711169269>` and :ref:`tdMigrateRANGE_N <en-us_topic_0000001233922159__en-us_topic_0218440346_li143711916152611>`) are set to values other than **comment** or **range** respectively.

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

Scenario 4: Assume that the configuration parameters (:ref:`tdMigrateCASE_N <en-us_topic_0000001233922159__en-us_topic_0218440346_li33711169269>` and :ref:`tdMigrateRANGE_N <en-us_topic_0000001233922159__en-us_topic_0218440346_li143711916152611>`) are set to any value.

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

Scenario 5: Assume that the configuration parameters (:ref:`tdMigrateCASE_N <en-us_topic_0000001233922159__en-us_topic_0218440346_li33711169269>` and :ref:`tdMigrateRANGE_N <en-us_topic_0000001233922159__en-us_topic_0218440346_li143711916152611>`) are set to **comment** or **range** respectively.

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

.. _en-us_topic_0000001188362598__en-us_topic_0238518354_en-us_topic_0237362205_section18781737133310:

ANALYZE
-------

**Input - CREATE TABLE with INDEX**

::

   CREATE TABLE EMP27 AS emp21 WITH DATA
   PRIMARY INDEX (EMPNO) ON COMMIT PRESERVE ROWS;

**Output**

.. code-block::

   Begin
   CREATE TABLE EMP27
   ( LIKE emp21 INCLUDING ALL EXCLUDING PARTITION EXCLUDING RELOPTIONS EXCLUDING
   DISTRIBUTION )
   DISTRIBUTE BY HASH ( EMPNO ) ;
   INSERT INTO EMP27
   select * from emp21 ;
   end ;
   /
   ANALYZE Emp27 (EmpNo);

.. _en-us_topic_0000001188362598__en-us_topic_0238518354_en-us_topic_0237362205_section1139515893319:

Data Types
----------

The following data type mappings are supported by the DSC.

+-----------------------------------------------------+--------------------------------+
| Input                                               | Output                         |
+=====================================================+================================+
| **Numeric**                                         | **Numeric**                    |
+-----------------------------------------------------+--------------------------------+
| BIGINT                                              | BIGINT                         |
+-----------------------------------------------------+--------------------------------+
| BYTEINT                                             | SMALLINT                       |
+-----------------------------------------------------+--------------------------------+
| DECIMAL [(n[,m])]                                   | DECIMAL [(n[,m])]              |
+-----------------------------------------------------+--------------------------------+
| DOUBLE PRECISION                                    | DOUBLE PRECISION               |
+-----------------------------------------------------+--------------------------------+
| FLOAT                                               | DOUBLE PRECISION               |
+-----------------------------------------------------+--------------------------------+
| INT / INTEGER                                       | INTEGER                        |
+-----------------------------------------------------+--------------------------------+
| NUMBER / NUMERIC                                    | NUMERIC                        |
+-----------------------------------------------------+--------------------------------+
| NUMBER(n[,m])                                       | NUMERIC (n[,m])                |
+-----------------------------------------------------+--------------------------------+
| REAL                                                | REAL                           |
+-----------------------------------------------------+--------------------------------+
| SMALLINT                                            | SMALLINT                       |
+-----------------------------------------------------+--------------------------------+
| **Character**                                       | **Character**                  |
+-----------------------------------------------------+--------------------------------+
| CHAR[(n)] / CHARACTER [(n)]                         | CHAR(n)                        |
+-----------------------------------------------------+--------------------------------+
| CLOB                                                | CLOB                           |
+-----------------------------------------------------+--------------------------------+
| LONG VARCHAR                                        | TEXT                           |
+-----------------------------------------------------+--------------------------------+
| VARCHAR(n) / CHAR VARYING(n) / CHARACTER VARYING(n) | VARCHAR(n)                     |
+-----------------------------------------------------+--------------------------------+
| **DateTime**                                        | **DateTime**                   |
+-----------------------------------------------------+--------------------------------+
| DATE                                                | DATE                           |
+-----------------------------------------------------+--------------------------------+
| TIME [(n)]                                          | TIME [(n)]                     |
+-----------------------------------------------------+--------------------------------+
| TIME [(n)] WITH TIME ZONE                           | TIME [(n)] WITH TIME ZONE      |
+-----------------------------------------------------+--------------------------------+
| TIMESTAMP [(n)]                                     | TIMESTAMP [(n)]                |
+-----------------------------------------------------+--------------------------------+
| TIMESTAMP [(n)] WITH TIME ZONE                      | TIMESTAMP [(n)] WITH TIME ZONE |
+-----------------------------------------------------+--------------------------------+
| **Period**                                          | **Period**                     |
+-----------------------------------------------------+--------------------------------+
| PERIOD(DATE)                                        | daterange                      |
+-----------------------------------------------------+--------------------------------+
| PERIOD(TIME [(n)])                                  | tsrange [(n)]                  |
+-----------------------------------------------------+--------------------------------+
| PERIOD(TIME WITH TIME ZONE)                         | tstzrange                      |
+-----------------------------------------------------+--------------------------------+
| PERIOD(TIMESTAMP [(n)])                             | tsrange [(n)]                  |
+-----------------------------------------------------+--------------------------------+
| PERIOD(TIMESTAMP WITH TIME ZONE)                    | tstzrange                      |
+-----------------------------------------------------+--------------------------------+
| **Binary**                                          | **Binary**                     |
+-----------------------------------------------------+--------------------------------+
| BLOB[(n)]                                           | blob                           |
+-----------------------------------------------------+--------------------------------+
| BYTE[(n)]                                           | bytea                          |
+-----------------------------------------------------+--------------------------------+
| VARBYTE[(n)]                                        | bytea                          |
+-----------------------------------------------------+--------------------------------+

For example: BYTEINT

**Input**

.. code-block::

   select cast(col as byteint) from tab;

**Output**

.. code-block::

    SELECT CAST( col AS SMALLINT ) FROM tab ;

.. _en-us_topic_0000001188362598__en-us_topic_0238518354_en-us_topic_0237362205_section883201463413:

Support for Specified Columns
-----------------------------

Migration tool supports queries that specify number of columns (not all columns specified) during INSERT. This can happen when the input INSERT statement does not contain all the columns mentioned in the input CREATE statement. During migration, the columns are added with any default values specified.

.. note::

   This feature is supported if :ref:`session_mode <en-us_topic_0000001233922159__en-us_topic_0218440346_li9493135323214>` is **Teradata**.

   -  The SELECT statement for the INSERT-INTO-SELECT must not include the following:

      -  Set operators
      -  MERGE, TOP with PERCENT, TOP PERCENT with TIES

**Input - TABLE with all columns of CREATE are not specified in the INSERT statement**

::

   CREATE
        VOLATILE TABLE
             Convert_Data3
             ,NO LOG (
                  zoneno CHAR( 6 )
                  ,brno CHAR( 6 )
                  ,currtype CHAR( 4 )
                  ,Commuteno CHAR( 4 )
                  ,Subcode CHAR( 12 )
                  ,accdate DATE format 'YYYY-MM-DD' NOT NULL
                  ,acctime INTEGER
                  ,quoteno CHAR( 1 )
                  ,quotedate DATE FORMAT 'YYYY-MM-DD'
                  ,lddrbaL DECIMAL( 18 ,0 ) DEFAULT 0
                  ,ldcrbal DECIMAL( 18 ,0 )
                  ,tddramt DECIMAL( 18 ,0 ) DEFAULT 25
                  ,tdcramt DECIMAL( 18 ,0 )
                  ,tddrbal DECIMAL( 18 ,2 )
                  ,tdcrbal DECIMAL( 18 ,2 )
             ) PRIMARY INDEX (
                  BRNO
                  ,CURRTYPE
                  ,SUBCODE
             )
                  ON COMMIT PRESERVE ROWS
   ;

   INSERT
        INTO
             Convert_Data3 (
                  zoneno
                  ,brno
                  ,currtype
                  ,commuteno
                  ,subcode
                  ,accdate
                  ,acctime
                  ,quoteno
                  ,quotedate
                  ,tddrbal
                  ,tdcrbal
             ) SELECT
                       A.zoneno
                       ,A.brno
                       ,'014' currtype
                       ,'2' commuteno
                       ,A.subcode
                       ,A.Accdate
                       ,A.Acctime
                       ,'2' quoteno
                       ,B.workdate quoteDate
                       ,CAST( ( CAST( SUM ( CAST( A.tddrbal AS FLOAT ) * CAST( B.USCVRATE AS FLOAT ) ) AS FLOAT ) ) AS DEC ( 18 ,2 ) ) AS tddrbal
                       ,CAST( ( CAST( SUM ( CAST( A.tdcrbal AS FLOAT ) * CAST( B.USCVRATE AS FLOAT ) ) AS FLOAT ) ) AS DEC ( 18 ,2 ) ) AS tdcrbal
                  FROM
                       table2 A
   ;

**Output**

::

   CREATE
        LOCAL TEMPORARY TABLE
             Convert_Data3 (
                  zoneno CHAR( 6 )
                  ,brno CHAR( 6 )
                  ,currtype CHAR( 4 )
                  ,Commuteno CHAR( 4 )
                  ,Subcode CHAR( 12 )
                  ,accdate DATE NOT NULL
                  ,acctime INTEGER
                  ,quoteno CHAR( 1 )
                  ,quotedate DATE
                  ,lddrbaL DECIMAL( 18 ,0 ) DEFAULT 0
                  ,ldcrbal DECIMAL( 18 ,0 )
                  ,tddramt DECIMAL( 18 ,0 ) DEFAULT 25
                  ,tdcramt DECIMAL( 18 ,0 )
                  ,tddrbal DECIMAL( 18 ,2 )
                  ,tdcrbal DECIMAL( 18 ,2 )
             )
                  ON COMMIT PRESERVE ROWS DISTRIBUTE BY HASH (
                  BRNO
                  ,CURRTYPE
                  ,SUBCODE
             )
   ;

   INSERT
        INTO
             Convert_Data3 (
                  lddrbaL
                  ,ldcrbal
                  ,tddramt
                  ,tdcramt
                  ,zoneno
                  ,brno
                  ,currtype
                  ,commuteno
                  ,subcode
                  ,accdate
                  ,acctime
                  ,quoteno
                  ,quotedate
                  ,tddrbal
                  ,tdcrbal
             ) SELECT
                       0
                       ,NULL
                       ,25
                       ,NULL
                       ,A.zoneno
                       ,A.brno
                       ,'014' currtype
                       ,'2' commuteno
                       ,A.subcode
                       ,A.Accdate
                       ,A.Acctime
                       ,'2' quoteno
                       ,B.workdate quoteDate
                       ,CAST( ( CAST( SUM ( CAST( A.tddrbal AS FLOAT ) * CAST( B.USCVRATE AS FLOAT ) ) AS FLOAT ) ) AS DECIMAL( 18 ,2 ) ) AS tddrbal
                       ,CAST( ( CAST( SUM ( CAST( A.tdcrbal AS FLOAT ) * CAST( B.USCVRATE AS FLOAT ) ) AS FLOAT ) ) AS DECIMAL( 18 ,2 ) ) AS tdcrbal
                  FROM
                       table2 A MINUS SELECT
                                 lddrbaL
                                 ,ldcrbal
                                 ,tddramt
                                 ,tdcramt
                                 ,zoneno
                                 ,brno
                                 ,currtype
                                 ,commuteno
                                 ,subcode
                                 ,accdate
                                 ,acctime
                                 ,quoteno
                                 ,quotedate
                                 ,tddrbal
                                 ,tdcrbal
                            FROM
                                 CONVERT_DATA3
   ;
