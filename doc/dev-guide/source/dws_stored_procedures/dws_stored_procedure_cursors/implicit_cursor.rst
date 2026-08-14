:original_name: dws_04_0548.html

.. _dws_04_0548:

Implicit Cursor
===============

The system automatically sets implicit cursors for non-query statements, such as **ALTER** and **DROP**, and creates work areas for these statements. These implicit cursors are named SQL, which is defined by the system.

Overview
--------

Implicit cursor operations, such as definition, opening, value-grant, and closing, are automatically performed by the system. Users can use only the attributes of implicit cursors to complete operations. The data stored in the work area of an implicit cursor is the latest SQL statement, and is not related to the user-defined explicit cursors.

**Format call**: SQL%

.. note::

   **INSERT**, **UPDATE**, **DROP**, and **SELECT** statements do not require defined cursors.

Attributes
----------

An implicit cursor has the following attributes:

-  **SQL%FOUND**: Boolean attribute, which returns **TRUE** if the last fetch returns a row.
-  **SQL%NOTFOUND**: Boolean attribute, which works opposite to the **SQL%FOUND** attribute.
-  **SQL%ROWCOUNT**: numeric attribute, which returns the number of records fetched from the cursor.
-  **SQL%ISOPEN**: Boolean attribute, whose value is always **FALSE**. Close implicit cursors immediately after an SQL statement is executed.

Examples
--------

::

   -- Delete all employees in a department from the EMP table. If the department has no employees, delete the department from the DEPT table.
   CREATE TABLE staffs_t1 AS TABLE staffs;
   CREATE TABLE sections_t1 AS TABLE sections;

   CREATE OR REPLACE PROCEDURE proc_cursor3()
   AS
       DECLARE
       V_DEPTNO NUMBER(4) := 100;
       BEGIN
           DELETE FROM staffs WHERE section_ID = V_DEPTNO;
           -- Proceed based on cursor status:
           IF SQL%NOTFOUND THEN
           DELETE FROM sections_t1 WHERE section_ID = V_DEPTNO;
           END IF;
       END;
   /

   CALL proc_cursor3();

   -- Drop the stored procedure and the temporary table:
   DROP PROCEDURE proc_cursor3;
   DROP TABLE staffs_t1;
   DROP TABLE sections_t1;
