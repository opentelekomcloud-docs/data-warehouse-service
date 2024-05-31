:original_name: dws_16_0064.html

.. _dws_16_0064:

.. _en-us_topic_0000001819416141:

CHARACTER SET and CASESPECIFIC
==============================

CHARACTER SET is used to specify the server character set for a character column. CASESPECIFIC specifies the case for character data comparisons and collations.

Use the :ref:`tdMigrateCharsetCase <en-us_topic_0000001819416085__en-us_topic_0000001706224349_en-us_topic_0000001432527901_li245515470479>` configuration parameter to configure migration of CHARACTER SET and CASESPECIFIC. If **tdMigrateCharsetCase** is set to **false**, the tool will skip migration of the query and will log a message.

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

   In GaussDB A, string data types are byte-based (not character-based). VARCHAR (100) and VARCHAR2 (100) can store up to 100 byte (not characters). However, NVARCHAR2 (100) can store up to 100 characters.

   So, if TD's LATIN, UNICODE and GRAPHIC character sets, VARCHAR should be migrated to NVARCHAR.

::

   CREATE TABLE tab1
   (
       col1 VARCHAR(10),
       COL2 CHAR(1)
   );

**Output**

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
             ) ;
