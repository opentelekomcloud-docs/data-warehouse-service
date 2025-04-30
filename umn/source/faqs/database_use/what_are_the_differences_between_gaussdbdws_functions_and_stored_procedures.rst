:original_name: dws_03_0002.html

.. _dws_03_0002:

What Are the Differences Between GaussDB(DWS) Functions and Stored Procedures?
==============================================================================

Functions and stored procedures are two common objects in database management systems. They have similarities and differences in implementing specific functions. Understanding their characteristics and application scenarios is important for properly designing the database structure and improving database performance.

.. table:: **Table 1** Differences between functions and stored procedures

   +----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Function                                                                                                                                                                   | Stored procedures                                                                                                                                                                                                           |
   +============================================================================================================================================================================+=============================================================================================================================================================================================================================+
   | Both can be used to implement specific functions. Both functions and stored procedures can encapsulate a series of SQL statements to complete certain specific operations. |                                                                                                                                                                                                                             |
   +----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Both can receive input parameters and perform corresponding operations based on the parameters.                                                                            |                                                                                                                                                                                                                             |
   +----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | The identifier of a function is **FUNCTION**.                                                                                                                              | The identifier of the stored procedure is **PROCEDURE**.                                                                                                                                                                    |
   +----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | A function must return a specific value of the specified numeric type.                                                                                                     | A stored procedure can have no return value, one return value, or multiple return values. You can use output parameters to return results or directly use the SELECT statement in a stored procedure to return result sets. |
   +----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Functions are used to return single values, for example, a number calculation result, a string processing result, or a table.                                              | Stored procedures are used for DML operations, for example, inserting, updating, and deleting data in batches.                                                                                                              |
   +----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

-  **Creating and Invoking a Function**

   Create the **emp** table and insert data into the table. The table data is as follows:

   ::

      SELECT * FROM emp;
       empno | ename |   job    | mgr  |      hiredate       |   sal   |  comm  | deptno
      -------+-------+----------+------+---------------------+---------+--------+--------
        7369 | SMITH | CLERK    | 7902 | 1980-12-17 00:00:00 |  800.00 |        |     20
        7499 | ALLEN | SALESMAN | 7698 | 1981-02-20 00:00:00 | 1600.00 | 300.00 |     30
        7566 | JONES | MANAGER  | 7839 | 1981-04-02 00:00:00 | 2975.00 |        |     20
        7521 | WARD  | SALESMAN | 7698 | 1981-02-22 00:00:00 | 1250.00 | 500.00 |     30
      (4 rows)

   Create the **emp_comp** function to accept two numbers as input and return the calculated value.

   ::

      CREATE OR REPLACE FUNCTION emp_comp (
          p_sal           NUMBER,
          p_comm          NUMBER
      ) RETURN NUMBER
      IS
      BEGIN
          RETURN (p_sal + NVL(p_comm, 0)) * 24;
      END;
      /

   Run the **SELECT** command to invoke the function:

   ::

      SELECT ename "Name", sal "Salary", comm "Commission", emp_comp(sal, comm) "Total Compensation" FROM emp;
       Name  | Salary  | Commission | Total Compensation
      -------+---------+------------+--------------------
       SMITH |  800.00 |            |           19200.00
       ALLEN | 1600.00 |     300.00 |           45600.00
       JONES | 2975.00 |            |           71400.00
       WARD  | 1250.00 |     500.00 |           42000.00
      (4 rows)

-  **Creating and Invoking a Stored Procedure**

   Create the **MATCHES** table and insert data into the table. The table data is as follows:

   ::

      SELECT * FROM MATCHES;
       matchno | teamno | playerno | won | lost
      ---------+--------+----------+-----+------
             1 |      1 |        6 |   3 |    1
             7 |      1 |       57 |   3 |    0
             8 |      1 |        8 |   0 |    3
             9 |      2 |       27 |   3 |    2
            11 |      2 |      112 |   2 |    3
      (5 rows)

   Create the stored procedure **delete_matches** to delete all matches that a specified player participates in.

   ::

      CREATE PROCEDURE delete_matches(IN p_playerno INTEGER)
      AS
      BEGIN
         DELETE FROM MATCHES WHERE playerno = p_playerno;
      END;
      /

   Invoke the stored procedure **delete_matches**.

   ::

      CALL delete_matches(57);

   Query the **MATCHES** table again. The returned result indicates that the data of the player whose **playerno** is **57** has been deleted.

   ::

      SELECT * FROM MATCHES;
       matchno | teamno | playerno | won | lost
      ---------+--------+----------+-----+------
            11 |      2 |      112 |   2 |    3
             8 |      1 |        8 |   0 |    3
             1 |      1 |        6 |   3 |    1
             9 |      2 |       27 |   3 |    2
      (4 rows)
