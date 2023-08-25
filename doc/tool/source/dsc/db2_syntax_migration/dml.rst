:original_name: dws_07_6842.html

.. _dws_07_6842:

DML
===

SELECT
------

FETCH clause

+-----------------------------------+-----------------------------------+
| DB2 Syntax                        | Syntax After Migration            |
+===================================+===================================+
| .. code-block::                   | .. code-block::                   |
|                                   |                                   |
|    SELECT empno, ename, deptno    |    SELECT empno, ename, deptno    |
|       FROM emp_t                  |       FROM emp_t                  |
|      ORDER BY salary              |      ORDER BY salary              |
|      FETCH FIRST ROW ONLY         |      LIMIT 1;                     |
|     -----                         |     -----                         |
|     SELECT empno, ename           |     SELECT empno, ename           |
|       FROM emp_t                  |       FROM emp_t                  |
|      WHERE deptno = 10            |      WHERE deptno = 10            |
|      ORDER BY salary              |      ORDER BY salary              |
|     fetch first 2 rows only;      |     LIMIT 2;                      |
+-----------------------------------+-----------------------------------+

.. note::

   The fetch-first-clause sets a maximum number of rows that can be retrieved.

WITH AS
-------

WITH AS with column list

+--------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------+
| DB2 Syntax                                                               | Syntax After Migration                                                                                       |
+==========================================================================+==============================================================================================================+
| .. code-block::                                                          | .. code-block::                                                                                              |
|                                                                          |                                                                                                              |
|    WITH rec (emp_no, emp_name, dept_name, dept_no) AS                    |    WITH rec AS                                                                                               |
|       ( SELECT empno, ename, cast('admin' as varchar(90)), 100 AS deptno |       ( SELECT empno AS emp_no, ename AS emp_name, cast('admin' as varchar(90)) AS dept_name, 100 AS dept_no |
|           FROM emp_t )                                                   |           FROM emp_t )                                                                                       |
|    SELECT * FROM rec;                                                    |    SELECT * FROM rec;                                                                                        |
+--------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------+

WITH AS with VALUES clause

+-----------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------+
| DB2 Syntax                                                                              | Syntax After Migration                                                                                                 |
+=========================================================================================+========================================================================================================================+
| .. code-block::                                                                         | .. code-block::                                                                                                        |
|                                                                                         |                                                                                                                        |
|     WITH rec (baseschema, basename, baselevel) AS                                       |     WITH rec AS                                                                                                        |
|        ( VALUES( cast('SCOTT' as varchar(90)), cast('EMP' as varchar(90)), 10000 ) )    |        ( SELECT cast('SCOTT' as varchar(90)) AS baseschema, cast('EMP' as varchar(90)) AS basename, 10000 AS baselevel |
|       SELECT owner, table_name, baselevel -1                                            |    From dual )                                                                                                         |
|         FROM ALL_TABLES, REC                                                            |       SELECT owner, table_name, baselevel -1                                                                           |
|        WHERE owner = BASESCHEMA                                                         |         FROM ALL_TABLES, REC                                                                                           |
|          AND table_name   = BASENAME;                                                   |        WHERE owner       = BASESCHEMA                                                                                  |
|                                                                                         |          AND table_name  = BASENAME;                                                                                   |
+-----------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------+

Table Function
--------------

TABLE function is specified with subquery.

+--------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------+
| DB2 Syntax                                                                                 | Syntax After Migration                                                                |
+============================================================================================+=======================================================================================+
| .. code-block::                                                                            | .. code-block::                                                                       |
|                                                                                            |                                                                                       |
|    SELECT prod_code, prod_desc, (received_qty-issued_qty) stk                              |    SELECT prod_code, prod_desc, (received_qty-issued_qty) stk                         |
|      FROM prod p, TABLE(select prod_code, SUM(received_qty) received_qty FROM prod_recd) r |      FROM prod p, (select prod_code, SUM(received_qty) received_qty FROM prod_recd) r |
|         , TABLE(select prod_code, SUM(issued_qty) issued_qty FROM prod_issue) i            |         , (select prod_code, SUM(issued_qty) issued_qty FROM prod_issue) i            |
|     WHERE r.prod_code = p.prod_code                                                        |     WHERE r.prod_code = p.prod_code                                                   |
|       AND i.prod_code = p.prod_code;                                                       |       AND i.prod_code = p.prod_code;                                                  |
+--------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------+
