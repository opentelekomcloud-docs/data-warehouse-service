:original_name: dws_16_0096.html

.. _dws_16_0096:

.. _en-us_topic_0000001860198785:

NAMED
=====

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
