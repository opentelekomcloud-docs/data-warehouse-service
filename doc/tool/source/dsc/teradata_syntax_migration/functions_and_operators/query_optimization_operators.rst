:original_name: dws_16_0056.html

.. _dws_16_0056:

.. _en-us_topic_0000001813598972:

Query Optimization Operators
============================

This section describes the syntax for migrating Teradata query optimization operators. The migration syntax determines how the keywords and features are migrated.

Use the :ref:`inToExists <en-us_topic_0000001813438796__en-us_topic_0000001432527901_li9993144993210>` parameter to configure the migration from **IN** or\ **NOT IN** to **EXISTS** or **NOT EXISTS**.

This parameter defaults to **FALSE**. To enable the query optimization feature, this parameter must be set to **TRUE**.

When being converted to GaussDB(DWS) SQL queries, Teradata queries containing the **IN** and **NOT IN** operators have been optimized, and **IN** and **NOT IN** have been converted to **EXISTS** and **NOT EXISTS**, respectively. The **IN** and **NOT IN** operators support single or multiple columns. DSC will migrate the **IN** or **NOT IN** statement only when it exists in the **WHERE** or **ON** clause. The following example shows the conversion from **IN** to **EXISTS**, which is also applicable to the conversion from **NOT IN** to **NOT EXISTS**.

**Simple conversion from IN to EXISTS**

In the following example, the keyword **IN** is provided in the input file. During the migration, DSC replaces **IN** with **EXISTS** to optimize query performance.

.. note::

   -  The **IN** and **NOT IN** statements with nested **IN** and **NOT IN** keywords cannot be migrated. In this case, the scripts will be invalid after migration.

      ::

         UPDATE tab1
            SET b = 123
          WHERE b IN ('abc')
            AND b IN ( SELECT  i
                         FROM tab2
                         WHERE j NOT IN (SELECT m
                                          FROM tab3
                                        )
                      )
         ;

      When an **IN** or **NOT IN** statement containing subqueries is being migrated, comments between the **IN** or **NOT IN** operator and the subqueries (see the example) cannot be migrated.

      **Example:**

      ::

         SELECT *
           FROM categories
          WHERE category_name
             IN --comment
                 ( SELECT category_name
                     FROM categories1 )
          ORDER BY category_name;

   -  **Migrating IN or NOT IN statements whose object names contain $ and #**

      -  DSC will not migrate the query if the **TABLE** name or **TABLE ALIAS** starts with **$**.

         ::

            SELECT Customer_Name
              FROM Customer_t $A
             WHERE Customer_ID IN( SELECT Customer_ID FROM Customer_t );

      -  If the **COLUMN** name starts with **#**, DSC may fail to migrate the query.

         ::

            SELECT Customer_Name
              FROM Customer_t
             WHERE #Customer_ID IN( SELECT #Customer_ID FROM Customer_t );

**Input: IN**

::

   SELECT ...
      FROM tab1 t
      WHERE t.col1 IN (SELECT icol1 FROM tab2 e)
    ORDER BY col1

**Output:**

::

   SELECT ...
      FROM tab1 t
      WHERE EXISTS (SELECT icol1
                       FROM tab2 e
                      WHERE icol1 = t.col1
                    )
    ORDER BY col1;

**Input: IN with multiple columns and Aggregate functions**

::

   SELECT deptno, job_id, empno, salary, bonus
      FROM emp_t
     WHERE ( deptno, job_id, CAST(salary AS NUMBER(10,2))+CAST(bonus AS NUMBER(10,2)) )
                   IN ( SELECT deptno, job_id,
                         MAX(CAST(salary AS NUMBER(10,2))+CAST(bonus AS NUMBER(10,2)))
                           FROM emp_t
                          WHERE hire_dt >= CAST( '20170101' AS DATE FORMAT 'YYYYMMDD' )
                          GROUP BY deptno, job_id )
       AND hire_dt IS NOT NULL;

**Output:**

::

   SELECT deptno, job_id, empno, salary, bonus
      FROM emp_t MAlias1
     WHERE EXISTS ( SELECT deptno, job_id,
                               MAX(CAST(salary AS NUMBER(10,2))+CAST(bonus AS NUMBER(10,2)))
                         FROM emp_t
                        WHERE hire_dt >= CAST( '20170101' AS DATE)
                              AND deptno  = MAlias1.deptno
                              AND job_id   = MAlias1.job_id
                        GROUP BY deptno, job_id
                       HAVING MAX(CAST(salary AS NUMBER(10,2))+CAST(bonus AS NUMBER(10,2)))
                               = CAST(MAlias1.salary AS NUMBER(10,2))+CAST(MAlias1.bonus AS NUMBER(10,2)) )
       AND hire_dt IS NOT NULL;
