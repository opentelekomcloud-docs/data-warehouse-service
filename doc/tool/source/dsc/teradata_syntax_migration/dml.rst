:original_name: dws_mt_0071.html

.. _dws_mt_0071:

DML
===

This section describes the syntax for migrating Teradata DML. The migration syntax determines how the keywords and features are migrated.

In Teradata, SQL queries in a file that contains the **SELECT**, **INSERT**, **UPDATE**, **DELETE**, or **MERGE** statement can be migrated to GaussDB(DWS).

For details, see the following topics:

:ref:`INSERT <en-us_topic_0000001234200625__en-us_topic_0238518363_en-us_topic_0237362323_section9317194224211>`

:ref:`SELECT <en-us_topic_0000001234200625__en-us_topic_0238518363_en-us_topic_0237362323_section6192558124215>`

:ref:`UPDATE <en-us_topic_0000001234200625__en-us_topic_0238518363_en-us_topic_0237362323_section13878017434>`

:ref:`DELETE <en-us_topic_0000001234200625__en-us_topic_0238518363_en-us_topic_0237362323_section16014119434>`

:ref:`MERGE <en-us_topic_0000001234200625__en-us_topic_0238518363_en-us_topic_0237362323_section97911516434>`

:ref:`NAMED <en-us_topic_0000001234200625__en-us_topic_0238518363_en-us_topic_0237362323_section20635162164315>`

:ref:`ACTIVITYCOUNT <en-us_topic_0000001234200625__en-us_topic_0238518363_en-us_topic_0237362323_section5591622124415>`

:ref:`TIMESTAMP <en-us_topic_0000001234200625__en-us_topic_0238518363_en-us_topic_0237362323_section10663203410406>`

.. _en-us_topic_0000001234200625__en-us_topic_0238518363_en-us_topic_0237362323_section9317194224211:

INSERT
------

The Teradata **INSERT** (:ref:`short key <en-us_topic_0000001233922185__en-us_topic_0238518364_en-us_topic_0237362468_section75711541419>` INS) statement is used to insert records into a table. DSC supports the **INSERT** statement.

The **INSERT INTO TABLE table_name** syntax exists in Teradata SQL, but is not supported by GaussDB(DWS). GaussDB(DWS) supports only the **INSERT INTO table_name** syntax. Therefore, remove the keyword **TABLE** when using DSC.

**Input**

.. code-block::

   INSERT TABLE tab1
   SELECT col1, col2
     FROM tab2
    WHERE col3 > 0;

**Output**

.. code-block::

   INSERT INTO tab1
   SELECT col1, col2
     FROM tab2
    WHERE col3 > 0;

.. _en-us_topic_0000001234200625__en-us_topic_0238518363_en-us_topic_0237362323_section6192558124215:

SELECT
------

#. .. _en-us_topic_0000001234200625__en-us_topic_0238518363_en-us_topic_0237362323_li7975713452:

   **ANALYZE**

   The Teradata **SELECT** command (:ref:`short key <en-us_topic_0000001233922185__en-us_topic_0238518364_en-us_topic_0237362468_section75711541419>` SEL) is used to specify the table columns from which data is to be retrieved.

   **ANALYZE** is used in GaussDB(DWS) for collecting optimizer statistics, which is used for improving query performance.

   **Input: ANALYZE with INSERT**

   ::

      INSERT INTO employee(empno,ename)  Values (1,'John');
      COLLECT STAT on employee;

   **Output**

   ::

      INSERT INTO employee( empno, ename)
      SELECT 1 ,'John';
      ANALYZE employee;

   **Input: ANALYZE with UPDATE**

   ::

      UPD employee SET ename = 'Jane'
              WHERE ename = 'John';
      COLLECT STAT on employee;

   **Output**

   ::

      UPDATE employee SET ename = 'Jane'
       WHERE ename = 'John';
      ANALYZE employee;

   **Input: ANALYZE with DELETE**

   ::

      DEL FROM employee WHERE ID > 10;
      COLLECT STAT on employee;

   **Output**

   .. code-block:: text

      DELETE FROM employee WHERE ID > 10;
      ANALYZE employee;

#. **Order of Clausses**

   For Teradata migration of **SELECT** statements, all the clauses (**FROM**, **WHERE**, **HAVING** and **GROUP BY**) can be listed in any order. The tool will not migrate the statement if it contains **a QUALIFY** as an **ALIAS** before the **FROM** clause.

   Use the :ref:`tdMigrateALIAS <en-us_topic_0000001233922159__en-us_topic_0218440346_li1163915119179>` configuration parameter to configure migration of ALIAS.

   **Input: Order of Clauses**

   ::

      SELECT expr1 AS alias1
            , expr2 AS alias2
            , expr3 AS alias3
            , MAX( expr4 ), ...
         FROM tab1 T1 INNER JOIN tab2 T2
           ON T1.c1 = T2.c2 ...
          AND T3.c5 = '010'
          AND ...
        WHERE T1.c7 = '000'
          AND ...
       HAVING alias1 <> 'IC'
               AND alias2 <> 'IC'
               AND alias3 <> ''
        GROUP BY 1, 2, 3 ;

   **Output**

   ::

      SELECT expr1 AS alias1
           , expr2 AS alias2
           , expr3 AS alias3
           , MAX( expr4 ), ...
        FROM tab1 T1 INNER JOIN tab2 T2
          ON T1.c1 = T2.c2 ...
         AND T3.c5 = '010'
         AND ...
       WHERE T1.c7 = '000'
         AND ...
       GROUP BY 1 ,2 ,3
      HAVING expr1 <> 'IC'
              AND expr2 <> 'IC'
              AND expr3 <> '';

   **Input: Order of Clauses**

   ::

      SELECT
                TOP 10 *
           GROUP BY
                DeptNo
           WHERE
                empID < 100
      FROM
                tbl_employee;

   **Output**

   ::

      SELECT
                *
           FROM
                tbl_employee
           WHERE
                empID < 100
           GROUP BY
                DeptNo LIMIT 10
      ;

   .. note::

      If the input script contains QUALIFY as an ALIAS before the FROM clause, the DSC will not migrate the statement and copy the input statement verbatim.

   **Input: Order of Clauses** **with QUALIFY as an ALIAS before the FROM clause**

   ::

      SELECT
                *
           FROM
                table1
           WHERE
                abc = (
                     SELECT
                               col1 AS qualify
                          FROM
                               TABLE
                               WHERE
                                    col1 = 5
                )
      ;

   **Output**

   ::

      SELECT
                *
           FROM
                table1
           WHERE
                abc = (
                     SELECT
                               col1 AS qualify
                          FROM
                               TABLE
                               WHERE
                                    col1 = 5
                )
      ;

#. .. _en-us_topic_0000001234200625__en-us_topic_0238518363_en-us_topic_0237362323_li0503163844512:

   **Extended Group by Clause**

   The **GROUP BY** clause can be specified if you want the database to group the selected rows based on the value of expr(s). If this clause contains **CUBE**, **ROLLUP** or **GROUPING SETS** extensions, then the database produces super-aggregate groupings in addition to the regular groupings. These features are not available in GaussDB(DWS), but similar functions can be enabled using the **UNION ALL** operator.

   Use the :ref:`extendedGroupByClause <en-us_topic_0000001233922159__en-us_topic_0218440346_li133691937183210>` configuration parameter to configure migration of the extended GROUP BY clause.

   **Input: Extended Group By Clause - CUBE**

   ::

      SELECT expr1 AS alias1
            , expr2 AS alias2
            , expr3 AS alias3
            , MAX( expr4 ), ...
         FROM tab1 T1 INNER JOIN tab2 T2
           ON T1.c1 = T2.c2 ...
          AND T3.c5 = '010'
          AND ...
        WHERE T1.c7 = '000'
          AND ...
       HAVING alias1 <> 'IC'
               AND alias2 <> 'IC'
               AND alias3 <> ''
        GROUP BY 1, 2, 3 ;

   **Output**

   ::

      SELECT expr1 AS alias1
           , expr2 AS alias2
           , expr3 AS alias3
           , MAX( expr4 ), ...
        FROM tab1 T1 INNER JOIN tab2 T2
          ON T1.c1 = T2.c2 ...
         AND T3.c5 = '010'
         AND ...
       WHERE T1.c7 = '000'
         AND ...
       GROUP BY 1 ,2 ,3
      HAVING expr1 <> 'IC'
              AND expr2 <> 'IC'
              AND expr3 <> '';

   **Input: Extended Group By Clause - ROLLUP**

   ::

      SELECT d.dname, e.job, MAX(e.sal)
        FROM emp e RIGHT OUTER JOIN dept d
          ON e.deptno=d.deptno
      WHERE e.job IS NOT NULL
      GROUP BY ROLLUP (d.dname, e.job);

   **Output**

   ::

      SELECT dname, job, ColumnAlias1
        FROM ( SELECT MAX(e.sal) AS ColumnAlias1, d.dname, e.job
                 FROM emp e RIGHT OUTER JOIN dept d
                   ON e.deptno = d.deptno
                WHERE e.job IS NOT NULL
                GROUP BY d.dname ,e.job
                UNION ALL
               SELECT MAX(e.sal) AS ColumnAlias1, d.dname, NULL AS
                       job
                 FROM emp e RIGHT OUTER JOIN dept d
                   ON e.deptno = d.deptno
                WHERE e.job IS NOT NULL
                GROUP BY d.dname
                UNION ALL
               SELECT MAX( e.sal ) AS ColumnAlias1, NULL AS dname,
                           NULL AS job
                 FROM emp e RIGHT OUTER JOIN dept d
                   ON e.deptno = d.deptno
                WHERE e.job IS NOT NULL
              );

   **Input: Extended Group By Clause - GROUPING SETS**

   ::

      SELECT d.dname, e.job, MAX(e.sal)
      FROM emp e RIGHT OUTER JOIN dept d
      ON e.deptno=d.deptno
      WHERE e.job IS NOT NULL
      GROUP BY GROUPING SETS(d.dname, e.job);

   **Output**

   ::

      SELECT dname, job, ColumnAlias1
        FROM ( SELECT MAX(e.sal) AS ColumnAlias1
                    , d.dname, NULL AS job
                 FROM emp e RIGHT OUTER JOIN dept d
                   ON e.deptno = d.deptno
                WHERE e.job IS NOT NULL
                GROUP BY d.dname
                UNION ALL
               SELECT MAX(e.sal) AS ColumnAlias1
                    , NULL AS dname, e.job
                 FROM emp e RIGHT OUTER JOIN dept d
                   ON e.deptno = d.deptno
                WHERE e.job IS NOT NULL
                GROUP BY e.job
              );

#. **TOP and SAMPLE**

   The **TOP** and **SAMPLE** clauses of Teradata are migrated to **LIMIT** in GaussDB(DWS).

   a. TOP

      The DSC also supports migration of **TOP** statements with dynamic parameters.

      .. note::

         -  For **TOP** clauses containing **WITH TIES**, the ORDER BY clause is also required. Otherwise, the tool will not migrate the statement and copy it as it is.
         -  When using TOP with dynamic parameters:

            -  The input dynamic parameters should be in the following form:

               ::

                   TOP :<parameter_name>

               The following characters are valid for dynamic parameters: a-z, A-Z, 0-9 and "_".

      **Input: SELECT .. TOP**

      ::

         SELECT TOP 1 c1, COUNT (*) cnt
           FROM tab1
          GROUP BY c1
          ORDER BY cnt;

      **Output**

      ::

         SELECT c1, COUNT( * ) cnt
           FROM tab1
          GROUP BY c1
          ORDER BY cnt
          LIMIT 1;

      **Input:** **SELECT .. TOP** **PERCENT**

      ::

         SELECT TOP 10 PERCENT c1, c2
           FROM employee
          WHERE ...
          ORDER BY c2 DESC;

      **Output**

      ::

         WITH top_percent AS (
               SELECT c1, c2
                 FROM employee
                WHERE ...
                ORDER BY c2 DESC
                             )
         SELECT *
           FROM top_percent
          LIMIT (SELECT CEIL(COUNT( * ) * 10 / 100)
                   FROM top_percent);

      **Input:** **SELECT .. TOP with** **dynamic parameters**

      ::

         SELECT
                    TOP :Limit WITH TIES c1
                   ,SUM (c2) sc2
              FROM
                   tab1
              WHERE
                   c3 > 10
              GROUP BY
                   c1
              ORDER BY
                   c1
         ;

      **Output**

      ::

         WITH top_ties AS (
              SELECT
                         c1
                        ,SUM (c2) sc2
                        ,rank (
                        ) OVER( ORDER BY c1 ) AS TOP_RNK
                   FROM
                        tab1
                   WHERE
                        c3 > 10
                   GROUP BY
                        c1
         ) SELECT
                   c1
                   ,sc2
              FROM
                   top_ties
              WHERE
                   TOP_RNK <= :Limit
              ORDER BY
                   TOP_RNK
         ;

      **Input:** **SELECT .. TOP with** **dynamic parameters and with TIES**

      ::

          SELECT
                    TOP :Limit WITH TIES Customer_ID
            FROM
                   Customer_t
            ORDER BY
                   Customer_ID
         ;

      **Output**

      ::

         WITH top_ties AS (
              SELECT
                        Customer_ID
                        ,rank (
                        ) OVER( order by Customer_id) AS TOP_RNK
                   FROM
                        Customer_t
         ) SELECT
                   Customer_ID
              FROM
                   top_ties
              WHERE
                   TOP_RNK <= :Limit
              ORDER BY
                   TOP_RNK
         ;

      **Input:** **SELECT .. TOP PERCENT with** **dynamic parameters**

      ::

         SELECT
                   TOP :Input_Limit PERCENT WITH TIES c1
                   ,SUM (c2) sc2
              FROM
                   tab1
              GROUP BY
                   c1
              ORDER BY
                   c1
         ;

      **Output**

      ::

         WITH top_percent_ties AS (
              SELECT
                        c1
                        ,SUM (c2) sc2
                        ,rank (
                        ) OVER( ORDER BY c1 ) AS TOP_RNK
                   FROM
                        tab1
                   GROUP BY
                        c1
         ) SELECT
                   c1
                   ,sc2
              FROM
                   top_percent_ties
              WHERE
                   TOP_RNK <= (
                        SELECT
                                  CEIL(COUNT( * ) * :Input_Limit / 100)
                             FROM
                                  top_percent_ties
                   )
              ORDER BY
                   TOP_RNK
         ;

   b. SAMPLE

      .. note::

         The tool only supports single positive integers in the SAMPLE clause.

      **Input:** **SELECT .. SAMPLE**

      ::

         SELECT c1, c2, c3
           FROM tab1
          WHERE c1 > 1000
         SAMPLE 1;

      **Output**

      ::

         SELECT c1, c2, c3
           FROM tab1
          WHERE c1 > 1000
          LIMIT 1;

.. _en-us_topic_0000001234200625__en-us_topic_0238518363_en-us_topic_0237362323_section13878017434:

UPDATE
------

The tool supports and migrates the **UPDATE** (:ref:`short key <en-us_topic_0000001233922185__en-us_topic_0238518364_en-us_topic_0237362468_section75711541419>` UPD) statements.

**Input: UPDATE with TABLE ALIAS**

::

   UPDATE T1
     FROM tab1 T1, tab2 T2
      SET c1 = T2.c1
        , c2 = T2.c2
    WHERE T1.c3 = T2.c3;

**Output**

::

   UPDATE tab1 T1
      SET c1 = T2.c1
        , c2 = T2.c2
     FROM tab2 T2
    WHERE T1.c3 = T2.c3;

**Input: UPDATE with TABLE ALIAS** **using a sub query**

::

   UPDATE t1
     FROM tab1 t1, ( SELECT c1, c2 FROM tab2
                      WHERE c2 > 100 ) t2
      SET c1 = t2.c1
    WHERE t1.c2 = t2.c2;

**Output**

::

    UPDATE tab1 t1
      SET c1 = t2.c1
     FROM ( SELECT c1, c2 FROM tab2
             WHERE c2 > 100 ) t2
    WHERE t1.c2 = t2.c2;

**Input: UPDATE with ANALYZE**

::

   UPD employee SET ename = 'Jane'
           WHERE ename = 'John';
   COLLECT STAT on employee;

**Output**

::

   UPDATE employee SET ename = 'Jane'
    WHERE ename = 'John';
   ANALYZE employee;

.. _en-us_topic_0000001234200625__en-us_topic_0238518363_en-us_topic_0237362323_section16014119434:

DELETE
------

**DELETE** (:ref:`short key <en-us_topic_0000001233922185__en-us_topic_0238518364_en-us_topic_0237362468_section75711541419>` abbreviated as **DEL**) is an ANSI-compliant SQL syntax operator used to delete existing records from a table. DSC supports the Teradata **DELETE** statement and its short key **DEL**. The **DELETE** statement that does not contain the **WHERE** clause is migrated to **TRUNCATE** in GaussDB(DWS). Use the :ref:`deleteToTruncate <en-us_topic_0000001233922159__en-us_topic_0218440346_li2884123118322>` parameter to enable or disable this behavior.

**Input: DELETE**

::

   DEL FROM tab1
    WHERE a =10;

**Output**

.. code-block:: text

   DELETE FROM tab1
    WHERE a =10;

**Input: DELETE without WHERE - Migrated to TRUNCATE** **if deletetoTruncate=TRUE**

.. code-block:: text

   DELETE FROM ${schemaname} . "tablename" ALL;

**Output**

::

   TRUNCATE
        TABLE
             ${schemaname} . "tablename";

**In DELETE, the same table is used in DELETE and FROM clauses with / without WHERE clause**

**Input**

.. code-block:: text

   DELETE DP_TMP.M_P_TX_SCV_REMAINING_PARTY
   FROM DP_TMP.M_P_TX_SCV_REMAINING_PARTY ALL ;
   ---
   DELETE DP_VMCTLFW.CTLFW_Process_Id
   FROM DP_VMCTLFW.CTLFW_Process_Id
   WHERE (Process_Name =  :_spVV2 )
   AND  (Process_Id  NOT IN (SELECT MAX(Process_Id )(NAMED Process_Id )
                                         FROM DP_VMCTLFW.CTLFW_Process_Id
                                        WHERE Process_Name =  :_spVV2 )
         );
   ---
   DELETE CPID
   FROM DP_VMCTLFW.CTLFW_Process_Id AS CPID
   WHERE (Process_Name =  :_spVV2 )
   AND  (Process_Id  NOT IN (SELECT MAX(Process_Id )(NAMED Process_Id )
                                         FROM DP_VMCTLFW.CTLFW_Process_Id
                                        WHERE Process_Name =  :_spVV2 )
         );

**Output**

.. code-block:: text

   DELETE FROM DP_TMP.M_P_TX_SCV_REMAINING_PARTY;
   ---
   DELETE FROM DP_VMCTLFW.CTLFW_Process_Id
   WHERE (Process_Name =  :_spVV2 )
   AND  (Process_Id  NOT IN (SELECT MAX(Process_Id )(NAMED Process_Id )
                                         FROM DP_VMCTLFW.CTLFW_Process_Id
                                        WHERE Process_Name =  :_spVV2 )
         );
   ---
   DELETE FROM DP_VMCTLFW.CTLFW_Process_Id AS CPID
   WHERE (Process_Name =  :_spVV2 )
   AND  (Process_Id  NOT IN (SELECT MAX(Process_Id )(NAMED Process_Id )
                                         FROM DP_VMCTLFW.CTLFW_Process_Id
                                        WHERE Process_Name =  :_spVV2 )
         );

**DELETE table_alias FROM table**

**Input**

.. code-block::

   SQL_Detail10124.sql
   delete a
     from ${BRTL_DCOR}.BRTL_CS_POT_CUST_UMPAY_INF_S as a
    where a.DW_Snsh_Dt = cast('${v_Trx_Dt}' as date format 'yyyy-mm-dd')
    and a.DW_Job_Seq = 1 ;
   was migrated as below:
         DELETE FROM
              BRTL_DCOR.BRTL_CS_POT_CUST_UMPAY_INF_S AS a
                   USING
         WHERE a.DW_Snsh_Dt = CAST( lv_mig_v_Trx_Dt AS DATE )
              AND a.DW_Job_Seq = 1 ;
   SQL_Detail10449.sql
   delete a
     from ${BRTL_DCOR}.BRTL_EM_YISHITONG_USR_INF as a
    where a.DW_Job_Seq = 1 ;
   was migrated as below:
         DELETE FROM
              BRTL_DCOR.BRTL_EM_YISHITONG_USR_INF AS a
                   USING
         WHERE a.DW_Job_Seq = 1 ;
   SQL_Detail5742.sql
   delete a
     from ${BRTL_DCOR}.BRTL_PD_FP_NAV_ADT_INF as a;
   was migrated as
         DELETE a
    FROM
         BRTL_DCOR.BRTL_PD_FP_NAV_ADT_INF AS a ;

**Output**

.. code-block::

   SQL_Detail10124.sql
   delete from ${BRTL_DCOR}.BRTL_CS_POT_CUST_UMPAY_INF_S as a
    where a.DW_Snsh_Dt = cast('${v_Trx_Dt}' as date format 'yyyy-mm-dd')
    and a.DW_Job_Seq = 1 ;
   SQL_Detail10449.sql
   delete from ${BRTL_DCOR}.BRTL_EM_YISHITONG_USR_INF as a
    where a.DW_Job_Seq = 1 ;
   SQL_Detail5742.sql
   delete from ${BRTL_DCOR}.BRTL_PD_FP_NAV_ADT_INF as a;

.. _en-us_topic_0000001234200625__en-us_topic_0238518363_en-us_topic_0237362323_section97911516434:

MERGE
-----

.. note::

   Gauss database in 6.5.0 or later versions support the MERGE function.

**MERGE** is an ANSI-standard SQL syntax operator used to select rows from one or more sources for updating or inserting into a table or view. The conditions to update or insert to the target table or view can be specified.

**Input: MERGE**

::

   MERGE INTO tab1 A
   using ( SELECT c1, c2, ... FROM tab2 WHERE ...) AS B
   ON A.c1 = B.c1
    WHEN MATCHED THEN
      UPDATE SET c2 = c2
               , c3 = c3
     WHEN NOT MATCHED THEN
   INSERT VALUES (B.c1, B.c2, B.c3);

**Output**

::

   WITH B AS (
        SELECT
                  c1
                  ,c2
                  ,...
             FROM
                  tab2
             WHERE
                  ...
   )
   ,UPD_REC AS (
        UPDATE
                  tab1 A
             SET
                  c2 = c2
                  ,c3 = c3
             FROM
                  B
             WHERE
                  A.c1 = B.c1 returning A. *
   )
   INSERT
        INTO
             tab1 SELECT
                       B.c1
                       ,B.c2
                       ,B.c3
                    FROM
                       B
                   WHERE
                       NOT EXISTS (
                            SELECT
                                    1
                              FROM
                                    UPD_REC A
                             WHERE
                                    A.c1 = B.c1
                                  )
   ;

.. _en-us_topic_0000001234200625__en-us_topic_0238518363_en-us_topic_0237362323_section20635162164315:

NAMED
-----

**NAMED** is used in Teradata to assign a temporary name to an expression or column. The **NAMED** statements for expression names are migrated to **AS** in GaussDB(DWS). The **NAMED** statements for column names are retained in the same syntax.

**Input: NAMED** **Expression migrated to AS**

::

   SELECT Name, ((Salary + (YrsExp * 200))/12) (NAMED Projection)
     FROM Employee
    WHERE DeptNo = 600 AND Projection < 2500;

**Output**

::

   SELECT Name, ((Salary + (YrsExp * 200))/12) AS  Projection
     FROM Employee
    WHERE DeptNo = 600 AND ((Salary + (YrsExp * 200))/12)  < 2500;

**Input: NAMED** **AS for Column Name**

::

   SELECT product_id AS id
     FROM emp where pid=2 or id=2;

**Output**

::

   SELECT product_id (NAMED "pid") AS id
     FROM emp where product_id=2 or product_id=2;

**Input: NAMED( ) for Column Name**

::

   INSERT INTO Neg100 (NAMED,ID,Dept) VALUES ('TEST',1,'IT');

**Output**

::

   INSERT INTO Neg100 (NAMED,ID,Dept) SELECT 'TEST',1, 'IT';

**Input: NAMED** **alias with TITLE** **alias without AS**

::

   SELECT dept_name (NAMED alias1) (TITLE alias2 )
     FROM employee
    WHERE dept_name like 'Quality';

**Output**

::

   SELECT dept_name
       AS alias1
     FROM employee
    WHERE dept_name like 'Quality';

**Input: NAMED** **alias with TITLE** **alias** **with AS**

The DSC will skip the NAMED alias and TITLE alias and use only the AS alias.

::

   SELECT sale_name (Named alias1 ) (Title alias2)
       AS alias3
     FROM employee
    WHERE sname = 'Stock' OR sname ='Sales';

**Output**

::

   SELECT sale_name
       AS alias3
     FROM employee
    WHERE sname = 'Stock' OR sname ='Sales';

**Input: NAMED with TITLE**

NAMED and TITLE used together, separated by comma(,) within brackets().

::

   SELECT customer_id (NAMED cust_id, TITLE 'Customer Id')
   FROM Customer_T
   WHERE cust_id > 10;

**Output**

::

   SELECT cust_id AS "Customer Id"
   FROM   (SELECT customer_id AS cust_id
                   FROM   customer_t
                   WHERE  cust_id > 10);

.. _en-us_topic_0000001234200625__en-us_topic_0238518363_en-us_topic_0237362323_section5591622124415:

ACTIVITYCOUNT
-------------

**Input**

It's a status variable that returns the number of rows affected by an SQL DML statement in an embedded SQL.

::

   SEL tablename
   FROM dbc.tables
   WHERE databasename ='tera_db'
     AND tablename='tab1';

   .IF ACTIVITYCOUNT > 0 THEN .GOTO NXTREPORT;
   CREATE MULTISET TABLE tera_db.tab1
           , NO FALLBACK
           , NO BEFORE JOURNAL
           , NO AFTER JOURNAL
           , CHECKSUM = DEFAULT
             (
                       Tx_Zone_Num CHAR( 4 )
                     , Tx_Org_Num  VARCHAR( 30 )
             )
             PRIMARY INDEX
             (
                       Tx_Org_Num
             )
             INDEX
             (
                       Tx_Teller_Id
             )
   ;

   .LABEL NXTREPORT
   DEL FROM tera_db.tab1;

**Output**

::

   DECLARE v_verify TEXT ;
   v_no_data_found NUMBER ( 1 ) ;

   BEGIN
        BEGIN
             v_no_data_found := 0 ;

             SELECT
                       mig_td_ext.vw_td_dbc_tables.tablename INTO v_verify
                  FROM
                       mig_td_ext.vw_td_dbc_tables
                  WHERE
                       mig_td_ext.vw_td_dbc_tables.schemaname = 'tera_db'
                       AND mig_td_ext.vw_td_dbc_tables.tablename = 'tab1' ;

                  EXCEPTION
                       WHEN NO_DATA_FOUND THEN
                       v_no_data_found := 1 ;

        END ;

        IF
             v_no_data_found = 1 THEN
                  CREATE TABLE tera_db.tab1 (
                       Tx_Zone_Num CHAR( 4 )
                       ,Tx_Org_Num VARCHAR( 30 )
                  ) DISTRIBUTE BY HASH ( Tx_Org_Num ) ;

        CREATE
             INDEX
                  ON tera_db.tab1 ( Tx_Teller_Id ) ;

        END IF ;

        DELETE FROM
             tera_db.tab1 ;

   END ;
   /

.. _en-us_topic_0000001234200625__en-us_topic_0238518363_en-us_topic_0237362323_section10663203410406:

TIMESTAMP
---------

**Input - TIMESTAMP with FORMAT**

The FORMAT phrase sets the format for a specific TIME or TIMESTAMP column or value. A FORMAT phrase overrides the system format.

::

   SELECT 'StartDTTM' as a
                ,CURRENT_TIMESTAMP (FORMAT 'HH:MI:SSBMMMBDD,BYYYY');

**Output**

::

   SELECT 'StartDTTM' AS a
                ,TO_CHAR( CURRENT_TIMESTAMP ,'HH:MI:SS MON DD, YYYY' ) ;

**TIMESTAMP Types Casting**

**Input**

.. code-block::

   COALESCE( a.Snd_Tm ,TIMESTAMP '0001-01-01 00:00:00' )
   should be migrated as below:
   COALESCE( a.Snd_Tm , CAST('0001-01-01 00:00:00' AS TIMESTAMP) )

**Output**

.. code-block::

   COALESCE( a.Snd_Tm , CAST('0001-01-01 00:00:00' AS TIMESTAMP) )
