:original_name: dws_mt_0103.html

.. _dws_mt_0103:

Type Casting and Formatting
===========================

This section contains the migration syntax for migrating Teradata type casting and formatting syntax. The migration syntax determines how the keywords and features are migrated.

In Teradata, the FORMAT keyword is used for formatting a column/expression. FORMAT '9(n)' and 'z(n)' are addressed using LPAD with 0 and space (' ') respectively. Data typing can be done using CAST or direct data type [like (expression1)(CHAR(n))]. This feature is addressed using CAST.

The following type casting and formatting statements are supported by the DSC:

-  :ref:`CHAR <en-us_topic_0000001234042135__en-us_topic_0238518370_en-us_topic_0237362170_section2011916307167>`
-  :ref:`COLUMNS and COLUMN ALIAS <en-us_topic_0000001234042135__en-us_topic_0238518370_en-us_topic_0237362170_section5692094135>`
-  :ref:`Expressions <en-us_topic_0000001234042135__en-us_topic_0238518370_en-us_topic_0237362170_section126680186810>`
-  :ref:`INT <en-us_topic_0000001234042135__en-us_topic_0238518370_en-us_topic_0237362170_section315523012168>`
-  :ref:`DATE <en-us_topic_0000001234042135__en-us_topic_0238518370_en-us_topic_0237362170_section1042495513341>`
-  :ref:`DAY to SECOND <en-us_topic_0000001234042135__en-us_topic_0238518370_en-us_topic_0237362170_section162931946131113>`
-  :ref:`DECIMAL <en-us_topic_0000001234042135__en-us_topic_0238518370_en-us_topic_0237362170_section198848243414>`
-  :ref:`Time Interval <en-us_topic_0000001234042135__en-us_topic_0238518370_en-us_topic_0237362170_section32801030171617>`
-  :ref:`NULL <en-us_topic_0000001234042135__en-us_topic_0238518370_en-us_topic_0237362170_section1576861217140>`
-  :ref:`Implicit Type Casting Issues <en-us_topic_0000001234042135__en-us_topic_0238518370_en-us_topic_0237362170_section11306133051616>`

.. _en-us_topic_0000001234042135__en-us_topic_0238518370_en-us_topic_0237362170_section2011916307167:

CHAR
----

**Input** **- Data type casting for CHAR**

::

   (expression1)(CHAR(n))

**Output**

::

   CAST( (expression1) AS CHAR(n) )

.. _en-us_topic_0000001234042135__en-us_topic_0238518370_en-us_topic_0237362170_section5692094135:

COLUMNS and COLUMN ALIAS
------------------------

**Input** **- Type casting and formatting of a column should ensure the column name is the same as the column alias**

::

   SELECT Product_Line_ID, MAX(Standard_Price)
     FROM ( SELECT A.Product_Description, A.Product_Line_ID
      , A.Standard_Price(DECIMAL(18),FORMAT '9(18)')(CHAR(18))
           FROM product_t A
         WHERE Product_Line_ID in (1, 2)
     ) AS tabAls
    GROUP BY Product_Line_ID;

**Output**

::

   SELECT Product_Line_ID, MAX( Standard_Price )
      FROM  ( SELECT A.Product_Description, A.Product_Line_ID
           , CAST( LPAD( CAST(A.Standard_Price AS DECIMAL( 18 ,0 )), 18, '0' ) AS CHAR( 18 ) ) AS Standard_Price
                    FROM product_t A
                  WHERE Product_Line_ID IN( 1 ,2 )
              ) AS tabAls
     GROUP BY Product_Line_ID;

.. _en-us_topic_0000001234042135__en-us_topic_0238518370_en-us_topic_0237362170_section126680186810:

Expressions
-----------

**Input** **- Type casting and formatting of an expression**

::

   SELECT product_id, standard_price*100.00(DECIMAL (17),FORMAT '9(17)' )(CHAR(17) ) AS order_amt
      FROM db_pvfc9_std.Product_t
     WHERE product_line_id is not null ;

**Output**

::

   SELECT product_id, CAST(LPAD(CAST(standard_price*100.00 AS DECIMAL(17)), 17, '0') AS CHAR(17)) AS order_amt
      FROM db_pvfc9_std.Product_t
      WHERE product_line_id is not null ;

.. _en-us_topic_0000001234042135__en-us_topic_0238518370_en-us_topic_0237362170_section315523012168:

INT
---

**Input** **- Data type casting for INT**

::

   SELECT
             CAST( col1 AS INT ) (
                  FORMAT '9(5)'
             )
        FROM
             table1
   ;

**Output**

::

   SELECT
             LPAD( CAST( col1 AS INT ) ,5 ,'0' )
        FROM
             table1
   ;

**Input** - **Data type casting for INT**

::

   SELECT
             CAST( col1 AS INT ) (
                  FORMAT '999999'
             )
        FROM
             table1
   ;

**Output**

::

   SELECT
             LPAD( CAST( col1 AS INT ) ,6 ,'0' )
        FROM
             table1
   ;

**Input** - **Data type casting for INT**

::

   SELECT
             CAST( expression1 AS INT FORMAT '9(10)' )
        FROM
             table1
   ;

**Output**

::

   SELECT
             LPAD( CAST( expression1 AS INT ) ,10 ,'0' )
        FROM
             table1
   ;

**Input** - **Data type casting for INT**

::

   SELECT
             CAST( expression1 AS INT FORMAT '9999' )
        FROM
             table1
   ;

**Output**

::

   SELECT
             LPAD( CAST( expression1 AS INT ) ,4 ,'0' )
        FROM
             table1
   ;

.. _en-us_topic_0000001234042135__en-us_topic_0238518370_en-us_topic_0237362170_section1042495513341:

DATE
----

In Teradata, when casting DATE from one format to another format, AS FORMAT is used. Migration tools will add TO_CHAR function to retain the specified input format.

For details, see :ref:`Date and Time Functions <dws_mt_0102>`.

**Input** - Data type casting without DATE keyword

::

   SELECT
         CAST( CAST( '2013-02-12' AS DATE FORMAT 'YYYY/MM/DD' ) AS FORMAT 'DD/MM/YY' )
   ;

**Output**

::

   SELECT
         TO_CHAR( CAST( '2013-02-12' AS DATE ) ,'DD/MM/YY' )
   ;

.. _en-us_topic_0000001234042135__en-us_topic_0238518370_en-us_topic_0237362170_section162931946131113:

DAY to SECOND
-------------

**Input** - Data type casting DAY to SECOND

::

   SELECT CAST(T1.Draw_Gold_Dt || ' ' ||T1.Draw_Gold_Tm as Timestamp)
   - CAST(T1.Tx_Dt || ' '|| T1.Tx_Tm as Timestamp)  DAY(4) To SECOND  from db_pvfc9_std.draw_tab T1;

**Output**

::

   SELECT
             CAST(( CAST( T1.Draw_Gold_Dt || ' ' || T1.Draw_Gold_Tm AS TIMESTAMP ) - CAST(T1.Tx_Dt || ' ' || T1.Tx_Tm AS TIMESTAMP ) ) AS INTERVAL DAY ( 4 ) TO SECOND )
        FROM
             db_pvfc9_std.draw_tab T1
   ;

.. _en-us_topic_0000001234042135__en-us_topic_0238518370_en-us_topic_0237362170_section198848243414:

DECIMAL
-------

**Input** - Data type casting for DECIMAL

::

   SELECT
             standard_price (
                  DECIMAL( 17 )
                  ,FORMAT '9(17)'
             ) (
                  CHAR( 17 )
             )
        FROM
             db_pvfc9_std.Product_t
   ;

**Output**

::

   SELECT
             CAST( LPAD( CAST( standard_price AS DECIMAL( 17 ,0 ) ) ,17 ,'0' ) AS CHAR( 17 ) )
        FROM
             db_pvfc9_std.Product_t
   ;

**Input** - Data type casting for DECIMAL

::

   SELECT
             standard_price (
                  DECIMAL( 17 ,0 )
                  ,FORMAT '9(17)'
             ) (
                  VARCHAR( 17 )
             )
        FROM
             db_pvfc9_std.Product_t
   ;

**Output**

::

   SELECT
             CAST( LPAD( CAST( standard_price AS DECIMAL( 17 ,0 ) ) ,17 ,'0' ) AS VARCHAR( 17 ) )
        FROM
             db_pvfc9_std.Product_t
   ;

**Input** - Data type casting for DECIMAL

::

   SELECT
             customer_id (
                  DECIMAL( 17 )
             ) (
                  FORMAT '9(17)'
             ) (
                  VARCHAR( 17 )
             )
        FROM
             db_pvfc9_std.Customer_t
   ;

**Output**

::

   SELECT
             CAST( LPAD( CAST( customer_id AS DECIMAL( 17 ,0 ) ) ,17 ,'0' ) AS VARCHAR( 17 ) )
        FROM
             db_pvfc9_std.Customer_t
   ;

.. _en-us_topic_0000001234042135__en-us_topic_0238518370_en-us_topic_0237362170_section32801030171617:

Time Interval
-------------

Type casting to time intervals is supported in DDL and DML. It is supported within SELECT and can be used in subqueries of VIEW, MERGE, and INSERT.

**Input** - Data type casting to time intervals

::

   SELECT TIME '06:00:00.00' HOUR TO SECOND;

**Output**

::

   SELECT TIME '06:00:00.00';

**Input** - Data type casting to time intervals with TOP

::

   SELECT TOP 3 * FROM dwQErrDtl_mc.C03_CORP_AGENT_INSURE
   WHERE Data_Dt > (SELECT TIME '06:00:00.00' HOUR TO SECOND);

**Output**

::

   SELECT  * FROM dwQErrDtl_mc.C03_CORP_AGENT_INSURE WHERE  Data_Dt > (SELECT TIME '06:00:00.00')   limit 3;

.. _en-us_topic_0000001234042135__en-us_topic_0238518370_en-us_topic_0237362170_section1576861217140:

NULL
----

DSC will migrate an expression in the form NULL(data_type) to CAST(NULL AS replacement_data_type).

**Input** - Data type casting for NULL

::

   NULL(VARCHAR(n))

**Output**

::

   CAST(NULL AS VARCHAR(n))

.. _en-us_topic_0000001234042135__en-us_topic_0238518370_en-us_topic_0237362170_section11306133051616:

Implicit Type Casting Issues
----------------------------

**Input** **- Implicit TYPE CASTING ISSUES**

::

   SELECT Data_Type,Start_Dt,End_Dt
    FROM (
     SELECT Data_Type,Start_Dt,End_Dt
     FROM (
      SELECT '101' AS Data_Type,CAST('${TX_DATE}' AS DATE FORMAT 'YYYYMMDD')-1 AS Start_Dt,CAST('${TX_DATE}' AS DATE FORMAT 'YYYYMMDD') AS End_Dt
     ) TT
     UNION ALL
     SELECT '201' AS Data_Type,CAST('${TX_DATE}' AS DATE FORMAT 'YYYYMMDD')-7 AS Start_Dt,CAST('${TX_DATE}' AS DATE FORMAT 'YYYYMMDD') AS End_Dt
     FROM Sys_Calendar.CALENDAR
     WHERE calendar_date = CAST('${TX_DATE}' AS DATE FORMAT 'YYYYMMDD')
     AND Day_Of_Week = 1
     UNION ALL
     SELECT Data_Type,Start_Dt,End_Dt
     FROM (
      SELECT '401' AS Data_Type,CAST('${TX_PRIMONTH_END}' AS DATE FORMAT 'YYYYMMDD') AS Start_Dt,CAST('${TX_DATE}' AS DATE FORMAT 'YYYYMMDD') AS End_Dt
      ) TT
     WHERE CAST('${TX_DATE}' AS DATE FORMAT 'YYYYMMDD')=CAST('${TX_MONTH_END}' AS DATE FORMAT 'YYYYMMDD')
     UNION ALL
     SELECT Data_Type,Start_Dt,End_Dt
     FROM (
      SELECT '501' AS Data_Type,CAST('${TX_PRIQUARTER_END}' AS DATE FORMAT 'YYYYMMDD') AS Start_Dt,CAST('${TX_DATE}' AS DATE FORMAT 'YYYYMMDD') AS End_Dt
      ) TT
     WHERE CAST('${TX_DATE}' AS DATE FORMAT 'YYYYMMDD')=CAST('${TX_QUARTER_END}' AS DATE FORMAT 'YYYYMMDD')
     UNION ALL
     SELECT Data_Type,Start_Dt,End_Dt
     FROM (
      SELECT '701' AS Data_Type,CAST('${TX_PRIYEAR_END}' AS DATE FORMAT 'YYYYMMDD') AS Start_Dt,CAST('${TX_DATE}' AS DATE FORMAT 'YYYYMMDD') AS End_Dt
      ) TT
     WHERE CAST('${TX_DATE}' AS DATE FORMAT 'YYYYMMDD')=CAST('${TX_YEAR_END}' AS DATE FORMAT 'YYYYMMDD')
    ) T1
    ;

**Output**

::

   SELECT Data_Type,Start_Dt,End_Dt
    FROM (
     SELECT Data_Type,Start_Dt,End_Dt
     FROM (
      SELECT CAST('101' AS TEXT) AS Data_Type,CAST('${TX_DATE}' AS DATE FORMAT 'YYYYMMDD')-1 AS Start_Dt,CAST('${TX_DATE}' AS DATE FORMAT 'YYYYMMDD') AS End_Dt
     ) TT
     UNION ALL
     SELECT CAST('201' AS TEXT) AS Data_Type,CAST('${TX_DATE}' AS DATE FORMAT 'YYYYMMDD')-7 AS Start_Dt,CAST('${TX_DATE}' AS DATE FORMAT 'YYYYMMDD') AS End_Dt
     FROM Sys_Calendar.CALENDAR
     WHERE calendar_date = CAST('${TX_DATE}' AS DATE FORMAT 'YYYYMMDD')
     AND Day_Of_Week = 1
     UNION ALL
     SELECT Data_Type,Start_Dt,End_Dt
     FROM (
      SELECT CAST('401' AS TEXT) AS Data_Type,CAST('${TX_PRIMONTH_END}' AS DATE FORMAT 'YYYYMMDD') AS Start_Dt,CAST('${TX_DATE}' AS DATE FORMAT 'YYYYMMDD') AS End_Dt
      ) TT
     WHERE CAST('${TX_DATE}' AS DATE FORMAT 'YYYYMMDD')=CAST('${TX_MONTH_END}' AS DATE FORMAT 'YYYYMMDD')
     UNION ALL
     SELECT Data_Type,Start_Dt,End_Dt
     FROM (
      SELECT CAST('501' AS TEXT) AS Data_Type,CAST('${TX_PRIQUARTER_END}' AS DATE FORMAT 'YYYYMMDD') AS Start_Dt,CAST('${TX_DATE}' AS DATE FORMAT 'YYYYMMDD') AS End_Dt
      ) TT
     WHERE CAST('${TX_DATE}' AS DATE FORMAT 'YYYYMMDD')=CAST('${TX_QUARTER_END}' AS DATE FORMAT 'YYYYMMDD')
     UNION ALL
     SELECT Data_Type,Start_Dt,End_Dt
     FROM (
      SELECT CAST('701' AS TEXT) AS Data_Type,CAST('${TX_PRIYEAR_END}' AS DATE FORMAT 'YYYYMMDD') AS Start_Dt,CAST('${TX_DATE}' AS DATE FORMAT 'YYYYMMDD') AS End_Dt
      ) TT
     WHERE CAST('${TX_DATE}' AS DATE FORMAT 'YYYYMMDD')=CAST('${TX_YEAR_END}' AS DATE FORMAT 'YYYYMMDD')
    ) T1
    ;
