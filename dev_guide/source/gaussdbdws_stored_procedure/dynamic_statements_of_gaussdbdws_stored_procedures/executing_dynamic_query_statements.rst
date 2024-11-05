:original_name: dws_04_0528.html

.. _dws_04_0528:

Executing Dynamic Query Statements
==================================

You can perform dynamic queries using **EXECUTE IMMEDIATE** or **OPEN FOR** in GaussDB(DWS). **EXECUTE IMMEDIATE** dynamically executes **SELECT** statements and **OPEN FOR** combines use of cursors. If you need to store query results in a data set, use **OPEN FOR**.

EXECUTE IMMEDIATE
-----------------

:ref:`Figure 1 <en-us_topic_0000001510402645__f5bdde6e248a14512a954fb2a0389a90c>` shows the syntax diagram.

.. _en-us_topic_0000001510402645__f5bdde6e248a14512a954fb2a0389a90c:

.. figure:: /_static/images/en-us_image_0000001510284113.png
   :alt: **Figure 1** EXECUTE IMMEDIATE dynamic_select_clause::=

   **Figure 1** EXECUTE IMMEDIATE dynamic_select_clause::=

:ref:`Figure 2 <en-us_topic_0000001510402645__fca15d616a3114294949e5b8ed8367c56>` shows the syntax diagram for **using_clause**.

.. _en-us_topic_0000001510402645__fca15d616a3114294949e5b8ed8367c56:

.. figure:: /_static/images/en-us_image_0000001460723300.png
   :alt: **Figure 2** using_clause-1

   **Figure 2** using_clause-1

The above syntax diagram is explained as follows:

-  **define_variable**: specifies variables to store single-line query results.
-  **USING IN bind_argument**: specifies where the variable passed to the dynamic SQL value is stored, that is, in the dynamic placeholder of **dynamic_select_string**.
-  **USING OUT bind_argument**: specifies where the dynamic SQL returns the value of the variable.

   .. important::

      -  In query statements, **INTO** and **OUT** cannot coexist.
      -  A placeholder name starts with a colon (:) followed by digits, characters, or strings, corresponding to *bind_argument* in the **USING** clause.
      -  *bind_argument* can only be a value, variable, or expression. It cannot be a database object such as a table name, column name, and data type. That is, *bind_argument* cannot be used to transfer schema objects for dynamic SQL statements. If a stored procedure needs to transfer database objects through *bind_argument* to construct dynamic SQL statements (generally, DDL statements), you are advised to use double vertical bars (||) to concatenate *dynamic_select_clause* with a database object.
      -  A dynamic PL/SQL block allows duplicate placeholders. That is, a placeholder can correspond to only one *bind_argument* in the **USING** clause.

**Example**

::

   --Retrieve values from dynamic statements (INTO clause).
   DECLARE
      staff_count  VARCHAR2(20);
   BEGIN
      EXECUTE IMMEDIATE 'select count(*) from staffs'
         INTO staff_count;
      dbms_output.put_line(staff_count);
   END;
   /

   --Pass and retrieve values (the INTO clause is used before the USING clause).
   CREATE OR REPLACE PROCEDURE dynamic_proc
   AS
      staff_id     NUMBER(6) := 200;
      first_name   VARCHAR2(20);
      salary       NUMBER(8,2);
   BEGIN
      EXECUTE IMMEDIATE 'select first_name, salary from staffs where staff_id = :1'
          INTO first_name, salary
          USING IN staff_id;
      dbms_output.put_line(first_name || ' ' || salary);
   END;
   /

   -- Invoke the stored procedure.
   CALL dynamic_proc();

   -- Delete the stored procedure.
   DROP PROCEDURE dynamic_proc;

OPEN FOR
--------

Dynamic query statements can be executed by using **OPEN FOR** to open dynamic cursors.

For details about the syntax, see :ref:`Figure 3 <en-us_topic_0000001510402645__f6e232247a15f4a7d816e4748bab655ec>`.

.. _en-us_topic_0000001510402645__f6e232247a15f4a7d816e4748bab655ec:

.. figure:: /_static/images/en-us_image_0000001510403025.png
   :alt: **Figure 3** open_for::=

   **Figure 3** open_for::=

Parameter description:

-  **cursor_name**: specifies the name of the cursor to be opened.
-  **dynamic_string**: specifies the dynamic query statement.
-  **USING** *value*: applies when a placeholder exists in dynamic_string.

For use of cursors, see :ref:`GaussDB(DWS) Stored Procedure Cursor <dws_04_0545>`.

**Example**

::

   DECLARE
       name          VARCHAR2(20);
       phone_number  VARCHAR2(20);
       salary        NUMBER(8,2);
       sqlstr        VARCHAR2(1024);

       TYPE app_ref_cur_type IS REF CURSOR; -- Define the cursor type.
       my_cur app_ref_cur_type; -- Define the cursor variable.

   BEGIN
       sqlstr := 'select first_name,phone_number,salary from staffs
            where section_id = :1';
       OPEN my_cur FOR sqlstr USING '30'; -- Open the cursor. using is optional.
       FETCH my_cur INTO name, phone_number, salary; -- Retrieve the data.
       WHILE my_cur%FOUND LOOP
             dbms_output.put_line(name||'#'||phone_number||'#'||salary);
             FETCH my_cur INTO name, phone_number, salary;
       END LOOP;
       CLOSE my_cur; -- Close the cursor.
   END;
   /
