:original_name: dws_mt_0306.html

.. _dws_mt_0306:

Data Type
=========

**Subtype**

The customized type in the package cannot be converted.

SUBTYPE error_msg IS sad_products_t.exception_description%TYPE;

SUBTYPE AR_FLAG IS SAD_RA_LINES_TI.AR_FLAG%TYPE;

SUBTYPE LOCK_FLAG IS SAD_SHIPMENT_BATCHES_T.LOCK_FLAG%TYPE;

bas_subtype_pkg.error_msg

**Input**:

.. code-block::

   CREATE OR REPLACE PACKAGE SAD.bas_subtype_pkg IS
    SUBTYPE func_name IS sad_products_t.func_name%TYPE;
   END bas_subtype_pkg;
   /
   CREATE OR REPLACE PACKAGE BODY SAD.bas_subtype_pkg IS
   BEGIN
     NULL;
   END bas_subtype_pkg;
   /

**Output**:

.. code-block::

   CREATE OR REPLACE PACKAGE BODY SAD.bas_dml_lookup_pkg IS
     g_pkg_name CONSTANT VARCHAR2(30) := 'bas_dml_ic_price_rule_pkg' ;
     g_func_name VARCHAR2(100);

     FUNCTION func_name
     RETURN VARCHAR2
     IS
       l_func_name bas_subtype_pkg.func_name;;
     BEGIN
        l_func_name := g_pkg_name || '.' || g_func_name ;
        RETURN l_func_name ;

      END func_name;

   END bas_dml_lookup_pkg;
   /

**%ROWTYPE**

The package procdure/function contains %ROWTYPE attribute in IN/OUT parameter and this is not supported

Scripts: BAS_DML_SERVIECE_PKG.sql, BAS_LOOKUP_MISC_PKG.sql

**INPUT**

.. code-block::

   CREATE OR REPLACE PACKAGE BODY "SAD"."BAS_DML_SERVIECE_PKG" IS
   PROCEDURE save_split_ou(pi_split_ou  IN split_ou%ROWTYPE,
   po_error_msg OUT VARCHAR2) IS
   ---
   BEGIN
   ---
   end save_split_ou;
   end BAS_DML_SERVIECE_PKG;

**OUTPUT**

.. code-block::

   CREATE
   OR REPLACE PROCEDURE SAD.BAS_DML_SERVIECE_PKG#save_split_ou ( pi_split_ou IN split_ou%ROWTYPE
   ,po_error_msg OUT VARCHAR2 ) IS MIG_PV_VAL_DUMMY_G_FUNC_NAME VARCHAR2 ( 30 ) := MIG_ORA_EXT.MIG_FN_GET_PKG_VARIABLE ( current_schema ( )
   ,'BAS_DML_SERVIECE_PKG'
   ,'g_func_name' ) ::VARCHAR2 ( 30 ) ;
   ex_data_error
   EXCEPTION ;
   ex_prog_error
   EXCEPTION ;
   ---
   BEGIN
   ---
   END;

**Input**

.. code-block::

   CREATE OR REPLACE PACKAGE BODY SAD.BAS_DML_SERVIECE_PKG IS
     PROCEDURE save_split_ou(pi_split_ou  IN split_ou%ROWTYPE,
                             po_error_msg OUT VARCHAR2) IS
     BEGIN
       UPDATE split_ou so
          SET so.auto_balance_flag      = pi_split_ou.auto_balance_flag,
              so.balance_start_date     = pi_split_ou.balance_start_date,
              so.balance_source         = pi_split_ou.balance_source
        WHERE so.dept_code = pi_split_ou.dept_code;
     EXCEPTION
       WHEN OTHERS THEN
         po_error_msg := 'Others Exception raise in ' || g_func_name || ',' ||
                         SQLERRM;
     END save_split_ou;
   END bas_dml_serviece_pkg;
   /

**Output**

.. code-block::

   CREATE TYPE mig_typ_split_ou AS ...;

   CREATE OR REPLACE PROCEDURE SAD.BAS_DML_SERVIECE_PKG#save_split_ou
    ( pi_split_ou IN mig_typ_split_ou
        ,po_error_msg OUT VARCHAR2 )
   PACKAGE
   IS
   BEGIN
             UPDATE split_ou so
             SET so.auto_balance_flag = pi_split_ou.auto_balance_flag
                  ,so.balance_start_date = pi_split_ou.balance_start_date
                  ,so.balance_source = pi_split_ou.balance_source
             WHERE so.dept_code = pi_split_ou.dept_code ;

   EXCEPTION
       WHEN OTHERS THEN
           po_error_msg := 'Others Exception raise in ' || g_func_name || ',' || SQLERRM ;
   END ;
   /
