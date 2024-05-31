:original_name: dws_16_0090.html

.. _dws_16_0090:

.. _en-us_topic_0000001819336145:

Extended Group By Clause
========================

The **GROUP BY** clause can be specified if you want the database to group the selected rows based on the value of expr(s). If this clause contains **CUBE**, **ROLLUP** or **GROUPING SETS** extensions, then the database produces super-aggregate groupings in addition to the regular groupings. These features are not available in GaussDB(DWS), but similar functions can be enabled using the **UNION ALL** operator.

Use the :ref:`extendedGroupByClause <en-us_topic_0000001819416085__en-us_topic_0000001706224349_en-us_topic_0000001432527901_li133691937183210>` configuration parameter to configure migration of the extended GROUP BY clause.

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
