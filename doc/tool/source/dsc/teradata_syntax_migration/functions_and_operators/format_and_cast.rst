:original_name: dws_16_0053.html

.. _dws_16_0053:

.. _en-us_topic_0000001860318893:

FORMAT and CAST
===============

In Teradata, the **FORMAT** keyword is used for formatting a column/expression. For example, FORMAT '9(n)' and 'z(n)' are addressed using LPAD with 0 and space (' ') respectively.

Data typing is done using **CAST** or direct data type [like (expression1)(CHAR(n))]. This feature is addressed using **CAST**. For details, see :ref:`Type Casting and Formatting <en-us_topic_0000001860318709>`.

**Input: FORMAT with CAST**

::

   SELECT
             CAST(TRIM( Agt_Num ) AS DECIMAL( 5 ,0 ) FORMAT '9(5)' )
        FROM
             C03_AGENT_BOND
   ;

   SELECT
             CAST(CAST( Agt_Num AS INT FORMAT 'Z(17)' ) AS CHAR( 5 ) )
        FROM
             C03_AGENT_BOND
   ;

   SELECT
             CHAR(CAST( CAST( CND_VLU AS DECIMAL( 17 ,0 ) FORMAT 'Z(17)' ) AS VARCHAR( 17 ) ) )
        FROM
             C03_AGENT_BOND
   ;

**Output:**

.. code-block::

   SELECT
             LPAD( CAST( TRIM( Agt_Num ) AS DECIMAL( 5 ,0 ) ) ,5 ,'0' ) AS Agt_Num
        FROM
             C03_AGENT_BOND
   ;
   SELECT
   CAST(CAST( Agt_Num AS INT FORMAT 'Z(17)' ) AS CHAR( 5 ) )
   FROM
   C03_AGENT_BOND
   ;

   SELECT
             LENGTH( CAST( LPAD( CAST( CND_VLU AS DECIMAL( 17 ,0 ) ) ,17 ,' ' ) AS VARCHAR( 17 ) ) ) AS CND_VLU
        FROM
             C03_AGENT_BOND
   ;

**Input - FORMAT 'Z(n)9'**

::

   SELECT
             standard_price (FORMAT 'Z(5)9') (CHAR( 6 ))
             ,max_price (FORMAT 'ZZZZZ9') (CHAR( 6 ))
        FROM
             product_t
   ;

**Output:**

::

   SELECT
             CAST( TO_CHAR( standard_price ,'999990' ) AS CHAR( 6 ) ) AS standard_price
             ,CAST( TO_CHAR( max_price ,'999990' ) AS CHAR( 6 ) ) AS max_price
        FROM
             product_t
   ;

**Input - FORMAT 'z(m)9.9(n)'**

::

   SELECT
             standard_price (FORMAT 'Z(6)9.9(2)') (CHAR( 6 ))
        FROM
             product_t
   ;

**Output:**

::

   SELECT
             CAST( TO_CHAR( standard_price ,'9999990.00' ) AS CHAR( 6 ) ) AS standard_price
        FROM
             product_t
   ;

**Input - CAST AS INTEGER**

::

   SELECT
             CAST( standard_price AS INTEGER )
        FROM
             product_t
   ;

**Output:**

::

   SELECT
            (standard_price)
        FROM
             product_t
   ;

**Input - CAST AS INTEGER FORMAT**

::

   SELECT
             CAST( price11 AS INTEGER FORMAT 'Z(4)9' ) (
                  CHAR( 10 )
             )
        FROM
             product_t
   ;

**Output:**

::

   SELECT
             CAST( TO_CHAR(  ( price11 ) ,'99990' ) AS CHAR( 10 ) ) AS price11
        FROM
             product_t
   ;

.. note::

   The following GaussDB(DWS) function is added to convert to integer:

   ::

      CREATE OR REPLACE FUNCTION
      /*  This function is used to support "CAST AS INTEGER" of Teradata.
          It should be created in the "mig_td_ext" schema.
      */
           ( i_param                            TEXT )
      RETURN INTEGER
      AS
        v_castasint    INTEGER;
      BEGIN

         v_castasint := CASE WHEN i_param IS NULL
                                                      THEN NULL         -- if NULL value is provided as input
                                                                      WHEN TRIM(i_param) IS NULL
                                                                                      THEN 0                  -- if empty string with one or more spaces is provided
                                                                      ELSE TRUNC(CAST(i_param AS NUMBER))            -- if any numeric value is provided
                                      END;

      RETURN v_castasint;
      END;
