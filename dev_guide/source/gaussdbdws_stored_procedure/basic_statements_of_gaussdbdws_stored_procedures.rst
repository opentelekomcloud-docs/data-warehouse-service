:original_name: dws_04_0523.html

.. _dws_04_0523:

Basic Statements of GaussDB(DWS) Stored Procedures
==================================================

Variable Definition Statement
-----------------------------

This section describes the declaration of variables in the PL/SQL and the scope of this variable in codes.

**Variable declaration**

For details about the variable declaration syntax, see :ref:`Figure 1 <en-us_topic_0000001811609505__f3705e8285f024bfe8ab480866c9fb57a>`.

.. _en-us_topic_0000001811609505__f3705e8285f024bfe8ab480866c9fb57a:

.. figure:: /_static/images/en-us_image_0000002040329244.png
   :alt: **Figure 1** declare_variable::=

   **Figure 1** declare_variable::=

The syntax is described as follows:

-  **variable_name** indicates the name of a variable.
-  **type** indicates the type of a variable.
-  **value** indicates the initial value of the variable. (If the initial value is not given, **NULL** is taken as the initial value.) **value** can also be an expression.

**Examples**

::

   DECLARE
       emp_id  INTEGER := 7788; -- Define a variable and assign a value to it.
   BEGIN
       emp_id := 5*7784; -- Assign a value to the variable.
   END;
   /

In addition to the declaration of basic variable types, **%TYPE** and **%ROWTYPE** can be used to declare variables related to table columns or table structures.

**%TYPE attribute**

**%TYPE** declares a variable to be of the same data type as a previously declared variable (for example, a column in a table). For example, if you want to define a **my_name** variable that has the same data type as the **firstname** column in the **employee** table, you can define the variable as follows:

.. code-block::

   my_name employee.firstname%TYPE

In this way, you can declare **my_name** even if you do not know the data type of **firstname** in **employee**, and the data type of **my_name** can be automatically updated when the data type of **firstname** changes.

**%ROWTYPE attribute**

**%ROWTYPE** declares data types of a set of data. It stores a row of table data or results fetched from a cursor. For example, if you want to define a set of data with the same column names and column data types as the **employee** table, you can define the data as follows:

.. code-block::

   my_employee employee%ROWTYPE

.. important::

   If multiple CNs are used, the **%ROWTYPE** and **%TYPE** attributes of temporary tables cannot be declared in a stored procedure, because a temporary table is valid only in the current session and is invisible to other CNs in the compilation phase. In this case, a message is displayed indicating that the temporary table does not exist.

**Variable scope**

The scope of a variable indicates the accessibility and availability of a variable in code block. In other words, a variable takes effect only within its scope.

-  To define a function scope, a variable must declare and create a **BEGIN-END** block in the declaration section. The necessity of such declaration is also determined by block structure, which requires that a variable has different scopes and lifetime during a process.
-  A variable can be defined multiple times in different scopes, and inner definition can cover outer one.
-  A variable defined in an outer block can also be used in a nested block. However, the outer block cannot access variables in the nested block.

**Examples**

::

   DECLARE
       emp_id  INTEGER :=7788; -- Define a variable and assign a value to it.
       outer_var  INTEGER :=6688; -- Define a variable and assign a value to it.
   BEGIN
       DECLARE
           emp_id INTEGER :=7799; -- Define a variable and assign a value to it.
           inner_var  INTEGER :=6688; -- Define a variable and assign a value to it.
       BEGIN
           dbms_output.put_line('inner emp_id ='||emp_id); -- Display the value as 7799.
           dbms_output.put_line('outer_var ='||outer_var); -- Cite variables of an outer block.
       END;
       dbms_output.put_line('outer emp_id ='||emp_id); -- Display the value as 7788.
   END;
   /

Assignment Statement
--------------------

**Syntax**

:ref:`Figure 2 <en-us_topic_0000001811609505__fbc103680b1964ce3bf8a16e5e9076438>` shows the syntax diagram for assigning a value to a variable.

.. _en-us_topic_0000001811609505__fbc103680b1964ce3bf8a16e5e9076438:

.. figure:: /_static/images/en-us_image_0000002040170940.png
   :alt: **Figure 2** assignment_value::=

   **Figure 2** assignment_value::=

The syntax is described as follows:

-  **variable_name** indicates the name of a variable.
-  **value** can be a value or an expression. The type of **value** must be compatible with the type of **variable_name**.

**Examples**

::

   DECLARE
       emp_id  INTEGER := 7788; --Assignment
   BEGIN
       emp_id := 5; --Assignment
       emp_id := 5*7784;
   END;
   /

Call Statement
--------------

**Syntax**

:ref:`Figure 3 <en-us_topic_0000001811609505__f299fb9795429468ea45fe86a41dbca6e>` shows the syntax diagram for calling a clause.

.. _en-us_topic_0000001811609505__f299fb9795429468ea45fe86a41dbca6e:

.. figure:: /_static/images/en-us_image_0000002076330085.png
   :alt: **Figure 3** call_clause::=

   **Figure 3** call_clause::=

The syntax is described as follows:

-  **procedure_name** specifies the name of a stored procedure.
-  **parameter** specifies the parameters for the stored procedure. You can set no parameter or multiple parameters.

**Examples**

::

   -- Create the stored procedure proc_staffs:
   CREATE OR REPLACE PROCEDURE proc_staffs
   (
   section     NUMBER(6),
   salary_sum out NUMBER(8,2),
   staffs_count out INTEGER
   )
   IS
   BEGIN
   SELECT sum(salary), count(*) INTO salary_sum, staffs_count FROM staffs where section_id = section;
   END;
   /

   -- Create the stored procedure proc_return:
   CREATE OR REPLACE PROCEDURE proc_return
   AS
   v_num NUMBER(8,2);
   v_sum INTEGER;
   BEGIN
   proc_staffs(30, v_sum, v_num);  --Invoke a statement:
   dbms_output.put_line(v_sum||'#'||v_num);
   RETURN;   --Return a statement.
   END;
   /

   -- Invoke a stored procedure proc_return:
   CALL proc_return();

   -- Delete a stored procedure:
   DROP PROCEDURE proc_staffs;
   DROP PROCEDURE proc_return;

   --Create the function func_return.
   CREATE OR REPLACE FUNCTION func_return returns void
   language plpgsql
   AS $$
   DECLARE
   v_num INTEGER := 1;
   BEGIN
   dbms_output.put_line(v_num);
   RETURN;  --Return a statement.
   END $$;


   -- Invoke the function func_return.
   CALL func_return();
   1

   -- Delete the function:
   DROP FUNCTION func_return;
