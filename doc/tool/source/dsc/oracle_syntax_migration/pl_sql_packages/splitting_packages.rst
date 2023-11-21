:original_name: dws_mt_0301.html

.. _dws_mt_0301:

Splitting Packages
==================

The package specification is migrated as a schema named after the package and the procedures and functions in the package body is migrated as **Packagename.procedurename** and **Packagename.funtionname**.

Migration can be performed after **pkgSchemaNaming** is set to **true**.

**Input - PACKAGE1.FUNC1**

.. code-block::

   CREATE OR REPLACE PACKAGE BODY pack AS
     FUNCTION get_fullname(n_emp_id NUMBER) RETURN VARCHAR2 IS
         v_fullname VARCHAR2(46);
     BEGIN
       SELECT first_name || ',' ||  last_name
       INTO v_fullname
       FROM employees
       WHERE employee_id = n_emp_id;
       RETURN v_fullname;
     END get_fullname;

    PROCEDURE get_salary(n_emp_id NUMBER) RETURN NUMBER IS
       n_salary NUMBER(8,2);
     BEGIN
       SELECT salary
       INTO n_salary
       FROM employees
       WHERE employee_id = n_emp_id;
       END get_salary;
   END pack;
   /

**Output**

.. code-block::

   CREATE
   OR REPLACE FUNCTION pack.get_fullname ( n_emp_id NUMBER )
   RETURN VARCHAR2 IS v_fullname VARCHAR2 ( 46 ) ;
   BEGIN
             SELECT
                       first_name || ',' || last_name INTO v_fullname
                  FROM
                       employees
                  WHERE
                       employee_id = n_emp_id ;
                  RETURN v_fullname ;
             END ;
   /
   CREATE
        OR REPLACE FUNCTION pack.get_salary ( n_emp_id NUMBER )
        RETURN NUMBER IS n_salary NUMBER ( 8 ,2 ) ;
   BEGIN
             SELECT
                       salary INTO n_salary
                  FROM
                       employees
                  WHERE
                       employee_id = n_emp_id ;
                  RETURN n_salary ;
             END ;
             /

**If** **pkgSchemaNaming** **is set to** **false, packages can be split.**

When **bas_lookup_misc_pkg** is calling **insert_fnd_data_change_logs**, **insert_fnd_data_change_logs** will be not migrated.

**Input**

.. code-block::

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

      END ;

     PROCEDURE data_change_logs ( pi_table_name        IN VARCHAR2
                                , pi_table_key_columns IN VARCHAR2
                                , po_error_msg         OUT VARCHAR2
           )
     IS
     BEGIN
       g_func_name := 'insert_fnd_data_change_logs_t';

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

.. code-block::

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
   CREATE OR REPLACE PROCEDURE SAD.bas_dml_lookup_pkg#data_change_logs ( pi_table_name IN VARCHAR2
                    , pi_table_key_columns IN VARCHAR2
                    , po_error_msg OUT VARCHAR2 )
   IS
    MIG_PV_VAL_DUMMY_G_FUNC_NAME VARCHAR2(30) := MIG_ORA_EXT.MIG_FN_GET_PKG_VARIABLE ( 'SAD' ,'BAS_DML_LOOKUP_PKG' ,'G_FUNC_NAME' )::VARCHAR2(30) ;
   BEGIN
        MIG_PV_VAL_DUMMY_G_FUNC_NAME := 'insert_fnd_data_change_logs_t' ;

        INSERT INTO fnd_data_change_logs_t (
             logid,table_name,table_key_columns )
        VALUES ( NEXTVAL ( 'fnd_data_change_logs_t_s' )
               , pi_table_name, pi_table_key_columns ) ;

        MIG_ORA_EXT.MIG_FN_SET_PKG_VARIABLE ( 'SAD', 'BAS_DML_LOOKUP_PKG', 'G_FUNC_NAME', MIG_PV_VAL_DUMMY_G_FUNC_NAME ) ;

        EXCEPTION
           WHEN OTHERS THEN
              po_error_msg := 'Others Exception raise in ' || SAD.bas_dml_lookup_pkg#func_name( ) || ',' || SQLERRM ;
              MIG_ORA_EXT.MIG_FN_SET_PKG_VARIABLE ( 'SAD', 'BAS_DML_LOOKUP_PKG', 'G_FUNC_NAME', MIG_PV_VAL_DUMMY_G_FUNC_NAME ) ;

   END ;
   /

**PACKAGE Keyword**

The kernel needs to add the package tag to the functions and stored procedures converted from the package.

**Input**

.. code-block::

   CREATE OR REPLACE PACKAGE BODY SAD.bas_dml_lookup_pkg IS

     FUNCTION func_name
     RETURN VARCHAR2
     IS
       l_func_name VARCHAR2(100) ;
     BEGIN
        l_func_name := 'bas_dml_lookup_pkg' || '.' || 'func_name' ;
        RETURN l_func_name ;

      END ;

   END bas_dml_lookup_pkg ;
   /

**Output**

.. code-block::

   CREATE OR REPLACE FUNCTION func_name
   RETURN VARCHAR2
   PACKAGE
   IS
     l_func_name VARCHAR2(100) ;
   BEGIN
      l_func_name := 'bas_dml_lookup_pkg' || '.' || 'func_name' ;
      RETURN l_func_name ;

   END ;
   /
