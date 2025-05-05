:original_name: dws_16_0074.html

.. _dws_16_0074:

.. _en-us_topic_0000001813598856:

Support for Specified Columns
=============================

Migration tool supports queries that specify number of columns (not all columns specified) during INSERT. This can happen when the input INSERT statement does not contain all the columns mentioned in the input CREATE statement. During migration, the columns are added with any default values specified.

.. note::

   -  This feature is supported if :ref:`session_mode <en-us_topic_0000001813438796__en-us_topic_0000001432527901_li9493135323214>` is **Teradata**.
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

**Output:**

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
