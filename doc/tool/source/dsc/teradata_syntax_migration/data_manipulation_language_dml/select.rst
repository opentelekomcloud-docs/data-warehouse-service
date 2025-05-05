:original_name: dws_16_0087.html

.. _dws_16_0087:

.. _en-us_topic_0000001860198961:

SELECT
======

.. _en-us_topic_0000001860198961__en-us_topic_0000001384390508_section1964918592810:

ANALYZE
-------

The Teradata **SELECT** command (:ref:`short key <en-us_topic_0000001860198793>` SEL) is used to specify the table columns from which data is to be retrieved.

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

.. _en-us_topic_0000001860198961__en-us_topic_0000001384390508_section12736428201017:

Order of Clauses
----------------

For Teradata migration of **SELECT** statements, all the clauses (**FROM**, **WHERE**, **HAVING** and **GROUP BY**) can be listed in any order. If the **FROM** clause of a statement contains a QUALIFY clause that is used as **ALIAS**, DSC does not migrate the statement.

Use the :ref:`tdMigrateALIAS <en-us_topic_0000001813438796__en-us_topic_0000001432527901_li1163915119179>` configuration parameter to configure migration of ALIAS.

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

   SELECT
             expr1 AS "alias1"
             ,expr2 AS "alias2"
             ,expr3 AS "alias3"
             ,MAX( expr4 )
             ,...
        FROM
             tab1 T1 INNER JOIN tab2 T2
                  ON T1.c1 = T2.c2 ...
             AND T3.c5 = '010'
             AND ...
        WHERE
             T1.c7 = '000'
             AND ...
        GROUP BY
             1 ,2 ,3
        HAVING
             alias1 <> 'IC'
             AND alias2 <> 'IC'
             AND alias3 <> '' ;

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

   If the input script contains **QUALIFY** as an **ALIAS** before the **FROM** clause, DSC will not migrate the statement and copy the input statement verbatim.

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

.. _en-us_topic_0000001860198961__en-us_topic_0000001384390508_section1718993814110:

Extended Group By Clause
------------------------

The **GROUP BY** clause can be specified if you want the database to group the selected rows based on the value of expr(s). If this clause contains **CUBE**, **ROLLUP** or **GROUPING SETS** extensions, the database produces super-aggregate groupings in addition to the regular groupings. These features are not available in GaussDB(DWS), but similar functions can be enabled using the **UNION ALL** operator.

Use the :ref:`extendedGroupByClause <en-us_topic_0000001813438796__en-us_topic_0000001432527901_li133691937183210>` configuration parameter to configure migration of the extended **GROUP BY** clause.

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

   SELECT
             expr1 AS "alias1"
             ,expr2 AS "alias2"
             ,expr3 AS "alias3"
             ,MAX( expr4 )
             ,...
        FROM
             tab1 T1 INNER JOIN tab2 T2
                  ON T1.c1 = T2.c2 ...
             AND T3.c5 = '010'
             AND ...
        WHERE
             T1.c7 = '000'
             AND ...
        GROUP BY
             1 ,2 ,3
        HAVING
             alias1 <> 'IC'
             AND alias2 <> 'IC'
             AND alias3 <> '' ;

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

SELECT AS
---------

GaussDB(DWS) variable names are case insensitive, while Teradata variable names are case sensitive. To ensure that the Teradata script is correct before and after the migration, retain the case of the original variable name in the variable definition of the **SELECT** statement. The converted variable is defined in the **AS** *Variable name*.

**Input**

.. code-block::

   SELECT  TRIM('${JOB_NAME}')                                                                    AS JOB_NAME
          ,CASE WHEN LENGTH(trim(STRTOK('${JOB_NAME}','-',4)))=2
               THEN trim(STRTOK('${JOB_NAME}','-',4))
                ELSE ''
                END                                                                                     AS EDW_BANK_NM
          ,TRIM('${TX_DATE}')                                                                     AS TX_DATE
          ,USER                                                                                   AS ETL_USER
          ,CAST( CURRENT_TIMESTAMP(0) AS VARCHAR(19))                                             AS CURR_STIME
          ,'${ETL_DATA}'                                                                          AS ETL_DATA
          ,'T61_INDV_CUST_ACCT_ORG_AUM'                                                           AS TARGET_TABLE
          ,'CAST(''8999-12-31'' AS DATE)'                                                         AS MAXDATE
   ;
   .IF ERRORCODE <> 0 THEN .QUIT 12

**Output**

.. code-block::

   SELECT
             TRIM( '${job_name}' ) AS "JOB_NAME"
             ,CASE
                       WHEN LENGTH( TRIM( split_part ( '${job_name}' ,'-' ,4 ) ) ) = 2 THEN TRIM( split_part ( '${job_name}' ,'-' ,4 ) )
                  ELSE ''
             END AS "EDW_BANK_NM"
             ,TRIM( '${tx_date}' ) AS "TX_DATE"
             ,USER AS "ETL_USER"
             ,CAST( CURRENT_TIMESTAMP( 0 ) AS VARCHAR( 19 ) ) AS "CURR_STIME"
             ,'${etl_data}' AS "ETL_DATA"
             ,'T61_INDV_CUST_ACCT_ORG_AUM' AS "TARGET_TABLE"
             ,'CAST(''8999-12-31'' AS DATE)' AS "MAXDATE" ;
   \if ${ERROR} != 'false'
    \q 12
   \endif

   ;

Definition nested with **AS** expression is implemented by splitting multiple statements.

**Input**

.. code-block::

   SELECT  TRIM('${JOB_NAME}')                                                                    AS JOB_NAME
          ,'CAST(''0001-01-02'' AS DATE)'                                                         AS ILLDATE
          ,'T61_INDV_CUST_HOLD_PROD_IND_AUM'                                                      AS TARGET_TABLE
          ,0                                                                                      AS NULLNUMBER
          ,'CAST(''00:00:00.999'' AS TIME(3))'                                                    AS NULLTIME
          ,'CAST(''0001-01-01 00:00:00.000000'' AS TIMESTAMP(6))'                                 AS NULLTIMESTAMP
          ,'VT_'||TARGET_TABLE                                                                    AS VT_TABLE
          ,'V'||SUBSTR(TARGET_TABLE,2,CHAR(TARGET_TABLE)-1)                                       AS TARGET_TABLE_V
          ,'${GDM_DETAIL_DDL}'                                                                    AS V_TDDLDB
          ,'${GDM_DETAIL_VIEW}'                                                                   AS V_TARGETDB
          ,'${UDF}'                                                                               AS V_PUB_UDF

   ;
   .IF ERRORCODE <> 0 THEN .QUIT 12

**Output**

.. code-block::

   SELECT
             TRIM( '${job_name}' ) AS "JOB_NAME"
             ,'CAST(''0001-01-02'' AS DATE)' AS "ILLDATE"
             ,'T61_INDV_CUST_HOLD_PROD_IND_AUM' AS "TARGET_TABLE"
             ,0 AS "NULLNUMBER"
             ,'CAST(''00:00:00.999'' AS TIME(3))' AS "NULLTIME"
             ,'CAST(''0001-01-01 00:00:00.000000'' AS TIMESTAMP(6))' AS "NULLTIMESTAMP"
             ,'${gdm_detail_ddl}' AS "V_TDDLDB"
             ,'${gdm_detail_view}' AS "V_TARGETDB"
             ,'${udf}' AS "V_PUB_UDF" ;
   SELECT
             'VT_' || '${TARGET_TABLE}' AS "VT_TABLE" ;
   SELECT
             'V' || SUBSTR( '${TARGET_TABLE}' ,2 ,LENGTH( '${TARGET_TABLE}' ) - 1 ) AS "TARGET_TABLE_V" ;
   \if ${ERROR} != 'false'
    \q 12
   \endif

   ;

.. _en-us_topic_0000001860198961__en-us_topic_0000001384390508_section10916131891314:

TOP Clauses
-----------

DSC also supports the migration of **TOP** statements with dynamic parameters. The **TOP** clauses of Teradata are migrated to the **LIMIT** clauses in GaussDB(DWS).

.. note::

   -  When migrating a statement with a **TOP** clause that includes **WITH TIES**, it is necessary to include the **ORDER BY** clause as well. Otherwise, the tool will be unable to migrate the statement, and it will be copied as it is.
   -  When using TOP with dynamic parameters:

      -  The input dynamic parameters should be in the following form:

         ::

             TOP :<parameter_name>

         The following characters are allowed: lowercase letters (a-z), uppercase letters (A-Z), digits (0-9), and underscores (_).

**Input: SELECT...TOP**

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

**Input:** **SELECT...TOP PERCENT**

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

**Input:** **SELECT...TOP with dynamic parameters**

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

**Input:** **SELECT...TOP** **with dynamic parameters and TIES**

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

**Input:** **SELECT...TOP PERCENT with dynamic parameters**

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

SAMPLE clauses
--------------

The **SAMPLE** clause of Teradata is migrated to the **LIMIT** clause in GaussDB(DWS).

.. note::

   The tool only supports single positive integers in the **SAMPLE** clause.

**Input: SELECT...SAMPLE**

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
