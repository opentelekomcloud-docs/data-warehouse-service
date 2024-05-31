:original_name: dws_mt_0155.html

.. _dws_mt_0155:

PL/SQL Collections (Using User-Defined Types)
=============================================

This section descripes the migration syntax of Oracle PL/SQL Collections. The migration syntax decides how the keywords/features are migrated.

A user-defined type (UDT) is a data type that is derived from a supported data type.

UDT uses built-in datatypes and other user-defined datatypes as the building blocks for datatypes that model the structure and behavior of data in applications. UDT makes it easier to work with PL/SQL collections.

UDT Table
---------

The table type is created to track the structure of the UDT. No data will be stored in the table.

**Input - CREATE TABLE TYPE**

::

   CREATE <OR REPLACE> TYPE <schema.>inst_no_type IS TABLE OF VARCHAR2 (32767);

**Output**

::

   CREATE TABLE<schema.>mig_inst_no_type
          ( typ_col VARCHAR2 (32767) );

UDT VArray
----------

**Input - CREATE VArray**

::

   CREATE TYPE phone_list_typ_demo AS VARRAY(n) OF VARCHAR2(25);

**Output**

::

   CREATE TABLE mig_pone_list_typ_demo
            ( typ_col VARCHAR2 (25) );

**Declare UDT**

**Input - Declare UDT**

::

   DECLARE
          v_SQL_txt_array
           inst_no_type <:=
           inst_no_type()>;
   BEGIN
          …

**Output**

::

   DECLARE
   /*       v_SQL_txt_array inst_no_type <:= inst_no_type()>; */
   BEGIN
            EXECUTE IMMEDIATE 'DROP TABLE IF EXISTS
             v_SQL_txt_array;
                 CREATE LOCAL TEMPORARY TABLE
                  v_SQL_txt_array
                      ON COMMIT PRESERVE ROWS
                      AS SELECT *, CAST(NULL AS INT) AS
                       typ_idx_col
                          FROM mig_inst_no_type
                         WHERE FALSE';
          …

UDT Count
---------

**Input - UDT - COUNT in FOR LOOP**

::

   BEGIN
       ...
      FOR i IN 1..v_jobnum_list.COUNT
      LOOP
         SELECT COUNT(*) INTO v_abc
            FROM ...
           WHERE ...
             AND nvl(t.batch_num,
              c_batchnum_null_num) =
              v_jobnum_list(i);
         ...
      END LOOP;
      ...

**Output**

.. code-block::

   BEGIN
       ...
       FOR i IN 1..(SELECT COUNT(*) from v_jobnum_list)
       LOOP
         SELECT COUNT(*) INTO v_abc
            FROM ...
           WHERE ...
             AND nvl(t.batch_num, c_batchnum_null_num) =
             (SELECT typ_col FROM v_jobnum_list
                   WHERE typ_idx_col = i);
         ...
      END LOOP;
      ...

UDT Record
----------

A Record type is used to create records and can be defined in the declarative part of any PL/SQL block, subprogram, or package.

**Input - RECORD Type**

::

   Create
        or Replace Procedure test_proc AS TYPE t_log IS RECORD ( col1 int ,col2 emp.ename % type ) ;
        fr_wh_SQL t_log ;
   BEGIN
        fr_wh_SQL.col1 := 101 ;
        fr_wh_SQL.col2 := 'abcd' ;
   DBMS_OUTPUT.PUT_LINE ( fr_wh_SQL.col1 || ',' || fr_wh_SQL.col2 ) ;
   END test_proc;
   /

**Output**

::

   Create
        or Replace Procedure test_proc AS /*TYPE t_log IS RECORD ( col1 int,col2 emp.ename%type );*/
        fr_wh_SQL RECORD ;
        MIG_t_log_col1 int ;
   MIG_t_log_col2 emp.ename % type ;
   BEGIN
   select
             MIG_t_log_col1 as col1 ,MIG_t_log_col2 as col2 INTO FR_WH_SQL ;
             fr_wh_SQL.col1 := 101 ;
        fr_wh_SQL.col2 := 'abcd' ;
   DBMS_OUTPUT.PUT_LINE ( fr_wh_SQL.col1 || ',' || fr_wh_SQL.col2 ) ;
   END ;
   /

Enhancement of User-defined types
---------------------------------

The tool supports the enhancement of PL/SQL type of TABLE used in Oracle for specific data types and for any table column.

**Input - PL/SQL type of TABLE of a specific data-type**

::

   DECLARE
     type       fr_wh_SQL_info_type is table of VARCHAR(10);
    fr_wh_SQL  fr_wh_SQL_info_type   [:= fr_wh_SQL_info_type()];
   BEGIN
          …

**Output**

::

   DECLARE
   /*      type   fr_wh_SQL_info_type    is table of varchar(10); */
   /*      fr_wh_SQL    fr_wh_SQL_info_type    [:= fr_wh_SQL_info_type()]; */
   BEGIN
        EXECUTE IMMEDIATE 'DROP TABLE IF EXISTS mig_fr_wh_SQL_info_type;
             CREATE LOCAL TEMPORARY TABLE mig_fr_wh_SQL_info_type
                   ( typ_col VARCHAR (10) )
                 ON COMMIT PRESERVE ROWS' ;

         EXECUTE IMMEDIATE 'DROP TABLE IF EXISTS fr_wh_SQL;
                   CREATE LOCAL TEMPORARY TABLE fr_wh_SQL
                        ON COMMIT PRESERVE ROWS AS
                        AS SELECT  *, CAST(NULL AS INT) AS typ_idx_col
                             FROM mig_fr_wh_SQL_info_type
                            WHERE FALSE';
                  …

**Input - PL/SQL type of TABLE of any table's column**

::

   DECLARE
         type         fr_wh_SQL_info_type    is table of fr_wh_SQL_info.col1%type;
         fr_wh_SQL   fr_wh_SQL_info_type    [:= fr_wh_SQL_info_type()];
   BEGIN
          …

**Output**

::

   DECLARE
   /*      type          fr_wh_SQL_info_type    is table of fr_wh_SQL_info.col1%type; */
   /*      fr_wh_SQL   fr_wh_SQL_info_type    [:= fr_wh_SQL_info_type()]; */
   BEGIN
        EXECUTE IMMEDIATE 'DROP TABLE IF EXISTS mig_fr_wh_SQL_info_type;
              CREATE LOCAL TEMPORARY TABLE mig_fr_wh_SQL_info_type
                  ON COMMIT PRESERVE ROWS
                  AS SELECT col1 AS typ_col
                        FROM fr_wh_SQL_info
                       WHERE FALSE' ;

         EXECUTE IMMEDIATE 'DROP TABLE IF EXISTS fr_wh_SQL;
              CREATE LOCAL TEMPORARY TABLE fr_wh_SQL
                  ON COMMIT PRESERVE ROWS AS
                  AS SELECT  *, CAST(NULL AS INT) AS typ_idx_col
                       FROM mig_fr_wh_SQL_info_type
                      WHERE FALSE';
   …

EXTEND
------

GaussDB(DWS) supports keyword **EXTEND**.

**Input - Extend**

.. code-block::

   FUNCTION FUNC_EXTEND ( in_str  IN   VARCHAR2)
         RETURN ARRYTYPE
      AS
         v_count2    INTEGER;
         v_strlist   arrytype;
         v_node      VARCHAR2 (2000);
      BEGIN
         v_count2 := 0;
         v_strlist := arrytype ();
        FOR v_i IN 1 .. LENGTH (in_str)
         LOOP
           IF v_node IS NULL
              THEN
                  v_node := '';
             END IF;

            IF (v_count2 = 0) OR (v_count2 IS NULL)
            THEN
               EXIT;
            ELSE
               v_strlist.EXTEND ();
               v_strlist (v_i) := v_node;
               v_node := '';
            END IF;
         END LOOP;

         RETURN v_strlist;
      END;
      /

**Output**

.. code-block::

   FUNCTION FUNC_EXTEND ( in_str IN VARCHAR2 )
   RETURN ARRYTYPE AS v_count2 INTEGER ;
   v_strlist arrytype ;
   v_node VARCHAR2 ( 2000 ) ;
   BEGIN
             v_count2 := 0 ;
             v_strlist := arrytype ( ) ;
        FOR v_i IN 1.. LENGTH( in_str ) LOOP
             IF
                  v_node IS NULL
                  THEN
                       v_node := '' ;
                  END IF ;
                  IF
                       ( v_count2 = 0 )
                       OR( v_count2 IS NULL )
                       THEN
                            EXIT ;
                       ELSE
                            v_strlist.EXTEND ( 1 ) ;
                            v_strlist ( v_i ) := v_node ;
                            v_node := '' ;
                       END IF ;
                  END LOOP ;
             RETURN v_strlist ;
        END ;
        /

RECORD
------

The Record type declared in the package specification is actually used in the corresponding package body.

After **plsqlCollection** is set to varray, UDT will be migrated as VARRY.

**Input - RECORD**

.. code-block::

   CREATE OR REPLACE FUNCTION func1 (i1 INT)
   RETURN INT
   As
   TYPE r_rthpagat_list IS RECORD (--Record information about cross-border RMB business parameters (rthpagat)
   rthpagat_REQUESTID RMTS_REMITTANCE_PARAM.REQUESTID%TYPE ,rthpagat_PARAMTNAME RMTS_REMITTANCE_PARAM.PARAMTNAME%TYPE ,rthpagat_PARAMNUM RMTS_REMITTANCE_PARAM.PARAMNUM%TYPE ,rthpagat_PARAMSTAT RMTS_REMITTANCE_PARAM.PARAMSTAT%TYPE ,rthpagat_REQTELLERNO RMTS_REMITTANCE_PARAM.REQTELLERNO%TYPE ,rthpagat_REQUESTTIME RMTS_REMITTANCE_PARAM.REQUESTTIME%TYPE ,rthpagat_HOSTERRNO RMTS_REMITTANCE_PARAM.HOSTERRNO%TYPE ,rthpagat_HOSTERRMSG RMTS_REMITTANCE_PARAM.HOSTERRMSG%TYPE ,rthpagat_GATBANK RMTS_REMITTANCE_PARAM.VALUE1%TYPE ,rthpagat_GATEEBANK RMTS_REMITTANCE_PARAM.VALUE2%TYPE ,rthpagat_TELLER RMTS_REMITTANCE_PARAM.VALUE3%TYPE ,rthpagat_DATE RMTS_REMITTANCE_PARAM.VALUE4%TYPE ,rthpagat_BM_GATBANK RMTS_REMITTANCE_PARAM.VALUE5%TYPE ,rthpagat_BM_GATEEBANK RMTS_REMITTANCE_PARAM.VALUE6%TYPE ,rthpagat_BM_LMTEL RMTS_REMITTANCE_PARAM.VALUE7%TYPE ,rthpagat_BM_LMDAT RMTS_REMITTANCE_PARAM.VALUE8%TYPE ) ;

     v1  r_rthpagat_list;
   BEGIN

   END;
   /

**Output**

.. code-block::

   CREATE
   TYPE rmts_remitparammgmt_rthpagat.r_rthpagat_list AS (/* O_ERRMSG error description */
   Rthpagat_REQUESTID
        rthpagat_REQUESTID RMTS_REMITTANCE_PARAM.REQUESTID%TYPE ,rthpagat_PARAMTNAME RMTS_REMITTANCE_PARAM.PARAMTNAME%TYPE ,rthpagat_PARAMNUM RMTS_REMITTANCE_PARAM.PARAMNUM%TYPE ,rthpagat_PARAMSTAT RMTS_REMITTANCE_PARAM.PARAMSTAT%TYPE ,rthpagat_REQTELLERNO RMTS_REMITTANCE_PARAM.REQTELLERNO%TYPE ,rthpagat_REQUESTTIME RMTS_REMITTANCE_PARAM.REQUESTTIME%TYPE ,rthpagat_HOSTERRNO RMTS_REMITTANCE_PARAM.HOSTERRNO%TYPE ,rthpagat_HOSTERRMSG RMTS_REMITTANCE_PARAM.HOSTERRMSG%TYPE ,rthpagat_GATBANK RMTS_REMITTANCE_PARAM.VALUE1%TYPE ,rthpagat_GATEEBANK RMTS_REMITTANCE_PARAM.VALUE2%TYPE ,rthpagat_TELLER RMTS_REMITTANCE_PARAM.VALUE3%TYPE ,rthpagat_DATE RMTS_REMITTANCE_PARAM.VALUE4%TYPE ,rthpagat_BM_GATBANK RMTS_REMITTANCE_PARAM.VALUE5%TYPE ,rthpagat_BM_GATEEBANK RMTS_REMITTANCE_PARAM.VALUE6%TYPE ,rthpagat_BM_LMTEL RMTS_REMITTANCE_PARAM.VALUE7%TYPE ,rthpagat_BM_LMDAT RMTS_REMITTANCE_PARAM.VALUE8%TYPE ) ;

   CREATE OR REPLACE FUNCTION func1 (i1 INT)
   RETURN INT
   AS
     v1  r_rthpagat_list;
   BEGIN

   END;
   /

Naming Convention of Type Name
------------------------------

User-defined types allow for the definition of data types that model the structure and behavior of the data in an application.

**Input**

.. code-block::

   CREATE
        TYPE t_line AS ( product_line VARCHAR2 ( 30 )
                                   ,product_amount NUMBER ) ;
   ;

**Output**

.. code-block::

   CREATE
        TYPE sad_dml_product_pkg.t_line AS ( product_line VARCHAR2 ( 30 )
                                                                             ,product_amount NUMBER ) ;

**Input**

.. code-block::

   CREATE
        TYPE t_line AS ( product_line VARCHAR2 ( 30 )
                                   ,product_amount NUMBER ) ;

**Output**

.. code-block::

   CREATE
        TYPE SAD.sad_dml_product_pkg#t_line AS ( product_line VARCHAR2 ( 30 )
                                                                                      ,product_amount NUMBER ) ;

.. note::

   -  For the first output(pkg.t),if **pkgSchemaNaming** is set to **true** in the configuration, PL RECORD migration should have package name as a schema name along with a type name.
   -  For the second output (pkg#t), assume that TYPE belongs to sad_dml_product_pkg package.

   If **pkgSchemaNaming** is set to **false** in the configuration, PL RECORD migration should have schema name as a schema name along with a package name + a type name separated by # as a type name.

SUBTYPE
-------

With the SUBTYPE statement, PL/SQL allows you to define your own subtypes or aliases of predefined datatypes, sometimes referred to as abstract datatypes.

**Input**

.. code-block::

   CREATE OR REPLACE PACKAGE "SAD"."BAS_SUBTYPE_PKG" IS
   SUBTYPE CURRENCY IS BAS_PRICE_LIST_T.CURRENCY%TYPE;
   END bas_subtype_pkg;
   /
   CREATE OR REPLACE PACKAGE BODY "SAD"."BAS_SUBTYPE_PKG" IS
   BEGIN
     NULL;
   END bas_subtype_pkg;
   /
   --********************************************************************
   CREATE OR REPLACE PACKAGE BODY SAD.bas_lookup_misc_pkg IS
     FUNCTION get_currency(pi_price_type IN NUMBER) RETURN VARCHAR2 IS
       v_currency bas_subtype_pkg.currency;
     BEGIN
       g_func_name := 'get_currency';
       FOR rec_currency IN (SELECT currency FROM sad_price_type_v WHERE price_type_code = pi_price_type)
       LOOP
         v_currency := rec_currency.currency;
       END LOOP;
       RETURN v_currency;
     END get_currency;
    END SAD.bas_lookup_misc_pkg;
    /

**Output**

.. code-block::

    CREATE OR REPLACE FUNCTION SAD.bas_lookup_misc_pk#get_currency(pi_price_type IN NUMBER) RETURN VARCHAR2 IS
       v_currency BAS_PRICE_LIST_T.CURRENCY%TYPE;
     BEGIN
       g_func_name := 'get_currency';
       FOR rec_currency IN (SELECT currency FROM sad_price_type_v WHERE price_type_code = pi_price_type)
       LOOP
         v_currency := rec_currency.currency;
       END LOOP;
       RETURN v_currency;
     END ;
    /

.. note::

   As SUBTYPE is not supported in GaussDB, the SUBTYPE variable needs to be replaced with the actual type.
