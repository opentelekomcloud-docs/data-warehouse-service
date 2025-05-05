:original_name: dws_mt_0304.html

.. _dws_mt_0304:

Granting Execution Permissions
==============================

This feature is used to give privileges to users for specific packages. All the procedures and functions defined in the specific packages will be granted the execution permission.

**Input**

::

   GRANT EXECUTE ON SAD.BAS_LOOKUP_MISC_PKG TO EIP_SAD;

**Output**

::

   GRANT EXECUTE ON procedure_name TO EIP_SAD;
   GRANT EXECUTE ON function1_name TO EIP_SAD;

.. note::

   Both procedure \_name and function1_name must belong to SAD.BAS_LOOKUP_MISC_PKG.

**The execution permission**

The last authorization of the package is not converted.

--GRANT

**Input**

::

   Below should be created as 1spec/t603.SQL
   CREATE OR REPLACE PACKAGE SAD.bas_dml_lookup_pkg IS
    FUNCTION func_name RETURN VARCHAR2;
    PROCEDURE data_change_logs ( pi_table_name        IN VARCHAR2
                                  , pi_table_key_columns IN VARCHAR2
                                  , po_error_msg         OUT VARCHAR2
             );
   END bas_dml_lookup_pkg;
   /
   GRANT EXECUTE ON SAD.bas_dml_lookup_pkg TO eip_sad;
   ==============================
   Below should be created as 2body/t603.SQL
   CREATE OR REPLACE PACKAGE BODY SAD.bas_dml_lookup_pkg IS
     g_pkg_name CONSTANT VARCHAR2(30) := 'bas_dml_ic_price_rule_pkg' ;
     g_func_name VARCHAR2(100);

     FUNCTION func_name
     RETURN VARCHAR2
     IS
       l_func_name VARCHAR2(100) ;
     BEGIN
        l_func_name := g_pkg_name || '.' || g_func_name ;
        RETURN l_func_name ;

      END func_name;

     PROCEDURE data_change_logs ( pi_table_name        IN VARCHAR2
                                , pi_table_key_columns IN VARCHAR2
                                , po_error_msg         OUT VARCHAR2
           )
     IS
     BEGIN
       ...
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
         , UPPER( 'VARCHAR2(30)' ),TRUE,'bas_dml_ic_price_rule_pkg'
         , FALSE ) ;

        INSERT INTO MIG_ORA_EXT.MIG_PKG_VARIABLES
       ( PACKAGE_NAME,SPEC_OR_BODY,VARIABLE_NAME
       , VARIABLE_TYPE,CONSTANT_I,DEFAULT_VALUE
       , RUNTIME_EXEC_I )
        VALUES ( UPPER('bas_dml_lookup_pkg'), 'B', UPPER( 'g_func_name' )
      , UPPER( 'VARCHAR2(100)' ),FALSE,NULL
      , FALSE ) ;

   END ;
   /

   CREATE OR REPLACE FUNCTION SAD.bas_dml_lookup_pkg#bas_dml_lookup_pkg#func_name
   RETURN VARCHAR2
   PACKAGE
   IS
        MIG_PV_VAL_DUMMY_G_PKG_NAME VARCHAR2(30) := MIG_ORA_EXT.MIG_FN_GET_PKG_VARIABLE( 'SAD' ,'bas_dml_lookup_pkg' ,'g_pkg_name' )::VARCHAR2(30);
        MIG_PV_VAL_DUMMY_G_FUNC_NAME VARCHAR2(100) := MIG_ORA_EXT.MIG_FN_GET_PKG_VARIABLE( 'SAD' ,'bas_dml_lookup_pkg' ,'g_func_name' )::VARCHAR2(100);
        l_func_name VARCHAR2 ( 100 ) ;

   BEGIN
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
   BEGIN
             ...
   END ;
   /

   GRANT EXECUTE ON FUNCTION SAD.bas_dml_lookup_pkg#bas_dml_lookup_pkg#func_name() TO eip_sad;
   GRANT EXECUTE ON FUNCTION SAD.bas_dml_lookup_pkg#data_change_logs(VARCHAR2, VARCHAR2) TO eip_sad;
