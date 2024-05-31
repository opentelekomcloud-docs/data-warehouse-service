:original_name: dws_mt_0302.html

.. _dws_mt_0302:

.. _en-us_topic_0000001819336305:

REF CURSOR
==========

REF Cursor is a data type that can store the database cursor values and is used to return query results. DSC supports migration of REF CURSOR. The example below shows how the DSC migrates **lref_strong_emptyp** (local REF CURSOR) and ref_strong_emptyp (package-level REF CURSOR).

**Input - REF CURSOR in PL/SQL Package** (Package Specification and Body)

::

   # Package specification
   CREATE OR REPLACE PACKAGE pkg_refcur
   IS
       TYPE ref_variable IS REF CURSOR;
       TYPE ref_strong_emptyp IS REF CURSOR RETURN emp_o%ROWTYPE;
       PROCEDURE p_get_employees ( v_id in INTEGER ,po_results OUT ref_strong_emptyp );

   END pkg_refcur ;
   /

   # Package body
   CREATE OR REPLACE PACKAGE BODY pkg_refcur
   IS
       TYPE lref_strong_emptyp IS REF CURSOR RETURN emp_o%ROWTYPE ;
       var_num NUMBER ;

       PROCEDURE p_get_employees ( v_id IN INTEGER, po_results OUT ref_strong_emptyp )
       is
           vemp_rc lref_strong_emptyp ;
       Begin
           OPEN po_results for
           SELECT * FROM emp_o e
            WHERE e.id = v_id;

       EXCEPTION
           WHEN OTHERS THEN
               RAISE;
       END p_get_employees;
   END pkg_refcur;
   /

**Output**

::

   BEGIN
             INSERT INTO MIG_ORA_EXT.MIG_PKG_VARIABLES ( SCHEMA_NAME ,PACKAGE_NAME ,SPEC_OR_BODY ,VARIABLE_NAME ,VARIABLE_TYPE ,CONSTANT_I ,DEFAULT_VALUE ,EXPRESSION_I )
        VALUES ( UPPER( current_schema ( ) ) ,UPPER( 'pkg_refcur' ) ,'B' ,UPPER( 'var_num' ) ,UPPER( 'NUMBER' ) ,false ,NULL ,false ) ;
   END ;
   /

   CREATE
        OR REPLACE PROCEDURE pkg_refcur#p_get_employees ( v_id IN INTEGER ,po_results OUT SYS_REFCURSOR ) is vemp_rc SYS_REFCURSOR ;
        Begin
             OPEN po_results for SELECT
                       *
                  FROM
                       emp_o e
                  WHERE
                       e.id = v_id ;
                       EXCEPTION WHEN OTHERS
                  THEN RAISE ;
        END ;
        /
