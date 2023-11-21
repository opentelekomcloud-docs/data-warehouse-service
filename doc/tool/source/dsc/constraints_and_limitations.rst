:original_name: dws_mt_0012.html

.. _dws_mt_0012:

Constraints and Limitations
===========================

This section describes the constraints and limitations of DSC.

General
-------

-  DSC is used only for syntax migration and not for data migration.

-  If the **SELECT** clause of a subquery contains an aggregate function when the **IN** or **NOT IN** operator is converted to **EXISTS** or **NOT EXISTS**, errors may occur during script migration.

Teradata
--------

-  If a **case** statement containing **FORMAT** is not enclosed in parentheses, this statement will not be processed.

   For example:

   ::

      case when column1='0' then column1='value' end (FORMAT 'YYYYMMDD')as alias1

   In this example, **case when column1= "0", column1= "value" end** is not enclosed in parentheses and it will not be processed.

-  If **SELECT \*** and **QUALIFY** clauses are both used in an input query, the migrated query returns an additional column for the **QUALIFY** clause.

   An example is as follows:

   Teradata query

   ::

      SELECT * FROM dwQErrDtl_mc.C03_CORP_TIME_DPSIT_ACCT
      WHERE 1 = 1
      AND Data_Dt = CAST( '20150801' AS DATE FORMAT 'YYYYMMDD' )
      QUALIFY ROW_NUMBER( ) OVER( PARTITION BY Agt_Num, Agt_Modif_Num ORDER BY NULL ) = 1;

   Query after migration

   ::

      SELECT * FROM (
      SELECT *, ROW_NUMBER( ) OVER( PARTITION BY Agt_Num, Agt_Modif_Num ORDER BY NULL ) AS ROW_NUM1
      FROM dwQErrDtl_mc.C03_CORP_TIME_DPSIT_ACCT
      WHERE 1 = 1
      AND Data_Dt = CAST( '20150801' AS DATE )
      ) Q1
      WHERE Q1.ROW_NUM1 = 1;

   In the migrated query, the **ROW_NUMBER() OVER(PARTITION BY Agt_Num and Agt_Modif_Num ORDER BY NULL) AS ROW_NUM1** column is returned additionally.

-  Named references to a table in a query cannot be migrated from subqueries or functions.

   For example, if the input query contains a table named **foo**, DSC will not migrate any named references to the table from a subquery (**foo.fooid**) or when called from a function (**getfoo(foo.fooid)**).

   ::

      SELECT * FROM foo
       WHERE foosubid IN (
                           SELECT foosubid
                             FROM getfoo(foo.fooid) z
                            WHERE z.fooid = foo.fooid
                            );

Oracle
------

-  **ROWID** is not processed in the following case:

   ::

      SELECT empno,ename ,d.deptno,d.rowid  FROM ( SELECT * FROM employees where deptno is not null) e  , dept d WHERE d.deptno = e.deptno;

-  The aggregate function in Window functions (RANK and ROW_NUMBER) is not available in the column list.

   For example:

   ::

      SELECT
            TIME, Region, ROW_NUMBER ( ) OVER (ORDER BY SUM (profit) DESC) AS rownumber
              , GROUPING (TIME) AS T
              , GROUPING (Region) AS R
          FROM Sales GROUP BY
              CUBE (TIME, Region)
          ORDER BY
              TIME, Region;

-  The **INSERT FIRST** and **INSERT ALL** queries cannot be migrated if an asterisk (*) is specified in the **SELECT** statement and the **VALUES** clause is not specified.

-  DSC supports the **ROWNUM** condition specified at the end of the **WHERE** clause.

   For example:

   ::

      SELECT ROWNUM, ename,empid
      FROM employees
      WHERE empid = 10 AND deptno = 10 AND ROWNUM < 3
      ORDER BY ename;

-  DSC does not support the **ROWNUM** function in the **UPDATE** clause.

   For example:

   ::

      UPDATE tableName SET empno = ROWNUM
         where column = "value";

-  DSC does not support **PARTITION BY LIST** in the **INDEX** clause.

   For example:

   ::

      CREATE TABLE sales(acct_no NUMBER(5)
           ORGANIZATION INDEX
                   INCLUDING acct_no
                   OVERFLOW TABLESPACE example
           PARTITION BY LIST (acct_no)
                  (PARTITION VALUES (1)
           PARTITION VALUES (DEFAULT)
                         TABLESPACE example);

-  DSC does not support the **JOIN** condition without a table name or table alias.

-  Stored procedures and functions must end with a slash (/).
