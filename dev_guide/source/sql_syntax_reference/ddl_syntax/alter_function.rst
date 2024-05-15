:original_name: dws_06_0126.html

.. _dws_06_0126:

ALTER FUNCTION
==============

Function
--------

**ALTER FUNCTION** modifies the attributes of a customized function.

Precautions
-----------

Only the owner of a function or a system administrator can run this statement. The user who wants to change the owner of a function must be a direct or indirect member of the new owner role. If a function involves operations on temporary tables, the **ALTER FUNCTION** cannot be used.

Syntax
------

-  Modify the additional parameter of the customized function:

   ::

      ALTER FUNCTION function_name ( [ { [ argmode ] [ argname ] argtype} [, ...] ] )
          action [ ... ] [ RESTRICT ];

   The syntax of the **action** clause is as follows:

   ::

      {CALLED ON NULL INPUT | RETURNS NULL ON NULL INPUT | STRICT}
       | {IMMUTABLE | STABLE | VOLATILE}
       | {SHIPPABLE | NOT SHIPPABLE}
       | {NOT FENCED | FENCED}
       | [ NOT ] LEAKPROOF
       | { [ EXTERNAL ] SECURITY INVOKER | [ EXTERNAL ] SECURITY DEFINER }
       | AUTHID { DEFINER | CURRENT_USER }
       | COST execution_cost
       | ROWS result_rows
       | SET configuration_parameter { { TO | = } { value | DEFAULT }| FROM CURRENT}
       | RESET {configuration_parameter | ALL}

-  Modify the name of the customized function:

   ::

      ALTER FUNCTION funname ( [ { [ argmode ] [ argname ] argtype} [, ...] ] )
          RENAME TO new_name;

-  Modify the owner of the customized function:

   ::

      ALTER FUNCTION funname ( [ { [ argmode ] [ argname ] argtype} [, ...] ] )
          OWNER TO new_owner;

-  Modify the schema of the customized function:

   ::

      ALTER FUNCTION funname ( [ { [ argmode ] [ argname ] argtype} [, ...] ] )
          SET SCHEMA new_schema;

Parameter Description
---------------------

-  **function_name**

   Specifies the function name to be modified.

   Value range: An existing function name.

-  **argmode**

   Specifies whether a parameter is an input or output parameter.

   Value range: **IN**, **OUT**, **IN OUT**

-  **argname**

   Indicates the parameter name.

   Value range: A string. It must comply with the naming convention.

-  **argtype**

   Specifies the parameter type.

   Value range: A valid type. For details, see :ref:`Data Types <dws_06_0008>`.

-  **CALLED ON NULL INPUT**

   Declares that some parameters of the function can be invoked in normal mode if the parameter values are **NULL**. By default, the usage is the same as specifying the parameters.

-  **RETURNS NULL ON NULL INPUT**

   **STRICT**

   Indicates that the function always returns **NULL** whenever any of its arguments are **NULL**. If this parameter is specified, the function is not executed when there are null arguments; instead a null result is assumed automatically.

   The usage of **RETURNS NULL ON NULL INPUT** is the same as that of **STRICT**.

-  **IMMUTABLE**

   Indicates that the function always returns the same result if the parameter values are the same.

-  **STABLE**

   Indicates that the function cannot modify the database, and that within a single table scan it will consistently return the same result for the same parameter values, but that its result varies by SQL statements.

-  **VOLATILE**

   Indicates that the function value can change in one table scanning and no optimization is performed.

-  **SHIPPABLE**

   **NOT SHIPPABLE**

   Indicates whether the function can be pushed down to DNs for execution.

   -  Functions of the IMMUTABLE type can always be pushed down to the DNs.
   -  Functions of the STABLE or VOLATILE type can be pushed down to DNs only if their attribute is **SHIPPABLE**.

-  **LEAKPROOF**

   Indicates that the function has no side effect and specifies that the parameter includes only the returned value. **LEAKPROOF** can be set only by a system administrator.

-  (Optional) **EXTERNAL**

   The objective is to be compatible with SQL. This feature applies to all functions, including external functions.

-  **SECURITY INVOKER**

   **AUTHID CURREN_USER**

   Declares that the function will be executed according to the permission of the user that invokes it. By default, the usage is the same as specifying the parameters.

   **SECURITY INVOKER** and **AUTHID CURREN_USER** have the same functions.

-  **SECURITY DEFINER**

   **AUTHID DEFINER**

   Specifies that the function is to be executed with the permissions of the user that created it.

   The usage of **AUTHID DEFINER** is the same as that of **SECURITY DEFINER**.

-  **COST execution_cost**

   A positive number giving the estimated execution cost for the function.

   The unit of **execution_cost** is cpu_operator_cost.

   Value range: A positive number.

-  **ROWS result_rows**

   Estimates the number of rows returned by the function. This is only allowed when the function is declared to return a set.

   Value range: A positive number. The default is 1000 rows.

-  **configuration_parameter**

   -  **value**

      Sets a specified database session parameter to a specified value. If the value is **DEFAULT** or **RESET**, the default setting is used in the new session. **OFF** closes the setting.

      Value range: a string

      -  DEFAULT
      -  OFF
      -  RESET

      Specifies the default value.

   -  **from current**

      Uses the value of **configuration_parameter** of the current session.

-  **new_name**

   Specifies the new name of a function. To change a function's schema, you must also have the CREATE permission on the new schema.

   Value range: A string. It must comply with the naming convention.

-  **new_owner**

   Specifies the new owner of a function. To alter the owner, the new owner must also be a direct or indirect member of the new owning role, and that role must have CREATE permission on the function's schema.

   Value range: Existing user roles.

-  **new_schema**

   Specifies the new schema of a function.

   Value range: Existing schemas.

Example
-------

Create a function that calculates the sum of two integers and returns the result. If the input is null, null will be returned.

::

   DROP FUNCTION IF EXISTS func_add_sql2;
   CREATE FUNCTION func_add_sql2(num1 integer, num2 integer) RETURN integer
   AS
   BEGIN
   RETURN num1 + num2;
   END;
   /

Alter the execution rule of function add to IMMUTABLE (that is, the same result is returned if the parameter remains unchanged):

::

   ALTER FUNCTION func_add_sql2(INTEGER, INTEGER) IMMUTABLE;

Rename the **func_add_sql2** function as **add_two_number**:

::

   ALTER FUNCTION func_add_sql2(INTEGER, INTEGER) RENAME TO add_two_number;

Change the owner of function **add_two_number** to **dbadmin**:

::

   ALTER FUNCTION add_two_number(INTEGER, INTEGER) OWNER TO dbadmin;

Helpful Links
-------------

:ref:`CREATE FUNCTION <dws_06_0163>`, :ref:`DROP FUNCTION <dws_06_0193>`
