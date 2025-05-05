:original_name: dws_mt_0159.html

.. _dws_mt_0159:

Package Variables
=================

Package variables are available in Oracle packages that allow variables to retain all the functions and procedures in the package. DSC uses customized functions to help GaussDB(DWS) support package variables.

.. note::

   Prerequisites

   -  Create and use the **MIG_ORA_EXT** schema.
   -  Copy the contents of the custom script and execute the script in all target databases for which migration is to be performed. For details, see :ref:`Migration Process <dws_16_0017>`.

   If there is a space between a schema name and a package name, or either the package specification or body has quotes, the output may not be the same as expected.

**Input** - **Package variables**

::

   CREATE
        OR REPLACE PACKAGE scott.pkg_adm_util IS un_stand_value long := '`' ;
        defaultdate date := sysdate ;
   g_pkgname CONSTANT VARCHAR2 ( 255 ) DEFAULT 'pkg_adm_util' ;
   procedure p1 ;
   END pkg_adm_util ;
   /

   CREATE
        OR REPLACE PACKAGE BODY scott.pkg_adm_util AS defaulttime timestamp := systimestamp ;
        PROCEDURE P1 AS BEGIN
             scott.pkg_adm_util.un_stand_value := 'A' ;
             pkg_adm_util.un_stand_value := 'B' ;
        un_stand_value := 'C' ;
   DBMS_OUTPUT.PUT_LINE ( pkg_adm_util.defaultdate ) ;
   DBMS_OUTPUT.PUT_LINE ( defaulttime ) ;
   DBMS_OUTPUT.PUT_LINE ( scott.pkg_adm_util.un_stand_value ) ;
   DBMS_OUTPUT.PUT_LINE ( pkg_adm_util.un_stand_value ) ;
   DBMS_OUTPUT.PUT_LINE ( un_stand_value ) ;
   END ;
   END ;
   /

**Output**

::

   SCHEMA pkg_adm_util
   ;
   BEGIN
   INSERT INTO MIG_ORA_EXT.MIG_PKG_VARIABLES
   ( SCHEMA_NAME ,PACKAGE_NAME ,SPEC_OR_BODY ,VARIABLE_NAME
   ,VARIABLE_TYPE ,CONSTANT_
   I ,DEFAULT_VALUE ,EXPRESSION_I )
   VALUES
   ( UPPER( 'scott' ) ,UPPER( 'pkg_adm_util' ) ,'S' ,UPPER(
   'un_stand_value' ) ,UPPE
   R( 'TEXT' ) ,false ,'`' ,false ) ;
   INSERT INTO MIG_ORA_EXT.MIG_PKG_VARIABLES
   ( SCHEMA_NAME ,PACKAGE_NAME ,SPEC_OR_BODY ,VARIABLE_NAME
   ,VARIABLE_TYPE ,CONSTANT_
   I ,DEFAULT_VALUE ,EXPRESSION_I )
   VALUES
   ( UPPER( 'scott' ) ,UPPER( 'pkg_adm_util' ) ,'S' ,UPPER(
   'defaultdate' ) ,UPPER( '
   date' ) ,false ,$q$sysdate$q$ ,true ) ;
   INSERT INTO MIG_ORA_EXT.MIG_PKG_VARIABLES
   ( SCHEMA_NAME ,PACKAGE_NAME ,SPEC_OR_BODY ,VARIABLE_NAME
   ,VARIABLE_TYPE ,CONSTANT_
   I ,DEFAULT_VALUE ,EXPRESSION_I )
   VALUES
   ( UPPER( 'scott' ) ,UPPER( 'pkg_adm_util' ) ,'S' ,UPPER(
   'g_pkgname' ) ,UPPER( 'VA
   RCHAR2 ( 255 )' ) ,true ,'pkg_adm_util' ,false ) ;
   END ;
   /
   BEGIN
   INSERT INTO MIG_ORA_EXT.MIG_PKG_VARIABLES
   ( SCHEMA_NAME ,PACKAGE_NAME ,SPEC_OR_BODY ,VARIABLE_NAME
   ,VARIABLE_TYPE ,CONSTANT_
   I ,DEFAULT_VALUE ,EXPRESSION_I )
   VALUES
   ( UPPER( 'scott' ) ,UPPER( 'pkg_adm_util' ) ,'B' ,UPPER(
   'defaulttime' ) ,UPPER( '
   timestamp' ) ,false ,$q$CURRENT_TIMESTAMP$q$ ,true ) ;
   END ;
   /
   CREATE
   OR REPLACE PROCEDURE pkg_adm_util.P1 AS
   BEGIN
   MIG_ORA_EXT.MIG_FN_SET_PKG_VARIABLE ( current_schema ( )
   ,'pkg_adm_util' ,'un_stand_value' ,( 'A' ) ::TEXT ) ;
   MIG_ORA_EXT.MIG_FN_SET_PKG_VARIABLE ( current_schema ( )
   ,'pkg_adm_util' ,'un_stand_value' ,( 'B' ) ::TEXT ) ;
   MIG_ORA_EXT.MIG_FN_SET_PKG_VARIABLE ( current_schema ( )
   ,'pkg_adm_util' ,'un_stand_value' ,( 'C' ) ::TEXT ) ;

   DBMS_OUTPUT.PUT_LINE ( MIG_ORA_EXT.MIG_FN_GET_PKG_VARIABLE
   ( 'scott' ,'pkg_adm_util' ,'defaultdate' ) :: date ) ;
   DBMS_OUTPUT.PUT_LINE ( MIG_ORA_EXT.MIG_FN_GET_PKG_VARIABLE(
   'scott' ,'pkg_adm_util' ,'defaulttime' ) :: timestamp ) ;
   DBMS_OUTPUT.PUT_LINE ( MIG_ORA_EXT.MIG_FN_GET_PKG_VARIABLE(
   'scott' ,'pkg_adm_util' ,'un_stand_value' ) :: TEXT ) ;
   DBMS_OUTPUT.PUT_LINE ( MIG_ORA_EXT.MIG_FN_GET_PKG_VARIABLE(
   'scott' ,'pkg_adm_util' ,'un_stand_value' ) :: TEXT ) ;
   DBMS_OUTPUT.PUT_LINE ( MIG_ORA_EXT.MIG_FN_GET_PKG_VARIABLE(
   'scott' ,'pkg_adm_util' ,'un_stand_value' ) :: TEXT ) ;
   END ;
   /

.. note::

   If **pkgSchemaNaming** is **true**.

   -  Oracle supports package variables for multiple schemas. If different schemas have the same package and variable names, such as:

      -  schema1.mypackage.myvariable
      -  schema2.mypackage.myvariable

      After migration, the schema names will not be used to differentiate the two package variables. Because schema names are ignored, the last data type declaration or operation for [*any_schema*]\ **.mypackage.myvariable** will overwrite the type and value for **schema1.mypackage.myvariable** and **schema2.mypackage.myvariable**.

**Input-Package variable with default value declared in one package by using CONSTANT keyword and used in another package**

The global variable declared in the package specification is accessed in the same or another package.

::

   PACKAGE "SAD"."BAS_SUBTYPE_PKG" : (Declaring global variable)
   -------------------------------------------------
   g_header_waiting_split_status CONSTANT VARCHAR2(20) := 'Waiting_Distribute';

   PACKAGE SAD.sad_lookup_stage_pkg: (Used global variable)
   --------------------------------------------------
   PROCEDURE calc_product_price(pi_contract_no   IN VARCHAR2 DEFAULT NULL,
                                  pi_stage_id      IN NUMBER DEFAULT NULL,
                                  pi_calc_category IN VARCHAR2 DEFAULT 'all',
                                  pi_op_code       IN NUMBER,
                                  po_error_msg     OUT VARCHAR2)
    IS

    CURSOR cur_contract IS
         SELECT DISTINCT sdh.contract_number, sdh.stage_id
           FROM sad_distribution_headers_t sdh
          WHERE sdh.status = bas_subtype_pkg.g_header_waiting_split_status
            AND sdh.contract_number = nvl(pi_contract_no, sdh.contract_number)
            AND sdh.stage_id = nvl(pi_stage_id, sdh.stage_id);

    v_ras_flag VARCHAR2 ( 1 ) ;
   BEGIN
   ..
   ...
   END calc_product_price;
   /

**Output**

::

   PROCEDURE calc_product_price(pi_contract_no   IN VARCHAR2 DEFAULT NULL,
                                  pi_stage_id      IN NUMBER DEFAULT NULL,
                                  pi_calc_category IN VARCHAR2 DEFAULT 'all',
                                  pi_op_code       IN NUMBER,
                                  po_error_msg     OUT VARCHAR2)
    IS

    MIG_PV_VAL_DUMMY_G_HEADER_WAITING_SPLIT_STATUS VARCHAR2 ( 20 ) := MIG_ORA_EXT.MIG_FN_GET_PKG_VARIABLE ( 'SAD' ,'bas_subtype_pkg' ,'g_header_waiting_split_status' ) ::VARCHAR2 ( 20 ) ;

    CURSOR cur_contract IS
         SELECT DISTINCT sdh.contract_number, sdh.stage_id
           FROM sad_distribution_headers_t sdh
          WHERE sdh.status = MIG_PV_VAL_DUMMY_G_HEADER_WAITING_SPLIT_STATUS
            AND sdh.contract_number = nvl(pi_contract_no, sdh.contract_number)
            AND sdh.stage_id = nvl(pi_stage_id, sdh.stage_id);

    v_ras_flag VARCHAR2 ( 1 ) ;

   BEGIN
   ..
   ...
   END;
   /

.. note::

   Package variables need to be declared before CURSOR declaration.

**Input-Variable of type EXCEPTION**

A package variable is a kind of global variable, which can be used in the entire package after being declared once.

::

   CREATE OR REPLACE PACKAGE BODY SAD.sad_lookup_stage_pkg IS

     ex_prog_error EXCEPTION;

   PROCEDURE assert_null ( pi_value IN VARCHAR2 )
   IS
   BEGIN
       IF pi_value IS NOT NULL THEN
               RAISE ex_prog_error ;

       END IF ;

   END assert_null;

   END SAD.sad_lookup_stage_pkg
   /

**Output**

::

   CREATE
        OR REPLACE PROCEDURE SAD.sad_lookup_stage_pkg#assert_null
    ( pi_value IN VARCHAR2 )
   PACKAGE
   IS
     ex_prog_error EXCEPTION;
   BEGIN
       IF pi_value IS NOT NULL THEN
               RAISE ex_prog_error ;

       END IF ;

   END ;
   /

.. note::

   As GaussDB does not have the software package functions, the package variable needs to be declared in the procedure or function.

**Input - If the configuration parameter pkgSchemaNaming is set to false**

A package variable is a kind of global variable, which can be used in the entire package after being declared once.

::

   CREATE OR REPLACE PACKAGE BODY SAD.bas_lookup_misc_pkg IS

     g_pkg_name CONSTANT VARCHAR2(30) := 'bas_lookup_misc_pkg';
     g_func_name VARCHAR2(30);

     FUNCTION func_name RETURN VARCHAR2 IS
       l_func_name VARCHAR2(100);
     BEGIN
       l_func_name := g_pkg_name || '.' || g_func_name;
       RETURN l_func_name;
     END;
   END SAD.bas_lookup_misc_pkg;
   /

**Output**

::

   INSERT INTO MIG_ORA_EXT.MIG_PKG_VARIABLES (
             PACKAGE_NAME
             ,SPEC_OR_BODY
             ,VARIABLE_NAME
             ,VARIABLE_TYPE
             ,CONSTANT_I
             ,DEFAULT_VALUE
             ,RUNTIME_EXEC_I
        )
        VALUES ( UPPER( 'bas_lookup_misc_pkg' )
        ,'B'
        ,UPPER( 'g_func_name' )
        ,UPPER( 'VARCHAR2(30)' )
        ,FALSE
        ,NULL
        ,FALSE ) ;

   END ;
   /
   --********************************************************************
   CREATE
        OR REPLACE FUNCTION SAD.bas_lookup_misc_pkg#func_name
        RETURN VARCHAR2
     PACKAGE
     IS
     l_func_name VARCHAR2 ( 100 ) ;
        MIG_PV_VAL_DUMMY_G_PKG_NAME VARCHAR2 ( 30 ) := MIG_ORA_EXT.MIG_FN_GET_PKG_VARIABLE ( 'SAD' ,'bas_lookup_misc_pkg' ,'g_pkg_name' ) ::VARCHAR2 ( 30 ) ;
        MIG_PV_VAL_DUMMY_G_FUNC_NAME VARCHAR2 ( 30 ) := MIG_ORA_EXT.MIG_FN_GET_PKG_VARIABLE ( 'SAD' ,'bas_lookup_misc_pkg' ,'g_func_name' ) ::VARCHAR2 ( 30 ) ;

   BEGIN
       l_func_name := MIG_PV_VAL_DUMMY_G_PKG_NAME || '.' || MIG_PV_VAL_DUMMY_G_FUNC_NAME ;

    MIG_ORA_EXT.MIG_FN_SET_PKG_VARIABLE ( 'SAD','bas_lookup_misc_pkg','g_pkg_name',MIG_PV_VAL_DUMMY_G_PKG_NAME ) ;
    MIG_ORA_EXT.MIG_FN_SET_PKG_VARIABLE ( 'SAD','bas_lookup_misc_pkg','g_func_name',MIG_PV_VAL_DUMMY_G_FUNC_NAME ) ;

       RETURN l_func_name ;


   END ;
   /

.. note::

   If the configuration parameter **pkgSchemaNaming** is set to **false**, package variable migration is not happening properly in some places (for example, GET to fetch default value and SET to assign final value are not added). This setting is not recommended by the kernel team. Please check with Kernel team.

**Input-Package variable declared with data type as table column %TYPE**

If a data type is declared as table column %TYPE for a variable, the data type which is defined on table creation level is considered to be the corresponding column.

::

   CREATE OR REPLACE PACKAGE BODY SAD.bas_lookup_misc_pkg IS

     v_emp_name emp.ename%TYPE;

   PROCEDURE save_emp_dtls ( v_empno IN VARCHAR2 )
   IS
   BEGIN

       IF v_emp_name IS NULL THEN
          v_emp_name := 'test';
       END IF ;

   END save_emp_dtls;

   END bas_lookup_misc_pkg
   /

**Output**

::

   BEGIN

        INSERT INTO MIG_ORA_EXT.MIG_PKG_VARIABLES (
             PACKAGE_NAME
             ,SPEC_OR_BODY
             ,VARIABLE_NAME
             ,VARIABLE_TYPE
             ,CONSTANT_I
             ,DEFAULT_VALUE
             ,RUNTIME_EXEC_I
        )
        VALUES ( UPPER( 'bas_lookup_misc_pkg' )
        ,'B'
        ,UPPER( 'v_emp_name' )
        ,UPPER( 'VARCHAR2(30)' )
        ,FALSE
        ,NULL
        ,FALSE ) ;

   END ;
   /
   --*********************************************************
   CREATE
        OR REPLACE PROCEDURE SAD.bas_lookup_misc_pkg#save_emp_dtls ( v_empno IN VARCHAR2 )
   PACKAGE
   IS
     MIG_PV_VAL_DUMMY_EMP_NAME VARCHAR2 ( 30 ) := MIG_ORA_EXT.MIG_FN_GET_PKG_VARIABLE ( 'SAD' ,'bas_lookup_misc_pkg' ,'v_emp_name' ) ::VARCHAR2 ( 30 ) ;
   BEGIN
       IF MIG_PV_VAL_DUMMY_EMP_NAME IS NULL THEN
          MIG_PV_VAL_DUMMY_EMP_NAME := 'test';
       END IF ;

   END ;
   /

.. note::

   While migrating a package variable with a data type as table column %TYPE, take the actual data type from a table and use it while declaring a variable, rather than using %TYPE.

**Input - If the configuration parameter "pkgSchemaNaming" is set to false**

If the PACKAGE name is specified along with the SCHEMA name, use the SCHEMA name on GET() to fetch the default value and SET() to assign the final value .

::

   CREATE OR REPLACE PACKAGE BODY SAD.bas_lookup_misc_pkg IS

     g_pkg_name CONSTANT VARCHAR2(30) := 'bas_lookup_misc_pkg';
     g_func_name VARCHAR2(30);

     FUNCTION func_name RETURN VARCHAR2 IS
       l_func_name VARCHAR2(100);
     BEGIN
       l_func_name := g_pkg_name || '.' || g_func_name;
       RETURN l_func_name;
     END;
   END SAD.bas_lookup_misc_pkg;
   /

**Output**

::

   BEGIN
        INSERT INTO MIG_ORA_EXT.MIG_PKG_VARIABLES (
             PACKAGE_NAME
             ,SPEC_OR_BODY
             ,VARIABLE_NAME
             ,VARIABLE_TYPE
             ,CONSTANT_I
             ,DEFAULT_VALUE
             ,RUNTIME_EXEC_I
        )
        VALUES ( UPPER( 'bas_lookup_misc_pkg' )
        ,'B'
        ,UPPER( 'g_pkg_name' )
        ,UPPER( 'VARCHAR2(30)' )
        ,TRUE
        ,'bas_lookup_misc_pkg'
        ,FALSE ) ;

        INSERT INTO MIG_ORA_EXT.MIG_PKG_VARIABLES (
             PACKAGE_NAME
             ,SPEC_OR_BODY
             ,VARIABLE_NAME
             ,VARIABLE_TYPE
             ,CONSTANT_I
             ,DEFAULT_VALUE
             ,RUNTIME_EXEC_I
        )
        VALUES ( UPPER( 'bas_lookup_misc_pkg' )
        ,'B'
        ,UPPER( 'g_func_name' )
        ,UPPER( 'VARCHAR2(30)' )
        ,FALSE
        ,NULL
        ,FALSE ) ;

   END ;
   /
   --********************************************************************
   CREATE
        OR REPLACE FUNCTION SAD.bas_lookup_misc_pkg#func_name
        RETURN VARCHAR2
     PACKAGE
     IS
     l_func_name VARCHAR2 ( 100 ) ;
        MIG_PV_VAL_DUMMY_G_PKG_NAME VARCHAR2 ( 30 ) := MIG_ORA_EXT.MIG_FN_GET_PKG_VARIABLE ( 'SAD' ,'bas_lookup_misc_pkg' ,'g_pkg_name' ) ::VARCHAR2 ( 30 ) ;
        MIG_PV_VAL_DUMMY_G_FUNC_NAME VARCHAR2 ( 30 ) := MIG_ORA_EXT.MIG_FN_GET_PKG_VARIABLE ( 'SAD' ,'bas_lookup_misc_pkg' ,'g_func_name' ) ::VARCHAR2 ( 30 ) ;

   BEGIN
       l_func_name := MIG_PV_VAL_DUMMY_G_PKG_NAME || '.' || MIG_PV_VAL_DUMMY_G_FUNC_NAME ;

    MIG_ORA_EXT.MIG_FN_SET_PKG_VARIABLE ( 'SAD','bas_lookup_misc_pkg','g_pkg_name',MIG_PV_VAL_DUMMY_G_PKG_NAME ) ;
    MIG_ORA_EXT.MIG_FN_SET_PKG_VARIABLE ( 'SAD','bas_lookup_misc_pkg','g_func_name',MIG_PV_VAL_DUMMY_G_FUNC_NAME ) ;

       RETURN l_func_name ;


   END ;
   /

**Input - If the configuration parameter pkgSchemaNaming is set to false**

If the configuration parameter **pkgSchemaNaming** is set to **false**.

::

   CREATE OR REPLACE PACKAGE BODY bas_lookup_misc_pkg IS

     g_pkg_name CONSTANT VARCHAR2(30) := 'bas_lookup_misc_pkg';
     g_func_name VARCHAR2(30);

     FUNCTION func_name RETURN VARCHAR2 IS
       l_func_name VARCHAR2(100);
     BEGIN
       l_func_name := g_pkg_name || '.' || g_func_name;
       RETURN l_func_name;
     END;
   END SAD.bas_lookup_misc_pkg;
   /

**Output**

::

   BEGIN
        INSERT INTO MIG_ORA_EXT.MIG_PKG_VARIABLES (
             PACKAGE_NAME
             ,SPEC_OR_BODY
             ,VARIABLE_NAME
             ,VARIABLE_TYPE
             ,CONSTANT_I
             ,DEFAULT_VALUE
             ,RUNTIME_EXEC_I
        )
        VALUES ( UPPER( 'bas_lookup_misc_pkg' )
        ,'B'
        ,UPPER( 'g_pkg_name' )
        ,UPPER( 'VARCHAR2(30)' )
        ,TRUE
        ,'bas_lookup_misc_pkg'
        ,FALSE ) ;

        INSERT INTO MIG_ORA_EXT.MIG_PKG_VARIABLES (
             PACKAGE_NAME
             ,SPEC_OR_BODY
             ,VARIABLE_NAME
             ,VARIABLE_TYPE
             ,CONSTANT_I
             ,DEFAULT_VALUE
             ,RUNTIME_EXEC_I
        )
        VALUES ( UPPER( 'bas_lookup_misc_pkg' )
        ,'B'
        ,UPPER( 'g_func_name' )
        ,UPPER( 'VARCHAR2(30)' )
        ,FALSE
        ,NULL
        ,FALSE ) ;

   END ;
   /
   --********************************************************************
   CREATE
        OR REPLACE FUNCTION bas_lookup_misc_pkg#func_name
        RETURN VARCHAR2
     PACKAGE
     IS
     l_func_name VARCHAR2 ( 100 ) ;
        MIG_PV_VAL_DUMMY_G_PKG_NAME VARCHAR2 ( 30 ) := MIG_ORA_EXT.MIG_FN_GET_PKG_VARIABLE ( CURRENT_SCHEMA() ,'bas_lookup_misc_pkg' ,'g_pkg_name' ) ::VARCHAR2 ( 30 ) ;
        MIG_PV_VAL_DUMMY_G_FUNC_NAME VARCHAR2 ( 30 ) := MIG_ORA_EXT.MIG_FN_GET_PKG_VARIABLE ( CURRENT_SCHEMA() ,'bas_lookup_misc_pkg' ,'g_func_name' ) ::VARCHAR2 ( 30 ) ;

   BEGIN
       l_func_name := MIG_PV_VAL_DUMMY_G_PKG_NAME || '.' || MIG_PV_VAL_DUMMY_G_FUNC_NAME ;

    MIG_ORA_EXT.MIG_FN_SET_PKG_VARIABLE ( CURRENT_SCHEMA(),'bas_lookup_misc_pkg','g_pkg_name',MIG_PV_VAL_DUMMY_G_PKG_NAME ) ;
    MIG_ORA_EXT.MIG_FN_SET_PKG_VARIABLE ( CURRENT_SCHEMA(),'bas_lookup_misc_pkg','g_func_name',MIG_PV_VAL_DUMMY_G_FUNC_NAME ) ;

       RETURN l_func_name ;


   END ;
   /

**Input: if pkgSchemaNaming is set to false, package variable**

The global variable is not correctly converted during package conversion, and an error is reported during compilation. If the configuration parameter **pkgSchemaNaming** is set to **false**, package variable migration is not happening properly in some places. This setting is not recommended by Kernel team. Please check with Kernel team.

::

   CREATE OR REPLACE PACKAGE BODY SAD.bas_dml_lookup_pkg IS
     g_pkg_name CONSTANT VARCHAR2(30) := 'bas_dml_ic_price_rule_pkg' ;
     g_func_name VARCHAR2 (100);

     FUNCTION func_name
     RETURN VARCHAR2
     IS
       l_func_name VARCHAR2(100) ;
     BEGIN
        l_func_name := g_pkg_name || '.' || g_func_name ;
        RETURN l_func_name ;

      END ;

   END bas_dml_lookup_pkg ;
   /

**Output**

::

   BEGIN
        INSERT INTO MIG_ORA_EXT.MIG_PKG_VARIABLES (
               USER_NAME, PACKAGE_NAME, SPEC_OR_BODY
             , VARIABLE_NAME, VARIABLE_TYPE
             , CONSTANT_I, DEFAULT_VALUE, RUNTIME_EXEC_I
        )
        VALUES ( 'SAD', UPPER( 'bas_dml_lookup_pkg' ), 'B'
               , UPPER( 'g_pkg_name' ), UPPER( 'VARCHAR2 ( 30 )' )
               , TRUE, 'bas_dml_ic_price_rule_pkg', FALSE ) ;

        INSERT INTO MIG_ORA_EXT.MIG_PKG_VARIABLES (
               USER_NAME, PACKAGE_NAME, SPEC_OR_BODY
             , VARIABLE_NAME, VARIABLE_TYPE
             , CONSTANT_I, DEFAULT_VALUE, RUNTIME_EXEC_I
        )
        VALUES ( 'SAD', UPPER( 'bas_dml_lookup_pkg' ), 'B'
               , UPPER( 'g_func_name' ), UPPER( 'VARCHAR2(100)' )
               , FALSE, NULL, FALSE ) ;

   END ;
   /

   CREATE OR REPLACE FUNCTION SAD.bas_dml_lookup_pkg#func_name
   RETURN VARCHAR2
   IS
        MIG_PV_VAL_DUMMY_G_PKG_NAME VARCHAR2(30) := MIG_ORA_EXT.MIG_FN_GET_PKG_VARIABLE ( 'SAD', 'BAS_DML_LOOKUP_PKG', 'G_PKG_NAME' )::VARCHAR2(30) ;
        MIG_PV_VAL_DUMMY_G_FUNC_NAME VARCHAR2(100) := MIG_ORA_EXT.MIG_FN_GET_PKG_VARIABLE ( 'SAD', 'BAS_DML_LOOKUP_PKG', 'G_FUNC_NAME' )::VARCHAR2(100) ;
        l_func_name VARCHAR2(100) ;
   BEGIN
        l_func_name := MIG_PV_VAL_DUMMY_G_PKG_NAME || '.' || MIG_PV_VAL_DUMMY_G_FUNC_NAME ;
        RETURN l_func_name ;

   END ;
   /

**Input: table field type definition in the (%type) table**

During package conversion, the schema definition is not added to the table field type definition in the (%type) table. An error is reported during compilation.

::

   CREATE TABLE CTP_BRANCH
        ( ID            VARCHAR2(10)
     , NAME          VARCHAR2(100)
     , DESCRIPTION   VARCHAR2(500)
     );

   CREATE OR REPLACE PACKAGE BODY SAD.bas_dml_lookup_pkg IS
     g_pkg_name CONSTANT VARCHAR2(30) := 'bas_dml_ic_price_rule_pkg' ;
     g_func_name CTP_BRANCH.NAME%TYPE;

     FUNCTION func_name
     RETURN VARCHAR2
     IS
       l_func_name VARCHAR2(100) ;
     BEGIN
        l_func_name := g_pkg_name || '.' || g_func_name ;
        RETURN l_func_name ;

      END ;

   END bas_dml_lookup_pkg ;
   /

**Output**

::

   BEGIN
        INSERT INTO MIG_ORA_EXT.MIG_PKG_VARIABLES (
               USER_NAME, PACKAGE_NAME, SPEC_OR_BODY
             , VARIABLE_NAME, VARIABLE_TYPE
             , CONSTANT_I, DEFAULT_VALUE, RUNTIME_EXEC_I
        )
        VALUES ( 'SAD', UPPER( 'bas_dml_lookup_pkg' ), 'B'
               , UPPER( 'g_pkg_name' ), UPPER( 'VARCHAR2 ( 30 )' )
               , TRUE, 'bas_dml_ic_price_rule_pkg', FALSE ) ;

        INSERT INTO MIG_ORA_EXT.MIG_PKG_VARIABLES (
               USER_NAME, PACKAGE_NAME, SPEC_OR_BODY
             , VARIABLE_NAME, VARIABLE_TYPE
             , CONSTANT_I, DEFAULT_VALUE, RUNTIME_EXEC_I
        )
        VALUES ( 'SAD', UPPER( 'bas_dml_lookup_pkg' ), 'B'
               , UPPER( 'g_func_name' ), UPPER( 'VARCHAR2(100)' )
               , FALSE, NULL, FALSE ) ;

   END ;
   /
   CREATE OR REPLACE FUNCTION SAD.bas_dml_lookup_pkg#func_name
   RETURN VARCHAR2
   IS
        MIG_PV_VAL_DUMMY_G_PKG_NAME VARCHAR2(30) := MIG_ORA_EXT.MIG_FN_GET_PKG_VARIABLE ( 'SAD', 'BAS_DML_LOOKUP_PKG', 'G_PKG_NAME' )::VARCHAR2(30) ;
        MIG_PV_VAL_DUMMY_G_FUNC_NAME VARCHAR2(100) := MIG_ORA_EXT.MIG_FN_GET_PKG_VARIABLE ( 'SAD', 'BAS_DML_LOOKUP_PKG', 'G_FUNC_NAME' )::VARCHAR2(100) ;
        l_func_name VARCHAR2(100) ;
   BEGIN
        l_func_name := MIG_PV_VAL_DUMMY_G_PKG_NAME || '.' || MIG_PV_VAL_DUMMY_G_FUNC_NAME ;
        RETURN l_func_name ;

   END ;
   /

**EXCEPTION**

Package variables can be declare as **EXCEPTION**. GaussDB(DWS) does not support this function.

**Input**

::

   CREATE OR REPLACE PACKAGE BODY product_pkg IS

     ex_prog_error EXCEPTION;

     PROCEDURE assert_null(pi_value IN VARCHAR2) IS
     BEGIN
       IF pi_value IS NOT NULL
       THEN
         RAISE ex_prog_error;
       END IF;
     EXCEPTION
       WHEN ex_prog_error THEN
         RAISE ex_prog_error;

     END assert_null;
   END product_pkg;
   /

**Output**

::

   CREATE OR REPLACE PROCEDURE product_pkg.Assert_null (pi_value IN VARCHAR2)
   IS
     ex_prog_error EXCEPTION;
   BEGIN
       IF pi_value IS NOT NULL THEN
         RAISE ex_prog_error;
       END IF;
   EXCEPTION
     WHEN ex_prog_error THEN
                RAISE ex_prog_error;
   END;

   /

**Default value**

function is specified as a default value for a package variable.

**Input**

::

   BEGIN
        INSERT INTO MIG_ORA_EXT.MIG_PKG_VARIABLES (
             PACKAGE_NAME
             ,SPEC_OR_BODY
             ,VARIABLE_NAME
             ,VARIABLE_TYPE
             ,CONSTANT_I
             ,DEFAULT_VALUE
             ,RUNTIME_EXEC_I
        )
        VALUES ( UPPER( 'PKG_REVN_ARPU' )
        ,'B'
        ,UPPER( 'imodel' )
        ,UPPER( 'log_table.ds_exec%TYPE' )
        ,FALSE
        ,pkg_etl.proc_set_chain ( 'DAILY ARPU' )
        ,FALSE ) ;

   END ;
   /
   gSQL:PKG_REVN_ARPU_04.SQL:23: ERROR:  function pkg_etl.proc_set_chain(unknown) does not exist
   LINE 15:      ,pkg_etl.proc_set_chain ( 'DAILY ARPU' )
                  ^
   HINT:  No function matches the given name and argument types. You might need to add explicit type casts.



   CREATE OR REPLACE PACKAGE BODY IC_STAGE.PKG_REVN_ARPU
   AS
    imodel   log_table.ds_exec%TYPE := pkg_etl.proc_set_chain ('DAILY ARPU');
   PROCEDURE AGGR_X_AGG00_REVN_DEALER (p_date    PLS_INTEGER,
                                          p_days    PLS_INTEGER)
      AS
         v_start_date   PLS_INTEGER;
         v_curr_date    PLS_INTEGER;
      v_imodel   VARCHAR2(100);
      BEGIN
         pkg_etl.proc_start (p_date, 'AGGR_X_AGG00_REVN_DEALER ');

         v_start_date :=
            TO_CHAR (TO_DATE (p_date, 'yyyymmdd') - (p_days - 1), 'yyyymmdd');
         v_curr_date := p_date;
      v_imodel := imodel;

      END;
   END PKG_REVN_ARPU;
   /

**Output**

::

   SET
        package_name_list = 'PKG_REVN_ARPU' ;

   BEGIN
        INSERT INTO MIG_ORA_EXT.MIG_PKG_VARIABLES (
             PACKAGE_NAME
             ,SPEC_OR_BODY
             ,VARIABLE_NAME
             ,VARIABLE_TYPE
             ,CONSTANT_I
             ,DEFAULT_VALUE
             ,RUNTIME_EXEC_I
        )
        VALUES ( UPPER( 'PKG_REVN_ARPU' )
        ,'B'
        ,UPPER( 'imodel' )
        ,UPPER( 'log_table.ds_exec%TYPE' )
        ,FALSE
        ,$q$pkg_etl.proc_set_chain ('DAILY ARPU')$q$
        ,TRUE ) ;

   END ;
   /
   CREATE
        OR REPLACE PROCEDURE PKG_REVN_ARPU.AGGR_X_AGG00_REVN_DEALER ( p_date INTEGER
        ,p_days INTEGER )
     AS
     MIG_PV_VAL_DUMMY_IMODEL log_table.ds_exec%TYPE := MIG_ORA_EXT.MIG_FN_GET_PKG_VARIABLE ( CURRENT_USER,'PKG_REVN_ARPU','imodel' ) ::log_table.ds_exec%TYPE ;
        v_start_date INTEGER ;
        v_curr_date INTEGER ;
        v_imodel VARCHAR2 ( 100 ) ;

   BEGIN
        pkg_etl.proc_start ( p_date ,'AGGR_X_AGG00_REVN_DEALER ' ) ;
        v_start_date := TO_CHAR( TO_DATE( p_date ,'yyyymmdd' ) - ( p_days - 1 ),'yyyymmdd' ) ;
        v_curr_date := p_date ;
        v_imodel := MIG_PV_VAL_DUMMY_IMODEL ;
        MIG_ORA_EXT.MIG_FN_SET_PKG_VARIABLE ( CURRENT_USER,'PKG_REVN_ARPU','imodel',MIG_PV_VAL_DUMMY_IMODEL ) ;

   END ;
   /
   reset package_name_list ;

**PLS_INTEGER**

A PLS_INTEGER datatype is not converted into INTEGER for package variables but it is working fine for other local variables. Therefore, it should be converted to INTEGER, such as, varaible1 PLS_INTEGER ==> varaible1 INTEGER

SCRIPTS: SAD_CALC_BPART_PRICE_PKG.sql, SAD_CALC_ITEM_PKG_TEST_OB.sql, SAD_CALC_ITEM_PRICE_TEST_OB.sql, SAD_CALC_ITEM_PRI_TEST_OB.sql, SAD_CALC_ITEM_TEST_OB.sql

**Input**

::

   CREATE OR REPLACE PACKAGE BODY "SAD"."SAD_CALC_BPART_PRICE_PKG" IS
   g_max_number_of_entities PLS_INTEGER := 100;
   FUNCTION split_warning(pi_contract_number IN VARCHAR2,
   pi_stage_id        IN NUMBER,
   pi_quotation_id    IN NUMBER,
   pi_cfg_instance_id IN NUMBER) RETURN VARCHAR2 IS
   BEGIN
   ---
   l_item_list := items_no_cost(pi_contract_number        => pi_contract_number,
   pi_stage_id               => pi_stage_id,
   pi_quotation_id           => pi_quotation_id,
   pi_cfg_instance_id        => pi_cfg_instance_id,
   pi_max_number_of_entities => g_max_number_of_entities,
   pi_sep_char               => g_item_sep_char,
   po_error_msg              => po_error_msg);
   ---
   END split_warning;
   END SAD_CALC_BPART_PRICE_PKG;

**Output**

::

   BEGIN
   ---
   INSERT INTO MIG_ORA_EXT.MIG_PKG_VARIABLES (
   PACKAGE_NAME
   ,SPEC_OR_BODY
   ,VARIABLE_NAME
   ,VARIABLE_TYPE
   ,CONSTANT_I
   ,DEFAULT_VALUE
   ,RUNTIME_EXEC_I
   )
   VALUES ( UPPER( 'SAD_CALC_BPART_PRICE_PKG' )
   ,'B'
   ,UPPER( 'g_max_number_of_entities' )
   ,UPPER( 'PLS_INTEGER' )
   ,FALSE
   ,100
   ,FALSE ) ;
   ---
   END;
   /
   CREATE
   OR REPLACE FUNCTION SAD.SAD_CALC_BPART_PRICE_PKG#split_warning ( pi_contract_number IN VARCHAR2
   ,pi_stage_id IN NUMBER
   ,pi_quotation_id IN NUMBER
   ,pi_cfg_instance_id IN NUMBER )
   RETURN VARCHAR2 IS
   ---
   MIG_PV_VAL_DUMMY_G_MAX_NUMBER_OF_ENTITIES PLS_INTEGER := MIG_ORA_EXT.MIG_FN_GET_PKG_VARIABLE ( current_schema ( )
   ,'SAD_CALC_BPART_PRICE_PKG'
   ,'g_max_number_of_entities' ) ::PLS_INTEGER ;
   ---
   l_item_list := SAD.SAD_CALC_BPART_PRICE_PKG#items_no_cost ( pi_contract_number => pi_contract_number ,
   pi_stage_id => pi_stage_id ,
   pi_quotation_id => pi_quotation_id ,
   pi_cfg_instance_id => pi_cfg_instance_id ,
   pi_max_number_of_entities => MIG_PV_VAL_DUMMY_G_MAX_NUMBER_OF_ENTITIES ,
   pi_sep_char => MIG_PV_VAL_DUMMY_G_ITEM_SEP_CHAR ,
   po_error_msg => po_error_msg ) ;
   ---
   END;

**Input**

::

   PLS_INTEGER datatype not converted into INTEGER for package variables but it's working fine for other local variables therefore for package variables also PLS_INTEGER should be converted to INTEGER datatype i.e varaible1 PLS_INTEGER ==> varaible1 INTEGER

   SCRIPTS : SAD_CALC_BPART_PRICE_PKG.SQL, SAD_CALC_ITEM_PKG_TEST_OB.SQL, SAD_CALC_ITEM_PRICE_TEST_OB.SQL, SAD_CALC_ITEM_PRI_TEST_OB.SQL, SAD_CALC_ITEM_TEST_OB.SQL

   INPUT :

   CREATE OR REPLACE PACKAGE BODY "SAD"."SAD_CALC_BPART_PRICE_PKG" IS

    g_max_number_of_entities PLS_INTEGER := 100;

    FUNCTION split_warning(pi_contract_number IN VARCHAR2,
                            pi_stage_id        IN NUMBER,
                            pi_quotation_id    IN NUMBER,
                            pi_cfg_instance_id IN NUMBER) RETURN VARCHAR2 IS

     BEGIN
     ---

     l_item_list := items_no_cost(pi_contract_number        => pi_contract_number,
                                    pi_stage_id               => pi_stage_id,
                                    pi_quotation_id           => pi_quotation_id,
                                    pi_cfg_instance_id        => pi_cfg_instance_id,
                                    pi_max_number_of_entities => g_max_number_of_entities,
                                    pi_sep_char               => g_item_sep_char,
                                    po_error_msg              => po_error_msg);

     ---

     END split_warning;

   END SAD_CALC_BPART_PRICE_PKG;


   OUTPUT :

   BEGIN

   ---
   INSERT INTO MIG_ORA_EXT.MIG_PKG_VARIABLES (
             PACKAGE_NAME
             ,SPEC_OR_BODY
             ,VARIABLE_NAME
             ,VARIABLE_TYPE
             ,CONSTANT_I
             ,DEFAULT_VALUE
             ,RUNTIME_EXEC_I
        )
        VALUES ( UPPER( 'SAD_CALC_BPART_PRICE_PKG' )
        ,'B'
        ,UPPER( 'g_max_number_of_entities' )
        ,UPPER( 'PLS_INTEGER' )
        ,FALSE
        ,100
        ,FALSE ) ;
   ---

   END;
   /

   CREATE
        OR REPLACE FUNCTION SAD.SAD_CALC_BPART_PRICE_PKG#split_warning ( pi_contract_number IN VARCHAR2
        ,pi_stage_id IN NUMBER
        ,pi_quotation_id IN NUMBER
        ,pi_cfg_instance_id IN NUMBER )
        RETURN VARCHAR2 IS

     ---

        MIG_PV_VAL_DUMMY_G_MAX_NUMBER_OF_ENTITIES PLS_INTEGER := MIG_ORA_EXT.MIG_FN_GET_PKG_VARIABLE ( current_schema ( )
        ,'SAD_CALC_BPART_PRICE_PKG'
        ,'g_max_number_of_entities' ) ::PLS_INTEGER ;

     ---

     l_item_list := SAD.SAD_CALC_BPART_PRICE_PKG#items_no_cost ( pi_contract_number => pi_contract_number ,
                   pi_stage_id => pi_stage_id ,
                   pi_quotation_id => pi_quotation_id ,
                   pi_cfg_instance_id => pi_cfg_instance_id ,
                   pi_max_number_of_entities => MIG_PV_VAL_DUMMY_G_MAX_NUMBER_OF_ENTITIES ,
                   pi_sep_char => MIG_PV_VAL_DUMMY_G_ITEM_SEP_CHAR ,
                   po_error_msg => po_error_msg ) ;
     ---

   END;

**Output**

::

   BEGIN
        INSERT INTO MIG_ORA_EXT.MIG_PKG_VARIABLES
      (  PACKAGE_NAME, SPEC_OR_BODY, VARIABLE_NAME
             , VARIABLE_TYPE, CONSTANT_I, DEFAULT_VALUE
             , RUNTIME_EXEC_I )
        VALUES ( UPPER('SAD_CALC_BPART_PRICE_PKG')
         , 'B', UPPER( 'g_max_number_of_entities' )
         , UPPER( 'INTEGER' ),FALSE,100
         , FALSE ) ;
   END ;
   /

   CREATE OR REPLACE FUNCTION SAD.SAD_CALC_BPART_PRICE_PKG#split_warning
    ( pi_contract_number IN VARCHAR2
       , pi_stage_id   IN NUMBER )
   RETURN VARCHAR2
   PACKAGE
   IS
    MIG_PV_VAL_DUMMY_G_MAX_NUMBER_OF_ENTITIES INTEGER := MIG_ORA_EXT.MIG_FN_GET_PKG_VARIABLE('SAD', 'SAD_CALC_BPART_PRICE_PKG', 'g_max_number_of_entities') ::INTEGER ;
       po_error_msg sad_products_t.exception_description%TYPE ;

   BEGIN
        l_item_list := items_no_cost ( pi_contract_number => pi_contract_number ,pi_stage_id => pi_stage_id
             , pi_max_number_of_entities => MIG_PV_VAL_DUMMY_G_MAX_NUMBER_OF_ENTITIES
             , po_error_msg => po_error_msg ) ;
        MIG_ORA_EXT.MIG_FN_SET_PKG_VARIABLE ('SAD' ,'SAD_CALC_BPART_PRICE_PKG' ,'g_max_number_of_entities' ,MIG_PV_VAL_DUMMY_G_MAX_NUMBER_OF_ENTITIES);

        RETURN po_error_msg ;

   EXCEPTION
       WHEN OTHERS THEN
           po_error_msg := 'Program Others abnormal, Fail to obtain the warning information.' || SQLERRM ;
           MIG_ORA_EXT.MIG_FN_SET_PKG_VARIABLE ( 'SAD' ,'SAD_CALC_BPART_PRICE_PKG' ,'g_max_number_of_entities' ,MIG_PV_VAL_DUMMY_G_MAX_NUMBER_OF_ENTITIES ) ;

           RETURN po_error_msg ;

   END ;
   /

**Cursor With Package Variable**

The cursor declared in SAD.sad_calc_product_price_pkg#calc_product_price contains package variables and needs to be handled.

**Input**

::

   CREATE OR REPLACE PACKAGE SAD.bas_subtype_pkg IS
     g_header_waiting_split_status CONSTANT VARCHAR2(20) := 'Waiting_Distribute';
     SUBTYPE error_msg IS sad_products_t.exception_description%TYPE;
   END bas_subtype_pkg;
   /

   CREATE OR REPLACE PACKAGE BODY SAD.sad_calc_product_price_pkg IS
     PROCEDURE calc_product_price(pi_contract_no   IN VARCHAR2 DEFAULT NULL,
                                  pi_stage_id      IN NUMBER DEFAULT NULL,
                                  po_error_msg     OUT VARCHAR2) IS
       CURSOR cur_contract IS
         SELECT DISTINCT sdh.contract_number, sdh.stage_id
           FROM sad_distribution_headers_t sdh
          WHERE sdh.status = bas_subtype_pkg.g_header_waiting_split_status
            AND sdh.contract_number = nvl(pi_contract_no, sdh.contract_number)
            AND sdh.stage_id = nvl(pi_stage_id, sdh.stage_id);

       lv_error_msg bas_subtype_pkg.error_msg;
     BEGIN
       FOR rec_contract IN cur_contract
       LOOP

           validate_process_status(rec_contract.contract_number,
                                   rec_contract.stage_id,
                                   lv_error_msg);
       END LOOP;

    po_error_msg := lv_error_msg;
     END calc_product_price;

   END sad_calc_product_price_pkg;
   /

**Output**

::

   BEGIN
        INSERT INTO MIG_ORA_EXT.MIG_PKG_VARIABLES
       ( PACKAGE_NAME,SPEC_OR_BODY,VARIABLE_NAME
       , VARIABLE_TYPE,CONSTANT_I,DEFAULT_VALUE
       , RUNTIME_EXEC_I )
        VALUES ( UPPER('bas_subtype_pkg'), 'S', UPPER('g_header_waiting_split_status')
       , UPPER( 'VARCHAR2(20)' ), TRUE, 'Waiting_Distribute'
       , FALSE ) ;
   END ;
   /

   CREATE OR REPLACE PROCEDURE SAD.sad_calc_product_price_pkg#calc_product_price
    ( pi_contract_no IN VARCHAR2 DEFAULT NULL
       , pi_stage_id IN NUMBER DEFAULT NULL
       , po_error_msg OUT VARCHAR2 )
   PACKAGE
   IS
    MIG_PV_VAL_DUMMY_G_HEADER_WAITING_SPLIT_STATUS VARCHAR2 ( 20 ) := MIG_ORA_EXT.MIG_FN_GET_PKG_VARIABLE ( 'SAD' ,'bas_subtype_pkg'
       ,'g_header_waiting_split_status' ) ::VARCHAR2 ( 20 ) ;

    CURSOR cur_contract IS
    SELECT DISTINCT sdh.contract_number, sdh.stage_id
         FROM sad_distribution_headers_t sdh
        WHERE sdh.status = MIG_PV_VAL_DUMMY_G_HEADER_WAITING_SPLIT_STATUS
          AND sdh.contract_number = nvl( pi_contract_no ,sdh.contract_number )
          AND sdh.stage_id = nvl( pi_stage_id ,sdh.stage_id ) ;

       lv_error_msg sad_products_t.exception_description%TYPE ;
   BEGIN
        FOR rec_contract IN cur_contract
     LOOP
             validate_process_status ( rec_contract.contract_number ,rec_contract.stage_id ,lv_error_msg ) ;

        END LOOP ;
        po_error_msg := lv_error_msg ;
        MIG_ORA_EXT.MIG_FN_SET_PKG_VARIABLE ( 'SAD' ,'bas_subtype_pkg' ,'g_header_waiting_split_status' ,MIG_PV_VAL_DUMMY_G_HEADER_WAITING_SPLIT_STATUS ) ;

   END ;
   /

**SET VARIABLE function after the RETURN**

SET VARIABLE function should be called before the RETURN statements in the procedure and function.

**Input**

::

   CREATE OR REPLACE PACKAGE BODY SAD.bas_dml_lookup_pkg IS
     g_pkg_name CONSTANT VARCHAR2(30) := 'bas_dml_lookup_pkg' ;
     g_func_name VARCHAR2(100);

     FUNCTION func_name
     RETURN VARCHAR2
     IS
       l_func_name VARCHAR2(100) ;
     BEGIN
     g_func_name := 'func_name';
        l_func_name := g_pkg_name || '.' || g_func_name ;
        RETURN l_func_name ;

      END;

     PROCEDURE data_change_logs ( pi_table_name        IN VARCHAR2
                                , pi_table_key_columns IN VARCHAR2
                                , po_error_msg         OUT VARCHAR2
           )
     IS
     BEGIN
       g_func_name := 'data_change_logs';

    IF pi_table_name IS NULL
    THEN
     RETURN;
    END IF;

       INSERT INTO fnd_data_change_logs_t
         ( logid, table_name, table_key_columns )
       VALUES
         ( fnd_data_change_logs_t_s.NEXTVAL
         , pi_table_name, pi_table_key_columns );
     EXCEPTION
       WHEN OTHERS THEN
         po_error_msg := 'Others Exception raise in ' || func_name || ',' || SQLERRM;
     END data_change_logs;

   END bas_dml_lookup_pkg;
   /

**Output**

::

   BEGIN
        INSERT INTO MIG_ORA_EXT.MIG_PKG_VARIABLES
     ( PACKAGE_NAME,SPEC_OR_BODY,VARIABLE_NAME
     , VARIABLE_TYPE,CONSTANT_I,DEFAULT_VALUE
     , RUNTIME_EXEC_I )
        VALUES ( UPPER('bas_dml_lookup_pkg'), 'B', UPPER('g_pkg_name')
       , UPPER( 'VARCHAR2(30)' ), TRUE, 'bas_dml_lookup_pkg'
       , FALSE ) ;

        INSERT INTO MIG_ORA_EXT.MIG_PKG_VARIABLES
     ( PACKAGE_NAME,SPEC_OR_BODY,VARIABLE_NAME
     , VARIABLE_TYPE,CONSTANT_I,DEFAULT_VALUE
     , RUNTIME_EXEC_I )
     VALUES ( UPPER('bas_dml_lookup_pkg'), 'B', UPPER('g_func_name')
      , UPPER( 'VARCHAR2(100)' ), FALSE, NULL, FALSE ) ;

   END ;
   /
   CREATE OR REPLACE FUNCTION SAD.bas_dml_lookup_pkg#func_name
   RETURN VARCHAR2
   PACKAGE
   IS
    MIG_PV_VAL_DUMMY_G_PKG_NAME VARCHAR2 ( 30 ) := MIG_ORA_EXT.MIG_FN_GET_PKG_VARIABLE ( 'SAD' ,'bas_dml_lookup_pkg' ,'g_pkg_name' ) ::VARCHAR2 ( 30 ) ;
        MIG_PV_VAL_DUMMY_G_FUNC_NAME VARCHAR2 ( 100 ) := MIG_ORA_EXT.MIG_FN_GET_PKG_VARIABLE ( 'SAD' ,'bas_dml_lookup_pkg' ,'g_func_name' ) ::VARCHAR2 ( 100 ) ;
        l_func_name VARCHAR2 ( 100 ) ;

   BEGIN
        MIG_PV_VAL_DUMMY_G_FUNC_NAME := 'func_name' ;
        l_func_name := MIG_PV_VAL_DUMMY_G_PKG_NAME || '.' || MIG_PV_VAL_DUMMY_G_FUNC_NAME ;
        MIG_ORA_EXT.MIG_FN_SET_PKG_VARIABLE ( 'SAD' ,'bas_dml_lookup_pkg' ,'g_func_name' ,MIG_PV_VAL_DUMMY_G_FUNC_NAME ) ;
        MIG_ORA_EXT.MIG_FN_SET_PKG_VARIABLE ( 'SAD' ,'bas_dml_lookup_pkg' ,'g_pkg_name' ,MIG_PV_VAL_DUMMY_G_PKG_NAME ) ;

        RETURN l_func_name ;
   END ;
   /

   CREATE OR REPLACE PROCEDURE SAD.bas_dml_lookup_pkg#data_change_logs
    ( pi_table_name IN VARCHAR2
       , pi_table_key_columns IN VARCHAR2
       , po_error_msg OUT VARCHAR2 )
   PACKAGE
   IS
    MIG_PV_VAL_DUMMY_G_FUNC_NAME VARCHAR2 ( 100 ) := MIG_ORA_EXT.MIG_FN_GET_PKG_VARIABLE ( 'SAD' ,'bas_dml_lookup_pkg' ,'g_func_name' ) ::VARCHAR2 ( 100 ) ;
   BEGIN
        MIG_PV_VAL_DUMMY_G_FUNC_NAME := 'data_change_logs' ;

        IF pi_table_name IS NULL THEN
           MIG_ORA_EXT.MIG_FN_SET_PKG_VARIABLE ( 'SAD' ,'bas_dml_lookup_pkg' ,'g_func_name' ,MIG_PV_VAL_DUMMY_G_FUNC_NAME ) ;
     RETURN ;
        END IF ;

        INSERT INTO fnd_data_change_logs_t ( logid, table_name, table_key_columns )
        VALUES ( NEXTVAL ( 'fnd_data_change_logs_t_s' ), pi_table_name, pi_table_key_columns ) ;

        MIG_ORA_EXT.MIG_FN_SET_PKG_VARIABLE ( 'SAD' ,'bas_dml_lookup_pkg' ,'g_func_name' ,MIG_PV_VAL_DUMMY_G_FUNC_NAME ) ;

   EXCEPTION
       WHEN OTHERS THEN
           po_error_msg := 'Others Exception raise in ' || SAD.bas_dml_lookup_pkg#func_name ( ) || ',' || SQLERRM ;
     MIG_ORA_EXT.MIG_FN_SET_PKG_VARIABLE ( 'SAD' ,'bas_dml_lookup_pkg' ,'g_func_name' ,MIG_PV_VAL_DUMMY_G_FUNC_NAME ) ;
   END ;
   /

**Empty package**

Empty package bodies do not need to be migrated.

**Input**

::

   CREATE OR REPLACE PACKAGE BODY SAD.bas_subtype_pkg IS
   BEGIN
     NULL;
   END bas_subtype_pkg;
   /

Output will be an empty file.
