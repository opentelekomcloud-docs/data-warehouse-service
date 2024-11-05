:original_name: dws_04_0972.html

.. _dws_04_0972:

Rules for Using GaussDB(DWS) PL/pgSQL
=====================================

General Principles
------------------

#. Development shall strictly comply with design documents.
#. Program modules shall be highly cohesive and loosely coupled.
#. Proper, comprehensive troubleshooting measures shall be developed.
#. Code shall be reasonable and clear.
#. Program names shall comply with a unified naming rule.
#. Fully consider the program efficiency, including the program execution efficiency and database query and storage efficiency. Use efficient and effective processing methods.
#. Program comments shall be detailed, correct, and standard.
#. The commit or rollback operation shall be performed at the end of a stored procedure, unless otherwise required by applications.
#. Programs shall support 24/7 processing. In the case of an interruption, the applications shall provide secure, easy-to-use resuming features.
#. Application output shall be standard and simple. The output shall show the progress, error description, and execution results for application maintenance personnel, and provide clear and intuitive reports and documents for business personnel.

Programming Principles
----------------------

#. Use bound variables in SQL statements in the PL/pgSQL.
#. **RETURNING** is recommended for SQL statements in PL/pgSQL.
#. Principles for using stored procedures:

   a. Do not use more than 50 output parameters of the Varchar or Varchar2 type in a stored procedure.
   b. Do not use the LONG type for input or output parameters.
   c. Use the CLOB type for output strings that exceed 10 MB.

#. Variable declaration principles:

   a. Use **%TYPE** to declare a variable that has the same meaning as that of a column or variable in an application table.
   b. Use **%ROWTYPE** to declare a record that has the same meaning as that of a row in an application table.
   c. Each line of a variable declaration shall contain only one statement.
   d. Do not declare variables of the LONG type.

#. Principles for using cursors:

   a. Explicit cursors shall be closed after being used.
   b. A cursor variable shall be closed after being used. If the cursor variable needs to transfer data to an invoked application, the cursor shall be closed in the application. If the cursor variable is used only in a stored procedure, the cursor shall be closed explicitly.
   c. Before using **DBMS_SQL.CLOSE_CURSOR** to close a cursor, use **DBMS_SQL.IS_OPEN** to check whether the cursor is open.

#. Principles for collections:

   a. You are advised to use the **FOR ALL** statement instead of the **FOR** loop statement to reference elements in a collection.

#. Principles for using dynamic statements:

   a. Dynamic SQL shall not be used in the transaction programs of online systems.
   b. Dynamic SQL statements can be used to implement DDL statements and system control commands in PL/pgSQL.
   c. Variable binding is recommended.

#. Principles for assembling SQL statements:

   a. You are advised to use bound variables to assemble SQL statements.
   b. If the conditions for assembling SQL statements contain external input sources, the characters in the input conditions shall be checked to prevent attacks.
   c. In a PL/pgSQL script, the length of a single line of code cannot exceed 2499 characters.

#. Principles for using triggers:

   a. Triggers can be used to implement availability design in scenarios where differential data logs are irrelevant to service processing.
   b. Do not use triggers to implement service processing functions.

Exception Handling Principles
-----------------------------

Any error that occurs in a PL/pgSQL function aborts the execution of the function and related transactions. You can use a **BEGIN** block with an **EXCEPTION** clause to catch and fix errors.

#. In a PL/pgSQL block, if an SQL statement cannot return a definite result, you are advised to handle exceptions (if any) in **EXCEPTION**. Otherwise, unhandled errors may be transferred to the external block and cause program logic errors.
#. You can directly use the exceptions that have been defined in the system. GaussDB(DWS) does not support custom exceptions.
#. A block containing an **EXCEPTION** clause is more expensive to enter and exit than a block without one. Therefore, do not use **EXCEPTION** without need.

Writing Standard
----------------

#. Variable naming rules:

   a. The input parameter format of a procedure or function is **IN\_**\ *Parameter_name*. The parameter name shall be in uppercase.
   b. The output parameter format of a procedure or function is **OUT\_**\ *Parameter_name*. The parameter name shall be in uppercase.
   c. The format for input and output parameters in a procedure or function is **IO\_**\ *Parameter name*, with the parameter name written in uppercase.
   d. When creating variables for procedures and functions, use the format **v\_**\ *Variable name*, with the variable name written in lowercase.
   e. In query concatenation, the concatenation variable name of the **WHERE** statement shall be **v_where**, and the concatenation variable name of the **SELECT** statement shall be **v_select**.
   f. The record type (TYPE) name shall consist of **T** and a variable name. The name shall be in uppercase.
   g. A cursor name shall consist of **CUR** and a variable name. The name shall be in uppercase.
   h. The name of a reference cursor (REF CURSOR) shall consist of **REF** and a variable name. The name shall be in uppercase.

#. Rules for defining variable types:

   a. Use **%TYPE** to declare the type of a variable that has the same meaning as that of a column in an application table.
   b. Use **%ROWTYPE** to declare the type of a record that has the same meaning as that of a row in an application table.

#. Rules for writing comments:

   a. Comments shall be meaningful and shall not just repeat the code content.
   b. Comments shall be concise and easy to understand.
   c. Comments shall be provided at the beginning of each stored procedure or function. The comments shall contain a brief function description, author, compilation date, program version number, and program change history. The format of the comments at the beginning of stored procedures shall be the same.
   d. Comments shall be provided next to the input and output parameters to describe the meaning of variables.
   e. Comments shall be provided at the beginning of each block or large branch to briefly describe the function of the block. If an algorithm is used, comments shall be provided to describe the purpose and result of the algorithm.

#. Variable declaration format:

   Each line shall contain only one statement. To assign initial values, write them in the same line.

#. Letter case:

   Use uppercase letters except for variable names.

#. Indentation:

   In the statements used for creating a stored procedure, the keywords **CREATE**, **AS/IS**, **BEGIN**, and **END** at the same level shall have the same indent.

#. Statement rules:

   a. For statements that define variables, Each line shall contain only one statement.
   b. The keywords **IF**, **ELSE IF**, **ELSE**, and **END** at the same level shall have the same indent.
   c. The keywords **CASE** and **END** shall have the same indent. The keywords **WHEN** and **ELSE** shall be indented.
   d. The keywords **LOOP** and **END LOOP** at the same level shall have the same indent. Nested statements or statements at lower levels shall have more indent.
