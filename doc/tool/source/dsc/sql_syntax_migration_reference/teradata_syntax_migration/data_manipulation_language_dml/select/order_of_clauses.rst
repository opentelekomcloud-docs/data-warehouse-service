:original_name: dws_16_0089.html

.. _dws_16_0089:

.. _en-us_topic_0000001819416169:

Order of Clauses
================

For Teradata migration of **SELECT** statements, all the clauses (**FROM**, **WHERE**, **HAVING** and **GROUP BY**) can be listed in any order. The tool will not migrate the statement if it contains **a QUALIFY** as an **ALIAS** before the **FROM** clause.

Use the :ref:`tdMigrateALIAS <en-us_topic_0000001819416085__en-us_topic_0000001706224349_en-us_topic_0000001432527901_li1163915119179>` configuration parameter to configure migration of ALIAS.

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
