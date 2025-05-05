:original_name: dws_06_0229.html

.. _dws_06_0229:

CALL
====

Function
--------

**CALL** calls defined functions or stored procedures.

Precautions
-----------

If the name of a user-defined function is the same as that of a system function, you need to specify a schema when invoking the user-defined function. Otherwise, the system preferentially invokes the system function.

Syntax
------

::

   CALL [schema.] {func_name| procedure_name} ( param_expr );

Parameter Description
---------------------

-  **schema**

   Specifies the name of the schema where a function or stored procedure is located.

-  **func_name**

   Specifies the name of the function or stored procedure to be called.

   Value range: an existing function name

-  **param_expr**

   Specifies a list of parameters in the function. Use **:=** or **=>** to separate a parameter name and its value. This method allows parameters to be placed in any order. If only parameter values are in the list, the value order must be the same as that defined in the function or stored procedure.

   Value range: names of existing function or stored procedure parameters

   .. note::

      The parameters include input parameters (whose name and type are separated by **IN**) and output parameters (whose name and type are separated by **OUT**). When you run the **CALL** statement to call a function or stored procedure, the parameter list must contain an output parameter for non-overloaded functions. You can set the output parameter to a variable or any constant. For details, see :ref:`Examples <en-us_topic_0000001764675218__s573ee29c39fc4f11b27b21a603ec6b59>`. For an overloaded package function, the parameter list can have no output parameter, but the function may not be found. If an output parameter is contained, it must be a constant.

.. _en-us_topic_0000001764675218__s573ee29c39fc4f11b27b21a603ec6b59:

Examples
--------

Create the **func_add_sql** function to compute the sum of two integers and return the result:

::

   CREATE FUNCTION func_add_sql(num1 integer, num2 integer) RETURN integer
   AS
   BEGIN
   RETURN num1 + num2;
   END;
   /

Transfer based on parameter values:

::

   CALL func_add_sql(1, 3);

Transfer based on the naming flags:

::

   CALL func_add_sql(num1 => 1,num2 => 3);
   CALL func_add_sql(num2 := 2, num1 := 3);

Delete a function:

::

   DROP FUNCTION func_add_sql;

Create a function with output parameters:

::

   CREATE FUNCTION func_increment_sql(num1 IN integer, num2 IN integer, res OUT integer)
   RETURN integer
   AS
   BEGIN
   res := num1 + num2;
   END;
   /

Set output parameters to constants:

::

   CALL func_increment_sql(1,2,1);

Set output parameters to variables:

::

   DECLARE
   res int;
   BEGIN
   func_increment_sql(1, 2, res);
   dbms_output.put_line(res);
   END;
   /

Create overloaded functions:

::

   create or replace procedure package_func_overload(col int, col2 out int) package
   as
   declare
       col_type text;
   begin
        col := 122;
            dbms_output.put_line('two out parameters ' || col2);
   end;
   /

::

   create or replace procedure package_func_overload(col int, col2 out varchar)
   package
   as
   declare
       col_type text;
   begin
        col2 := '122';
            dbms_output.put_line('two varchar parameters ' || col2);
   end;
   /

Call a function:

::

   call package_func_overload(1, 'test');
   call package_func_overload(1, 1);

Delete a function:

::

   DROP FUNCTION func_increment_sql;
