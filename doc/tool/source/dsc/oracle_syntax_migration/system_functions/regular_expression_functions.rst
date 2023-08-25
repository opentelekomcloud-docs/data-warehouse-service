:original_name: dws_mt_0139.html

.. _dws_mt_0139:

Regular Expression Functions
============================

Regular expressions specify patterns to match strings using standardized syntax conventions. In Oracle, regular expressions are implemented using a set of SQL functions that allow you to search and use string data.

DSC can migrate :ref:`REGEXP_INSTR <en-us_topic_0000001233800665__en-us_topic_0238518400_en-us_topic_0237362432_en-us_topic_0202727114_section367618918273>`, :ref:`REGEXP_SUBSTR <en-us_topic_0000001233800665__en-us_topic_0238518400_en-us_topic_0237362432_en-us_topic_0202727114_section9584371501>`, and :ref:`REGEXP_REPLACE <en-us_topic_0000001233800665__en-us_topic_0238518400_en-us_topic_0237362432_en-us_topic_0202727114_section0549591474>` regular expressions. Details are as follows:

-  Regexp (REGEXP_INSTR and REGEXP_SUBSTR) that includes the **sub_expr** parameter are not supported. If the input script includes **sub_expr**, the DSC will log an error for it.
-  Regexp (REGEXP_INSTR, REGEXP_SUBSTR, and REGXP_REPLACE) uses the **match_param** parameter to set the default matching behavior. The DSC supports values i (case-insensitive) and c (case-sensitive) for this parameter. Other values for **match_param** are not supported.
-  Regexp (REGEXP_INSTR) uses the **return_option** parameter to set what is returned for regexp. The DSC supports the value 0 (zero) for this parameter. Other values for return_option are not supported.

.. _en-us_topic_0000001233800665__en-us_topic_0238518400_en-us_topic_0237362432_en-us_topic_0202727114_section367618918273:

REGEXP_INSTR
------------

REGEXP_INSTR extends the functionality of the INSTR function by supporting the regular expression pattern for the search string. REGEXP_INSTR with 2 to 6 parameters are supported for migration.

The **sub_expr** parameter (parameter #7) is available in Oracle but is not supported for migration. If the input script includes **sub_expr**, the DSC will log an error for it.

For **return_option**, the value 0 (zero) is supported. Other values for return_option are not supported.

For **match_param**, values i (case-insensitive) and c (case-sensitive) are supported. Other values for **match_param** are not supported.

.. code-block::

   REGEXP_INSTR(
        string,
         pattern,
         [start_position,]
         [nth_appearance,]
         [return_option,]
         [match_param,]
         [sub_expr]
   )

**Bulk Operations**

-  **Input - REGEXP_INSTR**

.. code-block::

   SELECT
             REGEXP_INSTR( 'TechOnTheNet is a great resource' ,'t' )
        FROM
             dual
   ;

**Output**

.. code-block::

   SELECT
             MIG_ORA_EXT.REGEXP_INSTR (
                  'TechOnTheNet is a great resource'
                  ,'t'
             )
        FROM
             dual
   ;

-  **Input - REGEXP_INSTR with 7 parameters** **(Invalid)**

.. code-block::

   SELECT
             Empno
             ,ename
             ,REGEXP_INSTR( ename ,'a|e|i|o|u' ,1 ,1 ,0 ,'i' ,7 ) AS Dname
        FROM
             emp19
   ;

**Output**

The input expression has 7 parameters. Since the tool supports REGEXP_INSTR with 2 to 6 arguments, an error will be logged, starting "*Seven(7) arguments for REGEXP_INSTR function is not supported.*"

.. code-block::

   SELECT
             Empno
             ,ename
             ,REGEXP_INSTR( ename ,'a|e|i|o|u' ,1 ,1 ,0 ,'i' ,7 ) AS Dname
        FROM
             emp19
   ;

**BLogic Operations**

-  **Input - REGEXP_INSTR**

.. code-block::

   CREATE OR REPLACE FUNCTION myfct
   RETURN VARCHAR2
   IS
   res VARCHAR2(200) ;
   BEGIN
       res := 100 ;
       INSERT INTO emp19 RW(RW.empno,RW.ename,dname) SELECT res, RWN.ename key
   , regexp_instr(ename ,'[ae]',4,2,0, 'i')   as Dname FROM   emp19 RWN ;

       RETURN res ;
   END ;
   /

**Output**

.. code-block::

   CREATE
        OR REPLACE FUNCTION myfct RETURN VARCHAR2 IS res VARCHAR2 ( 200 ) ;
        BEGIN
             res := 100 ;
             INSERT INTO emp19 ( empno ,ename ,dname ) SELECT
                  res ,RWN.ename "KEY" ,MIG_ORA_EXT.REGEXP_INSTR ( ename ,'[ae]' ,4 ,2 ,0 ,'i' ) as Dname
             FROM
                  emp19 RWN ;
                  RETURN res ; END ;
   /

.. _en-us_topic_0000001233800665__en-us_topic_0238518400_en-us_topic_0237362432_en-us_topic_0202727114_section9584371501:

REGEXP_SUBSTR
-------------

REGEXP_SUBSTR extends the functionality of the SUBSTR function by supporting regular expression pattern for the search string. REGEXP_SUBSTR with 2 to 5 parameters are supported for migration.

The **sub_expr** parameter (parameter #6) is available in Oracle but is not supported for migration. If the input script includes **sub_expr**, the DSC will log an error for it.

For **match_param**, values i (case-insensitive) and c (case-sensitive) are supported. Other values for **match_param** are not supported.

.. code-block::

   REGEXP_SUBSTR(
       string,
         pattern,
         [start_position,]
         [nth_appearance,]
         [match_param,]
         [sub_expr]
    )

**Bulk Operations**

-  **Input - REGEXP_SUBSTR**

.. code-block::

   SELECT
             Ename
             ,REGEXP_SUBSTR( 'Programming' ,'(\w).*?\1' ,1 ,1 ,'i' )
        FROM
             emp16
   ;

**Output**

.. code-block::

   SELECT
             Ename
             ,MIG_ORA_EXT.REGEXP_SUBSTR (
                  'Programming'
                  ,'(\w).*?\1'
                  ,1
                  ,1
                  ,'i'
             )
        FROM
             emp16
   ;

-  **Input - REGEXP_SUBSTR**

.. code-block::

   SELECT
             REGEXP_SUBSTR( '1234567890' ,'(123)(4(56)(78))' ,1 ,1 ,'i'  ) "REGEXP_SUBSTR"
        FROM
             DUAL
   ;

**Output**

.. code-block::

   SELECT
             MIG_ORA_EXT.REGEXP_SUBSTR (
                  '1234567890'
                  ,'(123)(4(56)(78))'
                  ,1
                  ,1
                  ,'i'
             ) "REGEXP_SUBSTR"
        FROM
             DUAL
   ;

-  **Input - REGEXP_SUBSTR with 6 parameters** **(Invalid)**

.. code-block::

   SELECT
             REGEXP_SUBSTR( '1234567890' ,'(123)(4(56)(78))' ,1 ,1 ,'i' ,1 ) "REGEXP_SUBSTR"
        FROM
             DUAL
   ;

**Output**

The input expression has 6 arguments. Since the tool supports REGEXP_SUBSTR with 2 to 5 parameters an error will be logged, starting "*Error message :Six(6) arguments for REGEXP_SUBSTR function is not supported.*"

.. code-block::

   SELECT
             REGEXP_SUBSTR( '1234567890' ,'(123)(4(56)(78))' ,1 ,1 ,'i' ,1 ) "REGEXP_SUBSTR"
        FROM
             DUAL
   ;

**BLogic Operations**

-  **Input - REGEXP_SUBSTR**

.. code-block::

   CREATE OR REPLACE FUNCTION myfct
   RETURN VARCHAR2
   IS
   res VARCHAR2(200) ;
   BEGIN
       res := 100 ;
       INSERT INTO emp19 RW(RW.empno,RW.ename,dname) SELECT res, RWN.ename key
   ,REGEXP_ SUBSTR ('TechOnTheNet', 'a|e|i|o|u', 1, 1, 'i') as Dname FROM   emp19 RWN ;

       RETURN res ;
   END ;
   /

**Output**

.. code-block::

   CREATE
        OR REPLACE FUNCTION myfct RETURN VARCHAR2 IS res VARCHAR2 ( 200 ) ;
        BEGIN
             res := 100 ;
             INSERT INTO emp19 ( empno ,ename ,dname ) SELECT
                  res ,RWN.ename "KEY" ,MIG_ORA_EXT.REGEXP_ SUBSTR ( 'TechOnTheNet' ,'a|e|i|o|u' ,1 ,1 ,'i' ) as Dname
             FROM
                  emp19 RWN ;
                  RETURN res ;
   END ;
   /

.. _en-us_topic_0000001233800665__en-us_topic_0238518400_en-us_topic_0237362432_en-us_topic_0202727114_section0549591474:

REGEXP_REPLACE
--------------

REGEXP_REPLACE extends the functionality of the REPLACE function by supporting regular expression pattern for the search string. REGEXP_REPLACE with 2 to 6 parameters are supported for migration.

For **match_param**, values i (case-insensitive) and c (case-sensitive) are supported. Other values for **match_param** are not supported.

.. code-block::

   REGEXP_REPLACE(
         string,
         pattern,
         [replacement_string,]
         [start_position,]
         [nth_appearance,]
         [match_param]
    )

**Bulk Operations**

-  **Input - REGEXP_REPLACE**

.. code-block::

   SELECT
             testcol
             ,regexp_replace( testcol ,'([[:digit:]]{3})\.([[:digit:]]{3})\.([[:digit:]]{4})' ,'(\1) \2-\3' ) RESULT
        FROM
             test
        WHERE
             LENGTH( testcol ) = 12
   ;

**Output**

.. code-block::

   SELECT
             testcol
             ,MIG_ORA_EXT.REGEXP_REPLACE (
                  testcol
                  ,'([[:digit:]]{3})\.([[:digit:]]{3})\. ([[:digit:]]{4})'
                  ,'(\1) \2-\3'
             ) RESULT
        FROM
             test
        WHERE
             LENGTH( testcol ) = 12
   ;

-  **Input - REGEXP_REPLACE**

.. code-block::

   SELECT
             UPPER( regexp_replace ( 'foobarbequebazilbarfbonk barbeque' ,'(b[^b]+)(b[^b]+)' ) )
        FROM
             DUAL
   ;

**Output**

.. code-block::

   SELECT
             UPPER( MIG_ORA_EXT.REGEXP_REPLACE ( 'foobarbequebazilbarfbonk barbeque' ,'(b[^b]+)(b[^b]+)' ) )
        FROM
             DUAL
   ;

-  **Input - REGEXP_REPLACE with 7 parameters** **(Invalid)**

.. code-block::

   SELECT
             REGEXP_REPLACE( 'TechOnTheNet' ,'a|e|i|o|u' ,'Z' ,1 ,1 ,'i' ,'(\1) \2-\3' ) AS First_Occurrence
        FROM
             emp
   ;

**Output**

The input expression has 7 parameters. Since the tool supports REGEXP_REPLACE with 2 to 6 parameters, an error will be logged, starting "*Too many arguments for REGEXP_REPLACE function [Max:6 argument(s) is/are allowed].*"

.. code-block::

   SELECT
             REGEXP_REPLACE( 'TechOnTheNet' ,'a|e|i|o|u' ,'Z' ,1 ,1 ,'i' ,'(\1) \2-\3' ) AS First_Occurrence
        FROM
             emp
   ;

**BLogic Operations**

-  **Input - REGEXP_REPLACE**

.. code-block::

   CREATE OR REPLACE FUNCTION myfct
   RETURN VARCHAR2
   IS
   res VARCHAR2(200) ;
   BEGIN
       res := 100 ;
       INSERT INTO emp19 RW(RW.empno,RW.ename,dname) SELECT res, RWN.ename key
   ,REGEXP_REPLACE ('TechOnTheNet', 'a|e|i|o|u', 'Z', 1, 1, 'i') as Dname FROM   emp19 RWN ;

       RETURN res ;
   END ;
   /

**Output**

.. code-block::

   CREATE
        OR REPLACE FUNCTION myfct RETURN VARCHAR2 IS res VARCHAR2 ( 200 ) ;
        BEGIN
             res := 100 ;
             INSERT INTO emp19 ( empno ,ename ,dname ) SELECT
                  res ,RWN.ename "KEY" ,MIG_ORA_EXT.REGEXP_REPLACE ( 'TechOnTheNet' ,'a|e|i|o|u' ,'Z' ,1 ,1 ,'i' ) as Dname
             FROM
                  emp19 RWN ;
                  RETURN res ;
   END ;
   /

LISTAGG/regexp_replace/regexp_instr
-----------------------------------

Configure the following parameters before migrating LISTAGG/regexp_replace/regexp_instr:

-  MigSupportForListAgg=false
-  MigSupportForRegexReplace=false

**Input- REMOVE LISTAGG/regexp_replace/regexp_instr**

.. code-block::

   SELECT LISTAGG(T.OS_SOFTASSETS_ID,',') WITHIN GROUP(ORDER BY T.SOFTASSETS_ID)
          INTO V_OS_SOFTASSETS_IDS
          FROM SPMS_SYSSOFT_PROP_APPR T
         WHERE T.APPR_ID = I_APPR_ID
           AND T.SYSSOFT_PROP = '001';

   V_ONLY_FILE_NAME := REGEXP_REPLACE( I_FILENAME ,'.*/' ,'' ) ;

   THEN v_auth_type := 102;
            ELSIF v_status IN ('0100', '0200')
                  AND REGEXP_INSTR (v_role_str, ',(411|414),') > 0

**Output**

.. code-block::

   "SELECT LISTAGG(T.OS_SOFTASSETS_ID,',') WITHIN GROUP(ORDER BY T.SOFTASSETS_ID)
          INTO V_OS_SOFTASSETS_IDS
          FROM SPMS_SYSSOFT_PROP_APPR T
         WHERE T.APPR_ID = I_APPR_ID
           AND T.SYSSOFT_PROP = '001';

   V_ONLY_FILE_NAME := REGEXP_REPLACE (I_FILENAME, '.*/', '');

   THEN  v_auth_type := 102;
            ELSIF v_status IN ('0100', '0200')
                      AND REGEXP_INSTR (v_role_str, ',(411|414),') > 0"
