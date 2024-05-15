:original_name: dws_04_0524.html

.. _dws_04_0524:

Variable Definition Statement
=============================

This section describes the declaration of variables in the PL/SQL and the scope of this variable in codes.

Variable Declaration
--------------------

For details about the variable declaration syntax, see :ref:`Figure 1 <en-us_topic_0000001188482218__f3705e8285f024bfe8ab480866c9fb57a>`.

.. _en-us_topic_0000001188482218__f3705e8285f024bfe8ab480866c9fb57a:

.. figure:: /_static/images/en-us_image_0000001188642254.png
   :alt: **Figure 1** declare_variable::=

   **Figure 1** declare_variable::=

The above syntax diagram is explained as follows:

-  **variable_name** indicates the name of a variable.
-  **type** indicates the type of a variable.
-  **value** indicates the initial value of the variable. (If the initial value is not given, NULL is taken as the initial value.) **value** can also be an expression.

Example:

::

   DECLARE
       emp_id  INTEGER := 7788; -- Define a variable and assign a value to it.
   BEGIN
       emp_id := 5*7784; -- Assign a value to the variable.
   END;
   /

In addition to the declaration of basic variable types, **%TYPE** and **%ROWTYPE** can be used to declare variables related to table columns or table structures.

%TYPE Attribute
---------------

%TYPE declares a variable to be of the same data type as a previously declared variable (for example, a column in a table). For example, if you want to define a **my_name** variable that has the same data type as the **firstname** column in the **employee** table, you can define the variable as follows:

.. code-block::

   my_name employee.firstname%TYPE

In this way, you can declare **my_name** even if you do not know the data type of **firstname** in **employee**, and the data type of **my_name** can be automatically updated when the data type of **firstname** changes.

%ROWTYPE Attribute
------------------

%ROWTYPE declares data types of a set of data. It stores a row of table data or results fetched from a cursor. For example, if you want to define a set of data with the same column names and column data types as the **employee** table, you can define the data as follows:

.. code-block::

   my_employee employee%ROWTYPE

.. important::

   If multiple CNs are used, the %ROWTYPE and %TYPE attributes of temporary tables cannot be declared in a stored procedure, because a temporary table is valid only in the current session and is invisible to other CNs in the compilation phase. In this case, a message is displayed indicating that the temporary table does not exist.

Scope of a Variable
-------------------

The scope of a variable indicates the accessibility and availability of a variable in code block. In other words, a variable takes effect only within its scope.

-  To define a function scope, a variable must declare and create a **BEGIN-END** block in the declaration section. The necessity of such declaration is also determined by block structure, which requires that a variable has different scopes and lifetime during a process.
-  A variable can be defined multiple times in different scopes, and inner definition can cover outer one.
-  A variable defined in an outer block can also be used in a nested block. However, the outer block cannot access variables in the nested block.

Example:

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
