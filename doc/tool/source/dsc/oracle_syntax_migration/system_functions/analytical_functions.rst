:original_name: dws_mt_0138.html

.. _dws_mt_0138:

Analytical Functions
====================

Analytical functions compute an aggregate value based on a group of rows. They differ from aggregate functions in that they return multiple rows for each group. Analytical functions are commonly used to compute cumulative, moving, centered, and reporting aggregates. DSC supports analytical functions including the RATIO_TO_REPORT function.

**Input - Analytical Functions**

.. code-block::

   SELECT empno, ename,  deptno
     , COUNT(*) OVER() AS cnt
     , AVG(DISTINCT empno) OVER (PARTITION BY deptno) AS cnt_dst
     FROM emp
   ORDER BY empno;

**Output**

.. code-block::

   WITH aggDistQuery1 AS (
        SELECT
                  deptno
                  ,AVG (
                       DISTINCT empno
                  ) aggDistAlias1
             FROM
                  emp
             GROUP BY
                  deptno
   ) SELECT
             empno
             ,ename
             ,deptno
             ,COUNT( * ) OVER( ) AS cnt
             ,(
                  SELECT
                            aggDistAlias1
                       FROM
                            aggDistQuery1
                       WHERE
                            deptno = MigTblAlias.deptno
             ) AS cnt_dst
        FROM
             emp MigTblAlias
        ORDER BY
            empno
   ;

RATIO_TO_REPORT
---------------

RATIO_TO_REPORT is an analytic function which returns the proportion of a value to a group of values.

**Input - RATIO_TO_REPORT**

.. code-block::

   SELECT last_name, salary
              , RATIO_TO_REPORT(salary) OVER () AS rr
     FROM employees
   WHERE job_id = 'PU_CLERK';

**Output**

.. code-block::

   SELECT last_name, salary
        , salary / NULLIF( SUM (salary) OVER( ), 0 ) AS rr
     FROM employees
   WHERE job_id = 'PU_CLERK';

**Input - RATIO_TO_REPORT** **with AGGREGATE column in SELECT**

.. code-block::

   SELECT
             Ename
             ,Deptno
             ,Empno
             ,SUM (salary)
             ,RATIO_TO_REPORT (
                  COUNT( DISTINCT Salary )
             ) OVER( PARTITION BY Deptno ) RATIO
        FROM
             emp1
        ORDER BY
             Ename
             ,Deptno
             ,Empno
   ;

**Output**

.. code-block::

   SELECT
             Ename
             ,Deptno
             ,Empno
             ,SUM (salary)
             ,COUNT( DISTINCT Salary ) / NULLIF( SUM ( COUNT( DISTINCT Salary ) ) OVER( PARTITION BY Deptno ) ,0 ) RATIO
        FROM
             emp1
        ORDER BY
             Ename
             ,Deptno
             ,Empno
   ;

**Input - RATIO_TO_REPORT** **with the AGGREGATE column using extending grouping feature but** **OUNT (Salary) in the RATIO TO REPORT column** **is not present in SELECT**

Use the :ref:`extendedGroupByClause <en-us_topic_0000001188202590__en-us_topic_0218440495_li121341415427>` configuration parameter to configure migration of the extended GROUP BY clause.

.. code-block::

   SELECT
             Ename
             ,Deptno
             ,Empno
             ,SUM (salary)
             ,RATIO_TO_REPORT (
                  COUNT( Salary )
             ) OVER( PARTITION BY Deptno ) RATIO
        FROM
             emp1
        GROUP BY
             GROUPING SETS (
                  Ename
                  ,Deptno
                  ,Empno
             )
        ORDER BY
             Ename
             ,Deptno
             ,Empno
   ;

**Output**

.. code-block::

   SELECT
             Ename
             ,Deptno
             ,Empno
             ,ColumnAlias1
             ,aggColumnalias1 / NULLIF( SUM ( aggColumnalias1 ) OVER( PARTITION BY Deptno ) ,0 ) RATIO
        FROM
             (
                  SELECT
                            SUM (salary) AS ColumnAlias1
                            ,COUNT( Salary ) aggColumnalias1
                            ,NULL AS Deptno
                            ,NULL AS Empno
                            ,Ename
                       FROM
                            emp1
                       GROUP BY
                            Ename
                  UNION
                  ALL SELECT
                            SUM (salary) AS ColumnAlias1
                            ,COUNT( Salary ) aggColumnalias1
                            ,Deptno
                            ,NULL AS Empno
                            ,NULL AS Ename
                       FROM
                            emp1
                       GROUP BY
                            Deptno
                  UNION
                  ALL SELECT
                            SUM (salary) AS ColumnAlias1
                            ,COUNT( Salary ) aggColumnalias1
                            ,NULL AS Deptno
                            ,Empno
                            ,NULL AS Ename
                       FROM
                            emp1
                       GROUP BY
                            Empno
             )
        ORDER BY
             Ename
             ,Deptno
             ,Empno
   ;
