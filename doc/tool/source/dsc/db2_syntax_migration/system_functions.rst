:original_name: dws_07_6846.html

.. _dws_07_6846:

System Functions
================

DAY
---

Day

+-----------------------------------+--------------------------------------------------------------------------+
| DB2 Syntax                        | Syntax After Migration                                                   |
+===================================+==========================================================================+
| .. code-block::                   | .. code-block::                                                          |
|                                   |                                                                          |
|    SELECT DAYS(doj) FROM emp;     |    SELECT (TRUNC(doj - TO_DATE('0001/01/01', 'YYYY/MM/DD'))+1) FROM emp; |
+-----------------------------------+--------------------------------------------------------------------------+

MONTH
-----

Month

+-------------------------------------------------+----------------------------------------------------------------+
| DB2 Syntax                                      | Syntax After Migration                                         |
+=================================================+================================================================+
| .. code-block::                                 | .. code-block::                                                |
|                                                 |                                                                |
|    SELECT (MONTH(ORDER_DATE)-1)/6+1 as TEMP_HY; |    SELECT (EXTRACT (MONTH FROM ORDER_DATE) -1)/6+1 as TEMP_HY; |
+-------------------------------------------------+----------------------------------------------------------------+

YEAR
----

Year

+----------------------------------------+------------------------------------------------------+
| DB2 Syntax                             | Syntax After Migration                               |
+========================================+======================================================+
| .. code-block::                        | .. code-block::                                      |
|                                        |                                                      |
|    SELECT YEAR(ORDER_DATE) as TEMP_HY; |    SELECT EXTRACT (YEAR FROM ORDER_DATE) as TEMP_HY; |
+----------------------------------------+------------------------------------------------------+

CURRENT DATE
------------

Current date

+-----------------------------------+-----------------------------------+
| DB2 Syntax                        | Syntax After Migration            |
+===================================+===================================+
| .. code-block::                   | .. code-block::                   |
|                                   |                                   |
|    SELECT CURRENT DATE FROM DUAL; |    SELECT CURRENT_DATE FROM DUAL; |
+-----------------------------------+-----------------------------------+

CURRENT TIMESTAMP
-----------------

Current timestamp

+---------------------------------------+---------------------------------------+
| DB2 Syntax                            | Syntax After Migration                |
+=======================================+=======================================+
| .. code-block::                       | .. code-block::                       |
|                                       |                                       |
|    SELECT CURRENT TIMESTAMP - 7 DAYS; |    SELECT CURRENT_TIMESTAMP - 7 DAYS; |
+---------------------------------------+---------------------------------------+

POSSTR
------

POSSTR

+----------------------------------------------------+---------------------------------------------------+
| DB2 Syntax                                         | Syntax After Migration                            |
+====================================================+===================================================+
| .. code-block::                                    | .. code-block::                                   |
|                                                    |                                                   |
|    SELECT POSSTR('THIS IS TEST','TEST') FROM DUAL; |    SELECT INSTR('THIS IS TEST','TEST') FROM DUAL; |
+----------------------------------------------------+---------------------------------------------------+

VALUE
-----

Value

+--------------------------------------+-----------------------------------------+
| DB2 Syntax                           | Syntax After Migration                  |
+======================================+=========================================+
| .. code-block::                      | .. code-block::                         |
|                                      |                                         |
|    Select VALUE('abc','') from dual; |    Select Coalesce('abc','') from dual; |
+--------------------------------------+-----------------------------------------+

Date
----

The **date** function returns a date using a value.

+------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------+
| DB2 Syntax                                                                               | Syntax After Migration                                                                                            |
+==========================================================================================+===================================================================================================================+
| .. code-block::                                                                          | .. code-block::                                                                                                   |
|                                                                                          |                                                                                                                   |
|    SELECT org_code, DATE(order_date)                                                     |    SELECT org_code, mig_db2_ext.mig_db2_fn_date(order_date)                                                       |
|       FROM view_cc_order                                                                 |       FROM view_cc_order                                                                                          |
|      WHERE order_date = DATE((SELECT start_date FROM year_week_mark                      |      WHERE order_date = mig_db2_ext.mig_db2_fn_date((SELECT start_date FROM year_week_mark                        |
|              WHERE year=TEMP_YEAR and week=TEMP_WEEK));                                  |              WHERE year=TEMP_YEAR and week=TEMP_WEEK));                                                           |
|     ---                                                                                  |     ---                                                                                                           |
|     SELECT deptno, deptname, DATE(SELECT max(doj) FROM emp e WHERE e.deptno = d.deptno)  |     SELECT deptno, deptname, mig_db2_ext.mig_db2_fn_date((SELECT max(doj) FROM emp e WHERE e.deptno = d.deptno))  |
|       FROM dept d;                                                                       |       FROM dept d;                                                                                                |
+------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------+
