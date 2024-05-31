:original_name: dws_mt_0114.html

.. _dws_mt_0114:

.. _en-us_topic_0000001819416309:

Database Keywords
=================

DSC supports GaussDB(DWS) keywords, such as **NAME**, **LIMIT**, **OWNER**, **KEY**, and **CAST**. These keywords must be enclosed in double quotation marks.

Gauss Keywords (NAME, VERSION, LABEL, POSITION)
-----------------------------------------------

The keywords **NAME**, **VERSION**, **LABEL**, and **POSITION** are changed to **AS**\ *Keyword*.

**Input - NAME, VERSION, LABEL, POSITION**

::

   SELECT id, NAME,label,description
           FROM (SELECT a.id             id,
                        b.NAME           NAME,
                        b.description    description,
                        b.default_label  label,
                        ROWNUM           ROW_ID
                   FROM CTP_ITEM A
                   LEFT OUTER JOIN CTP_ITEM_NLS B ON A.ID = B.ID
                                                 AND B.LOCALE = i_language
                  ORDER BY a.id ASC)
          WHERE ROW_ID >= to_number(begNum)
            AND ROW_ID < to_number(begNum) + to_number(fetchNum);

   SELECT DISTINCT REPLACE(VERSION,' ','') ID, VERSION TEXT
           FROM (SELECT T1.SOFTASSETS_NAME, T2.VERSION
                   FROM SPMS_SOFT_ASSETS T1, SPMS_SYSSOFT_ASSETS T2
                  WHERE T1.SOFTASSETS_ID = T2.SOFTASSETS_ID)
             WHERE SOFTASSETS_NAME = I_SOFT_NAME;

   SELECT COUNTRY, AMOUNT
             FROM (SELECT '' COUNTRY || '' AMOUNT, '1' POSITION
                     FROM DUAL )
            ORDER BY POSITION;

**Output**

::

   SELECT id,NAME,label,description FROM (
    SELECT a.id id,b.NAME AS NAME,
    b.description description
    ,b.default_label AS label,
    ROW_NUMBER( ) OVER( ) ROW_ID
    FROM CTP_ITEM A LEFT OUTER JOIN
    CTP_ITEM_NLS B
    ON A.ID = B.ID AND
    B.LOCALE = i_language
    ORDER BY a.id ASC) WHERE
    ROW_ID >= to_number( begNum )
    AND
    ROW_ID < to_number( begNum ) + to_number( fetchNum )
   ;

   SELECT
     DISTINCT REPLACE( VERSION ,' ' ,'' ) ID
        ,VERSION AS TEXT
        FROM
           (
            SELECT
            T1.SOFTASSETS_NAME
            ,T2.VERSION
            FROM
           SPMS_SOFT_ASSETS T1
          ,SPMS_SYSSOFT_ASSETS T2
           WHERE
           T1.SOFTASSETS_ID = T2.SOFTASSETS_ID
          )
         WHERE SOFTASSETS_NAME = I_SOFT_NAME ;



   SELECT COUNTRY ,AMOUNT
   FROM ( SELECT '' COUNTRY || '' AMOUNT
           ,'1' AS POSITION
             FROM
            DUAL
          )
        ORDER BY
        POSITION
   ;

TEXT & YEAR
-----------

**Input - TEXT, YEAR**

::

   SELECT
     NAME,
     VALUE,
     DESCRIPTION TEXT,
     JOINED YEAR,
     LIMIT
   FROM
     EMPLOYEE;

   SELECT
     NAME,
     TEXT,
     YEAR,
     VALUE,
     DESCRIPTION,
     LIMIT
   FROM
     EMPLOYEE_DETAILS;

**Output**

::

   SELECT
     "NAME",
     VALUE,
     DESCRIPTION AS TEXT,
     JOINED AS YEAR,
     "LIMIT"
   FROM
     EMPLOYEE;

   SELECT
     "NAME",
     "TEXT",
     "YEAR",
     VALUE,
     DESCRIPTION,
     "LIMIT"
   FROM
     EMPLOYEE_DETAILS;

NAME and LIMIT
--------------

**Input: GaussDB(DWS) keywords NAME and LIMIT**

::

   CREATE TABLE NAME
         ( NAME VARCHAR2(50) NOT NULL
         , VALUE VARCHAR2(255)
         , DESCRIPTION VARCHAR2(4000)
         , LIMIT NUMBER(9)
         )
     /*TABLESPACE users*/
     pctfree 10   initrans 1   maxtrans
     255
        storage ( initial 256K next 256K
        minextents 1 maxextents
        unlimited );

    SELECT NAME, VALUE, DESCRIPTION, LIMIT
      FROM NAME;

**Output**

::

   CREATE TABLE "NAME"
            ( "NAME" VARCHAR2 (50) NOT NULL
            , VALUE VARCHAR2 (255)
            , DESCRIPTION VARCHAR2 (4000)
            , "LIMIT" NUMBER (9)
            )
         /*TABLESPACE users*/
         pctfree 10 initrans 1 maxtrans 255
         storage ( initial 256 K NEXT 256 K minextents 1
         maxextents unlimited );

    SELECT "NAME", VALUE, DESCRIPTION, "LIMIT"
      FROM "NAME";

OWNER
-----

**Bulk Operations**

**Input: Use SELECT to query the GaussDB(DWS) keyword OWNER**

::

   SELECT
             owner
        FROM
             Test_Col;

**Output**

::

   SELECT
             "OWNER"
        FROM
             Test_Col;

**Input: Use DELETE to query the GaussDB(DWS) keyword OWNER**

.. code-block:: text

   DELETE FROM emp14
        WHERE
             ename = 'Owner';

**Input**

.. code-block:: text

   DELETE FROM emp14
        WHERE
             ename = 'Owner'

KEY
---

**Blogic Operations**

**Input: GaussDB(DWS) keyword KEY**

::

   CREATE
        OR REPLACE FUNCTION myfct RETURN VARCHAR2 parallel_enable IS res VARCHAR2 ( 200 ) ;
        BEGIN
             res := 100 ;
             INSERT INTO emp18 RW ( RW.empno ,RW.ename ) SELECT
                  res ,RWN.ename KEY
             FROM
                  emp16 RWN ;
                  COMMIT ;
             RETURN res ;
   END ;
   /

**Output**

::

   CREATE
        OR REPLACE FUNCTION myfct RETURN VARCHAR2 IS res VARCHAR2 ( 200 ) ;
        BEGIN
             res := 100 ;
             INSERT INTO emp18 ( empno ,ename ) SELECT
                  res ,RWN.ename "KEY"
             FROM
                  emp16 RWN ;
                  /* COMMIT; */
             null ;
             RETURN res ;
   END ;

Range, Account and Language
---------------------------

When Gauss keywords are used as aliases for any column in the SELECT list without defining the AS keyword, the AS keyword to define the aliases.

**Input**

::

   CREATE
        OR REPLACE /*FORCE*/
        VIEW SAD.FND_TERRITORIES_TL_V (
             TERRITORY_CODE
             ,TERRITORY_SHORT_NAME
             ,LANGUAGE
             ,Account
             ,Range
             ,LAST_UPDATED_BY
             ,LAST_UPDATE_DATE
             ,LAST_UPDATE_LOGIN
             ,DESCRIPTION
             ,SOURCE_LANG
             ,ISO_NUMERIC_CODE
        ) AS SELECT
                  t.TERRITORY_CODE
                  ,t.TERRITORY_SHORT_NAME
                  ,t.LANGUAGE
                  ,t.Account
                  ,t.Range
                  ,t.LAST_UPDATED_BY
                  ,t.LAST_UPDATE_DATE
                  ,t.LAST_UPDATE_LOGIN
                  ,t.DESCRIPTION
                  ,t.SOURCE_LANG
                  ,t.ISO_NUMERIC_CODE
             FROM
                  fnd_territories_tl t
        UNION
        ALL SELECT
                  'SS' TERRITORY_CODE
                  ,'Normal Country' TERRITORY_SHORT_NAME
                  ,NULL LANGUAGE
                  ,NULL Account
                  ,NULL Range
                  ,NULL LAST_UPDATED_BY
                  ,NULL LAST_UPDATE_DATE
                  ,NULL LAST_UPDATE_LOGIN
                  ,NULL DESCRIPTION
                  ,NULL SOURCE_LANG
                  ,NULL ISO_NUMERIC_CODE
             FROM
                  DUAL ;

**Output**

::

   CREATE
        OR REPLACE /*FORCE*/
        VIEW SAD.FND_TERRITORIES_TL_V (
             TERRITORY_CODE
             ,TERRITORY_SHORT_NAME
             ,LANGUAGE
             ,CREATED_BY
             ,CREATION_DATE
             ,LAST_UPDATED_BY
             ,LAST_UPDATE_DATE
             ,LAST_UPDATE_LOGIN
             ,DESCRIPTION
             ,SOURCE_LANG
             ,ISO_NUMERIC_CODE
        ) AS SELECT
                  t.TERRITORY_CODE
                  ,t.TERRITORY_SHORT_NAME
                  ,t.LANGUAGE
                  ,t.CREATED_BY
                  ,t.CREATION_DATE
                  ,t.LAST_UPDATED_BY
                  ,t.LAST_UPDATE_DATE
                  ,t.LAST_UPDATE_LOGIN
                  ,t.DESCRIPTION
                  ,t.SOURCE_LANG
                  ,t.ISO_NUMERIC_CODE
             FROM
                  fnd_territories_tl t
        UNION
        ALL SELECT
                  'SS' TERRITORY_CODE
                  ,'Normal Country' TERRITORY_SHORT_NAME
                  ,NULL AS LANGUAGE
                  ,NULL CREATED_BY
                  ,NULL CREATION_DATE
                  ,NULL LAST_UPDATED_BY
                  ,NULL LAST_UPDATE_DATE
                  ,NULL LAST_UPDATE_LOGIN
                  ,NULL DESCRIPTION
                  ,NULL SOURCE_LANG
                  ,NULL ISO_NUMERIC_CODE
             FROM
                  DUAL ;

Primary Key and Unique Key
--------------------------

If primary and unique keys are declared on table creation, only the primary key needs to consider for migration.

::

   create table SD_WO.WO_DU_TRIGGER_REVENUE_T
   (
     TRIGGER_REVENUE_ID NUMBER not null,
     PROJECT_NUMBER     VARCHAR2(40),
     DU_ID              NUMBER,
     STANDARD_MS_CODE   VARCHAR2(100),
     TRIGGER_STATUS     NUMBER,
     TRIGGER_MSG        VARCHAR2(4000),
     BATCH_NUMBER       NUMBER,
     PROCESS_STATUS     NUMBER,
     ENABLE_FLAG        CHAR(1) default 'Y',
     CREATED_BY         NUMBER,
     CREATION_DATE      DATE,
     LAST_UPDATE_BY     NUMBER,
     LAST_UPDATE_DATE   DATE
   )
   ;

   alter table SD_WO.WO_DU_TRIGGER_REVENUE_T
     add constraint WO_DU_TRIGGER_REVENUE_PK primary key (TRIGGER_REVENUE_ID);
   alter table SD_WO.WO_DU_TRIGGER_REVENUE_T
     add constraint WO_DU_TRIGGER_REVENUE_N1 unique (DU_ID, STANDARD_MS_CODE);

**Output**

.. code-block::

   CREATE
        TABLE
             SD_WO.WO_DU_TRIGGER_REVENUE_T (
                  TRIGGER_REVENUE_ID NUMBER NOT NULL
                  ,PROJECT_NUMBER VARCHAR2 (40)
                  ,DU_ID NUMBER
                  ,STANDARD_MS_CODE VARCHAR2 (100)
                  ,TRIGGER_STATUS NUMBER
                  ,TRIGGER_MSG VARCHAR2 (4000)
                  ,BATCH_NUMBER NUMBER
                  ,PROCESS_STATUS NUMBER
                  ,ENABLE_FLAG CHAR( 1 ) DEFAULT 'Y'
                  ,CREATED_BY NUMBER
                  ,CREATION_DATE DATE
                  ,LAST_UPDATE_BY NUMBER
                  ,LAST_UPDATE_DATE DATE
                  ,CONSTRAINT WO_DU_TRIGGER_REVENUE_PK PRIMARY KEY (TRIGGER_REVENUE_ID)
             ) ;

PROMPT
------

PROMPT should be converted to \\ECHO supported by GAUSS.

+-------------------------------------------+------------------------------------------+
| Oracle Syntax                             | Syntax after Migration                   |
+===========================================+==========================================+
| .. code-block::                           | .. code-block::                          |
|                                           |                                          |
|    prompt                                 |    \echo                                 |
|    prompt Creating table product          |    \echo Creating table product          |
|    prompt =============================== |    \echo =============================== |
|    prompt                                 |    \echo                                 |
|    create table product                   |    CREATE TABLE product                  |
|    (                                      |    (                                     |
|      product_id     VARCHAR2(20),         |      product_id     VARCHAR2(20),        |
|      product_name   VARCHAR2(50)          |      product_name   VARCHAR2(50)         |
|    );                                     |    );                                    |
+-------------------------------------------+------------------------------------------+
