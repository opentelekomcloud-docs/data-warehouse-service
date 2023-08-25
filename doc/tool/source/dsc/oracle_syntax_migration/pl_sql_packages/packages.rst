:original_name: dws_mt_0158.html

.. _dws_mt_0158:

Packages
========

Packages are schema objects that group logically related PL/SQL types, variables, functions and procedures. In Oracle, each package consists of two parts: package specification and package body. The package specification may contain variables and REF CURSOR in variables. The package REF CURSORs are identified and migrated in the referred places. The functions and procedures in the package body are migrated into individual functions and procedures. The types and variables in the package body are migrated to each of the functions and procedures.

If the schema names of the package specification and package body do not match, then the tool will log a schema name mismatch error to the **DSCError.log** file.


.. figure:: /_static/images/en-us_image_0000001233800845.png
   :alt: **Figure 1** Migration of PL/SQL packages

   **Figure 1** Migration of PL/SQL packages

**Input - PL/SQL Packages** (Package specifications and body)

.. code-block::

   CREATE or REPLACE PACKAGE BODY pkg_get_empdet
   IS
    PROCEDURE get_ename(eno in number,ename out varchar2)
    IS
    BEGIN
     SELECT ename || ',' || last_name
      INTO ename
        FROM emp
        WHERE empno = eno;

     END  get_ename;

     FUNCTION get_sal(eno in number) return number
     IS
     lsalary number;
     BEGIN
      SELECT salary
       INTO lsalary
          FROM emp
          WHERE empno = eno;
       RETURN lsalary;

      END get_sal;
     END  pkg_get_empdet;
    /

**Output** (The output contains separate functions and procedures for each of the functions and procedures in the package body of the input)

.. code-block::

   CREATE
        or REPLACE PROCEDURE
   pkg_get_empdet.get_ename ( eno in number ,ename out varchar2 ) IS
        BEGIN

   SELECT

   ename || ',' || last_name INTO ename
             FROM

   emp
             WHERE

   empno = eno ;
             END ;
             /

   CREATE
        or REPLACE FUNCTION
   pkg_get_empdet.get_sal ( eno in number )
       return number IS lsalary number ;
   BEGIN

   SELECT

   salary INTO lsalary

   FROM

   emp

   WHERE

   empno = eno ;

   RETURN lsalary ;
             END ;
             /

**Input - PL/SQL Packages**

.. code-block::

   CREATE OR replace VIEW vw_emp_name AS
             Select pkg_get_empdet.get_sal(emp.empno) as empsal from emp;

**Output**

.. code-block::

   CREATE
   OR replace VIEW vw_emp_name AS (SELECT

   pkg_get_empdet.get_sal (emp.empno) AS empsal
        FROM
             emp)
   ;

   output:
   set
   package_name_list = 'func' ;
   CREATE
   OR REPLACE FUNCTION func1 ( i1 INT )
   RETURN INT As TYPE r_rthpagat_list IS RECORD ( /* Record
   information about cross-border RMB */
   business parameters ( rthpagat ) rthpagat_REQUESTID
   RMTS_REMITTANCE_PARAM.REQUESTID%TYPE ,rthpagat_PARAMTNAME
   RMTS_REMITTANCE_PARAM.PARAMTNAME%TYPE ,rthpagat_PARAMNUM
   RMTS_REMITTANCE_PARAM.PARAMNUM%TYPE ,rthpagat_PARAMSTAT
   RMTS_REMITTANCE_PARAM.PARAMSTAT%TYPE ,rthpagat_REQTELLERNO RMTS_REMITTANCE_PARAM.REQTELLERNO%TYPE
   ,rthpagat_REQUESTTIME RMTS_REMITTANCE_PARAM.REQUESTTIME%TYPE
   ,rthpagat_HOSTERRNO RMTS_REMITTANCE_PARAM.HOSTERRNO%TYPE ,rthpagat_HOSTERRMSG
   RMTS_REMITTANCE_PARAM.HOSTERRMSG%TYPE ,rthpagat_GATBANK
   RMTS_REMITTANCE_PARAM.VALUE1%TYPE ,rthpagat_GATEEBANK
   RMTS_REMITTANCE_PARAM.VALUE2%TYPE ,rthpagat_TELLER
   RMTS_REMITTANCE_PARAM.VALUE3%TYPE ,rthpagat_DATE
   RMTS_REMITTANCE_PARAM.VALUE4%TYPE ,rthpagat_BM_GATBANK
   RMTS_REMITTANCE_PARAM.VALUE5%TYPE ,rthpagat_BM_GATEEBANK
   RMTS_REMITTANCE_PARAM.VALUE6%TYPE ,rthpagat_BM_LMTEL
   RMTS_REMITTANCE_PARAM.VALUE7%TYPE ,rthpagat_BM_LMDAT
   RMTS_REMITTANCE_PARAM.VALUE8%TYPE ) ;
   v1 r_rthpagat_list ;
   BEGIN
             END ;
             /
             reset
   package_name_list ;

**Input -Function/Procedure With No Parameter**

In case a procedure or function does not have any parameter or argument, put () after procedure or function name while calling the same procedure or function.

.. code-block::

   CREATE OR REPLACE PACKAGE BODY SAD.bas_lookup_misc_pkg IS

     g_pkg_name CONSTANT VARCHAR2(30) := 'bas_lookup_misc_pkg';
     g_func_name VARCHAR2(30);


     FUNCTION func_name
     RETURN VARCHAR2
      IS
       l_func_name VARCHAR2(100);
     BEGIN
       l_func_name := g_pkg_name || '.' || g_func_name;
       RETURN l_func_name;
     END func_name;
     ------------------------------------------------------------------------------
     PROCEDURE insert_fnd_data_change_logs(pi_table_name               IN VARCHAR2,
                                           pi_table_key_columns        IN VARCHAR2,
                                           pi_table_key_values         IN VARCHAR2,
                                           pi_column_name              IN VARCHAR2,
                                           pi_column_change_from_value IN VARCHAR2,
                                           pi_column_change_to_value   IN VARCHAR2,
                                           pi_op_code                  IN NUMBER,
                                           pi_description              IN VARCHAR2,
                                           po_error_msg                OUT VARCHAR2)
     IS

     BEGIN
       g_func_name := 'insert_fnd_data_change_logs_t';

     EXCEPTION
       WHEN OTHERS THEN
         po_error_msg := 'Others Exception raise in ' || func_name || ',' || SQLERRM;

     END insert_fnd_data_change_logs;
    END SAD.bas_lookup_misc_pkg;
   /

**Output**

.. code-block::

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

   -------------------------------------------------------------------------------------------------
   CREATE
        OR REPLACE PROCEDURE SAD.bas_lookup_misc_pkg#insert_fnd_data_change_logs ( pi_table_name IN VARCHAR2
        ,pi_table_key_columns IN VARCHAR2
        ,pi_table_key_values IN VARCHAR2
        ,pi_column_name IN VARCHAR2
        ,pi_column_change_from_value IN VARCHAR2
        ,pi_column_change_to_value IN VARCHAR2
        ,pi_op_code IN NUMBER
        ,pi_description IN VARCHAR2
        ,po_error_msg OUT VARCHAR2 )
     PACKAGE
     IS

        MIG_PV_VAL_DUMMY_G_FUNC_NAME VARCHAR2 ( 30 ) := MIG_ORA_EXT.MIG_FN_GET_PKG_VARIABLE ( 'SAD' ,'bas_lookup_misc_pkg' ,'g_func_name' ) ::VARCHAR2 ( 30 ) ;

   BEGIN
        MIG_PV_VAL_DUMMY_G_FUNC_NAME := 'insert_fnd_data_change_logs_t' ;

     MIG_ORA_EXT.MIG_FN_SET_PKG_VARIABLE ( 'SAD','bas_lookup_misc_pkg','g_pkg_name',MIG_PV_VAL_DUMMY_G_PKG_NAME ) ;
     MIG_ORA_EXT.MIG_FN_SET_PKG_VARIABLE ( 'SAD','bas_lookup_misc_pkg','g_func_name',MIG_PV_VAL_DUMMY_G_FUNC_NAME ) ;


             EXCEPTION
                  WHEN OTHERS THEN
                  po_error_msg := 'Others Exception raise in ' || SAD.bas_lookup_misc_pkg#func_name() || ',' || SQLERRM ;

   END ;
   /

**Input - Package Body with no procedure and functions**

In case package body does not have any logic,for example, procedures and functions, DSC needs to remove all code from the same package. The output is basically blank.

.. code-block::

   CREATE OR REPLACE PACKAGE BODY SAD.bas_subtype_pkg IS
   BEGIN
     NULL;
   END bas_subtype_pkg;
   /

**Input - SUBTYPE**

With the SUBTYPE statement, PL/SQL allows you to define your own subtypes or aliases of predefined datatypes, sometimes referred to as abstract datatypes.

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

   "SAD"."BAS_SUBTYPE_PKG" package will be blank after migration.
   --**********************************************************

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

   As the SUBTYPE not supported in GaussDB, the SUBTYPE variable used needs to be replaced with the actualy type.

**Input - sys.dbms_job**

The DBMS_JOB package schedules and manages jobs in the job queue.

.. code-block::

   CREATE OR replace PACKAGE BODY "SAD"."EIP_HTM_INTEGRATION_PKG"
   IS
     PROCEDURE Greate_import_instruction_job
     IS
       v_jobid NUMBER;
     BEGIN
         IF
   bas_lookup_misc_pkg.Exits_run_job('eip_htm_integration_pkg.import_instruction_job') = 'N' THEN
     sys.dbms_job.Submit(job => v_jobid,
                                                         what => 'begin
                                                                               eip_htm_integration_pkg.import_instruction_job;
                                                                             end;',
                                                         next_date => SYSDATE);

     COMMIT;
   END IF;
   ---
   END greate_import_instruction_job;
   END eip_htm_integration_pkg;

**Output**

.. code-block::

   CREATE OR replace PROCEDURE
   sad.Eip_htm_integration_pkg#greate_import_instruction_job
   IS
     v_jobid NUMBER;
   BEGIN
       IF Bas_lookup_misc_pkg#exits_run_job (
             'eip_htm_integration_pkg.import_instruction_job') = 'N' THEN
        dbms_job.Submit(job => v_jobid,
                                                         what => 'begin
                                                                               eip_htm_integration_pkg.import_instruction_job;
                                                                             end;',
                                                         next_date => SYSDATE);
         /*  COMMIT;  */
         NULL;
       END IF;
   ---
   END;

.. note::

   Remove the SYS schema while calling the package.

**Input - Procedure/Function variable**

The NULL constraint is not supported on variable declaration by Gauss, so it is recomended to comment the NULL keyword.

.. code-block::

   CREATE OR REPLACE PACKAGE BODY SAD.sad_lookup_contract_pkg IS
    FUNCTION CONTRACT_DISTRIBUTE_STATUS_S2(PI_CONTRACT_NUMBER IN VARCHAR2)
       RETURN VARCHAR2 IS
       L_CONTRACT_DISTRIBUTE_STATUS VARCHAR2(10)  NULL;

     BEGIN
          IF CUR_CONTRACT.CONTRACT_STATUS = 0 THEN
           L_CONTRACT_DISTRIBUTE_STATUS := 'Cancel';
         ELSE
           L_CONTRACT_DISTRIBUTE_STATUS := 'Active';
         END IF;

       RETURN L_CONTRACT_DISTRIBUTE_STATUS;

     EXCEPTION
       WHEN OTHERS THEN
         L_CONTRACT_DISTRIBUTE_STATUS := NULL;

     END CONTRACT_DISTRIBUTE_STATUS_S2;
   END sad_lookup_contract_pkg;
   /

**Output**

.. code-block::

   CREATE OR replace FUNCTION sad_lookup_contract_pkg.Contract_distribute_status_s2 ( pi_contract_number IN VARCHAR2 )
     RETURN VARCHAR2
   IS
     l_contract_distribute_statusvarchar2 ( 10 )
     /* NULL */
     ;
   BEGIN
     IF cur_contract.contract_status = 0 THEN
       l_contract_distribute_status := 'Cancel' ;
     ELSE
       l_contract_distribute_status := 'Active' ;
     END IF ;
     RETURN l_contract_distribute_status ;
   EXCEPTION
   WHEN OTHERS THEN
     l_contract_distribute_status := NULL ;
   END ;/

**Input - Configuration parameter addPackageNameList = true**

Hint to access objects from specific schema by system.

.. code-block::

   CREATE OR REPLACE PACKAGE BODY IC_STAGE.PKG_REVN_ARPU
   AS
   -----------
   -----------
   END PKG_REVN_ARPU;
   /

**Output**

.. code-block::

   SET  package_name_list = 'PKG_REVN_ARPU' ;
   --------------
   --------------
   reset package_name_list ;

**Input -** **Configuration parameter addPackageNameList = false**

Hint to access objects from specific schema by system.

.. code-block::

   CREATE OR REPLACE PACKAGE BODY IC_STAGE.PKG_REVN_ARPU
   AS
   -----------
   -----------
   END PKG_REVN_ARPU;
   /

**Output**

.. code-block::

   SET SEARCH_PATH=PKG_REVN_ARPU,PUBLIC;

**Input -PACKAGE**

Hint that procedure and functions belongs to a package.

.. code-block::

   CREATE OR REPLACE PACKAGE BODY SAD.sad_lookup_contract_pkg
   IS
    FUNCTION CONTRACT_DISTRIBUTE_STATUS_S2(PI_CONTRACT_NUMBER IN VARCHAR2)
       RETURN VARCHAR2 IS
       L_CONTRACT_DISTRIBUTE_STATUS VARCHAR2(10) ;

     BEGIN
          IF CUR_CONTRACT.CONTRACT_STATUS = 0 THEN
           L_CONTRACT_DISTRIBUTE_STATUS := 'Cancel';
         ELSE
           L_CONTRACT_DISTRIBUTE_STATUS := 'Active';
         END IF;

       RETURN L_CONTRACT_DISTRIBUTE_STATUS;

     EXCEPTION
       WHEN OTHERS THEN
         L_CONTRACT_DISTRIBUTE_STATUS := NULL;

     END CONTRACT_DISTRIBUTE_STATUS_S2;
   END sad_lookup_contract_pkg;
   /

**Output**

.. code-block::

   CREATE OR replace FUNCTION sad_lookup_contract_pkg.Contract_distribute_status_s2 ( pi_contract_number IN VARCHAR2 )
     RETURN VARCHAR2
   PACKAGE
   IS
     l_contract_distribute_statusvarchar2 ( 10 ) ;
   BEGIN
     IF cur_contract.contract_status = 0 THEN
       l_contract_distribute_status := 'Cancel' ;
     ELSE
       l_contract_distribute_status := 'Active' ;
     END IF ;
     RETURN l_contract_distribute_status ;
   EXCEPTION
   WHEN OTHERS THEN
     l_contract_distribute_status := NULL ;
   END ;
   /

.. note::

   You need to put the PACKAGE keyword while creating any procedure and function in front of the IS/AS statement.

**Input -Nested Procedure**

Creating a procedure inside a procedure is known as a nested procedure. The nested procedure is private and belongs to the parent procedure.

.. code-block::

   CREATE OR REPLACE PROCEDURE refresh_sw_product_amount(pi_stage_id IN NUMBER)
     IS

       v_product_amount          sad_sw_product_amount_t.product_amount%TYPE;
    FUNCTION get_sw_no
    RETURN VARCHAR2
     IS
         v_xh       NUMBER;
       BEGIN
         BEGIN
           SELECT nvl(to_number(substrb(MAX(sw_no), 3, 4)), 0)
             INTO v_xh
             FROM sad.sad_sw_product_amount_t
            WHERE pi_stage_id = pi_stage_id;
         EXCEPTION WHEN OTHERS THEN
           v_xh := 0;
         END;

         RETURN 'SW' || lpad(to_char(v_xh + 1), 4, '0') || 'Y';
       END get_sw_no;

       BEGIN

         FOR rec_pu IN (SELECT t.*, sh.header_id
                          FROM asms.ht_stages t, asms.ht, sad.sad_distribution_headers_t sh
                         WHERE t.hth = ht.hth
                           AND sh.contract_number = t.hth
                           AND sh.stage_id = t.stage_id
                           AND ht.sw_track_flag = 'Y'
                           AND to_char(t.category_id) IN
                                       (SELECT code
                                          FROM asms.asms_lookup_values
                                         WHERE type_code = 'CATEGORY_ID_EQUIPMENT'
                                           AND enabled_flag = 'Y')
                           AND nvl(t.status, '-1') <> '0'
                           AND t.stage_id = pi_stage_id)

         LOOP
           SELECT nvl(SUM(nvl(product_amount, 0)), 0)
             INTO v_product_amount
             FROM sad.sad_products_t sp
            WHERE sp.header_id = rec_pu.header_id
              AND sp.sw_flag = 'Y';


         END LOOP;

   END refresh_sw_product_amount;

**Output**

.. code-block::

   CREATE OR REPLACE FUNCTION get_sw_no(pi_stage_id IN NUMBER)
    RETURN VARCHAR2 IS
         v_xh       NUMBER;
       BEGIN
         BEGIN
           SELECT nvl(to_number(substrb(MAX(sw_no), 3, 4)), 0)
             INTO v_xh
             FROM sad.sad_sw_product_amount_t
            WHERE pi_stage_id = pi_stage_id;
         EXCEPTION WHEN OTHERS THEN
           v_xh := 0;
         END;

         RETURN 'SW' || lpad(to_char(v_xh + 1), 4, '0') || 'Y';
   END ;
   /

    --*****************************************************************************
    CREATE OR REPLACE PROCEDURE refresh_sw_product_amount(pi_stage_id IN NUMBER)
    IS

       v_product_amount          sad_sw_product_amount_t.product_amount%TYPE;

       BEGIN

         FOR rec_pu IN (SELECT t.*, sh.header_id
                          FROM asms.ht_stages t, asms.ht, sad.sad_distribution_headers_t sh
                         WHERE t.hth = ht.hth
                           AND sh.contract_number = t.hth
                           AND sh.stage_id = t.stage_id
                           AND ht.sw_track_flag = 'Y'
                           AND to_char(t.category_id) IN
                                       (SELECT code
                                          FROM asms.asms_lookup_values
                                         WHERE type_code = 'CATEGORY_ID_EQUIPMENT'
                                           AND enabled_flag = 'Y')
                           AND nvl(t.status, '-1') <> '0'
                           AND t.stage_id = pi_stage_id)

         LOOP
           SELECT nvl(SUM(nvl(product_amount, 0)), 0)
             INTO v_product_amount
             FROM sad.sad_products_t sp
            WHERE sp.header_id = rec_pu.header_id
              AND sp.sw_flag = 'Y';


         END LOOP;

   END;
   /

.. note::

   When nested procedures/functions are implemented, the package variables in all procedures/functions must be processed.

   After migrating sub-procedures/functions, migrate the parent procedure/function.

**if pkgSchemaNaming = false**

if **pkgSchemaNaming** is set to **false**, PL RECORD migration should not have package name in the type name as its schema.

**Input**

.. code-block::

   CREATE OR REPLACE PACKAGE BODY SAD.sad_dml_product_pkg IS

    PROCEDURE save_sad_product_line_amount(pi_stage_id            IN NUMBER,
                pi_product_line_code   IN VARCHAR2,
                po_error_msg           OUT VARCHAR2) IS

      TYPE t_line IS RECORD(
     product_line   VARCHAR2(30),
     product_amount NUMBER);
      TYPE tab_line IS TABLE OF t_line INDEX BY BINARY_INTEGER;
      rec_line             tab_line;
      v_product_line_arr   VARCHAR2(5000);
      v_product_line       VARCHAR2(30) ;
      v_count              INTEGER;
      v_start              INTEGER;
      v_pos                INTEGER;

    BEGIN
      v_count      := 0;
      v_start      := 1;

       v_product_line_arr := pi_product_line_code;
     LOOP
       v_pos := instr(v_product_line_arr, ',', v_start);
       IF v_pos <= 0
       THEN
      EXIT;
       END IF;
       v_product_line := substr(v_product_line_arr, v_start, v_pos - 1);
       v_count := v_count + 1;
       rec_line(v_count).product_line := v_product_line;
       rec_line(v_count).product_amount := 0;
       v_product_line_arr := substr(v_product_line_arr, v_pos + 1, length(v_product_line_arr));

     END LOOP;

      FOR v_count IN 1 .. rec_line.count
      LOOP
     UPDATE sad_product_line_amount_t spl
        SET spl.product_line_amount = rec_line(v_count).product_amount
      WHERE spl.stage_id = pi_stage_id
        AND spl.product_line_code = rec_line(v_count).product_line;
     IF SQL%NOTFOUND
     THEN
       INSERT INTO sad_product_line_amount_t
           (stage_id, product_line_code, product_line_amount)
       VALUES (pi_stage_id, rec_line(v_count).product_line, rec_line(v_count).product_amount);
     END IF;
      END LOOP;

    EXCEPTION
      WHEN OTHERS THEN
     po_error_msg := 'Others Exception raise in ' || func_name || ',' || SQLERRM;
    END save_sad_product_line_amount;

   END sad_dml_product_pkg;
   /

**Output**

.. code-block::

   CREATE  TYPE SAD.sad_dml_product_pkg#t_line AS
    ( product_line VARCHAR2 ( 30 )
       , product_amount NUMBER ) ;

   CREATE OR REPLACE PROCEDURE SAD.sad_dml_product_pkg#save_sad_product_line_amount
    ( pi_stage_id IN NUMBER
       , pi_product_line_code IN VARCHAR2
       , po_error_msg OUT VARCHAR2 )
   PACKAGE
   IS
    TYPE tab_line IS VARRAY ( 10240 ) OF SAD.sad_dml_product_pkg#t_line ;
        rec_line tab_line ;
        v_product_line_arr VARCHAR2 ( 5000 ) ;
        v_product_line VARCHAR2 ( 30 ) ;
        v_count INTEGER ;
        v_start INTEGER ;
        v_pos INTEGER ;
   BEGIN
        v_count := 0 ;
        v_start := 1 ;
        v_product_line_arr := pi_product_line_code ;

        LOOP
            v_pos := instr( v_product_line_arr ,',' ,v_start ) ;

      IF v_pos <= 0 THEN
          EXIT ;
      END IF ;

      v_product_line := SUBSTR( v_product_line_arr ,v_start ,v_pos - 1 ) ;
      v_count := v_count + 1 ;
      rec_line ( v_count ).product_line := v_product_line ;
      rec_line ( v_count ).product_amount := 0 ;
      v_product_line_arr := SUBSTR( v_product_line_arr ,v_pos + 1 ,length( v_product_line_arr ) ) ;

        END LOOP ;

        FOR v_count IN 1.. rec_line.count
     LOOP
             UPDATE sad_product_line_amount_t spl
                SET spl.product_line_amount = rec_line ( v_count ).product_amount
              WHERE spl.stage_id = pi_stage_id
                AND spl.product_line_code = rec_line ( v_count ).product_line ;

      IF SQL%NOTFOUND THEN
      INSERT INTO sad_product_line_amount_t
          ( stage_id, product_line_code, product_line_amount )
      VALUES ( pi_stage_id, rec_line ( v_count ).product_line
          , rec_line ( v_count ).product_amount ) ;

      END IF ;

        END LOOP ;

   EXCEPTION
       WHEN OTHERS THEN
           po_error_msg := 'Others Exception raise in ' || func_name || ',' || SQLERRM ;

   END ;
   /
