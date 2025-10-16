:original_name: dws_06_0170.html

.. _dws_06_0170:

CREATE PROCEDURE
================

Function
--------

**CREATE PROCEDURE** creates a stored procedure.

Precautions
-----------

-  Function creation also applies to stored procedures. For details, see :ref:`CREATE FUNCTION <dws_06_0163>`.

-  The precision values (if any) of the parameters or return values of a stored procedure are not checked.
-  When creating a stored procedure, you are advised to display the specified schema for the operations on the table objects in the stored procedure definition. Otherwise, the stored procedure may fail to be executed.
-  **current_schema** and **search_path** specified by **SET** during stored procedure creation are invalid. **search_path** and **current_schema** before and after function execution should be the same.
-  If a stored procedure has output parameters, the **SELECT** statement uses the default values of the output parameters when calling the procedure. When the **CALL** statement calls the stored procedure, it requires that the output parameter values are adapted to Oracle. When the **CALL** statement calls a non-overloaded function, output parameters must be specified. When the **CALL** statement calls an overloaded PACKAGE function, it can use the default values of the output parameters. For details, see examples in :ref:`CALL <dws_06_0229>`.
-  A stored procedure with the PACKAGE attribute can use overloaded functions.
-  When you create a procedure, you cannot insert aggregate functions or other functions out of the average function.
-  In a cluster with multiple CNs, the input or output parameters of a stored procedure cannot be set to the temporary table type. This is because when a stored procedure is created on a CN that is not currently connected, the correct temporary schema cannot be obtained based on the table name, resulting in an incorrect table type.
-  After a stored procedure is created, the definition of **CREATE FUNCTION** is returned when the definition is queried.
-  A stored procedure can have multiple return values or no return value. After a stored procedure without a return value is invoked, an empty command output is displayed.
-  Stored procedures cannot be submitted by segment.

Syntax
------

::

   CREATE [ OR REPLACE ] PROCEDURE procedure_name
       [ ( {[ argmode ] [ argname ] argtype [ { DEFAULT | := | = } expression ]}[,...]) ]
       [
          { IMMUTABLE | STABLE | VOLATILE }
          | { SHIPPABLE | NOT SHIPPABLE }
          | {PACKAGE}
          | [ NOT ] LEAKPROOF
          | { CALLED ON NULL INPUT | RETURNS NULL ON NULL INPUT | STRICT }
          | {[ EXTERNAL ] SECURITY INVOKER | [ EXTERNAL ] SECURITY DEFINER | AUTHID DEFINER | AUTHID CURRENT_USER}
          | COST execution_cost
          | ROWS result_rows
          | SET configuration_parameter { [ TO | = ] value | FROM CURRENT }
       ][ ... ]
    { IS | AS }
   plsql_body
   /

Parameter Description
---------------------

-  **OR REPLACE**

   Replaces the original definition when two stored procedures are with the same name.

-  **procedure_name**

   Specifies the name of the stored procedure that is created (optionally with schema names).

   Value range: a string. It must comply with the naming convention.

-  **argmode**

   Specifies the mode of an argument.

   .. important::

      **VARIADIC** specifies arguments of array types.

   Value range: **IN**, **OUT**, **IN OUT**, **INOUT**, and **VARIADIC**. The default value is **IN**. Only the argument of **OUT** mode can be followed by **VARIADIC**. The parameters of **OUT** and **INOUT** cannot be used in procedure definition of **RETURNS TABLE**.

-  **argname**

   Specifies the name of an argument.

   Value range: a string. It must comply with the naming convention.

-  **argtype**

   Specifies the type of a parameter.

   Value range: A valid data type.

   .. note::

      There is no strict requirement on the sequence of **argname** and **argmode**. The following order is advised: **argname**, **argmode**, and **argtype** in sequence.

-  **IMMUTABLE, STABLE, ...**

   Specifies a constraint. Parameters here are similar to those of **CREATE FUNCTION**. For details, see :ref:`5.18.17.13-CREATE FUNCTION <dws_06_0163>`.

-  **plsql_body**

   Indicates the PL/SQL stored procedure body.

   .. important::

      When you create a user, or perform other operations requiring password input in a stored procedure, the system catalog and csv log records the unencrypted password. Therefore, you are advised not to perform such operations in the stored procedure.

Examples
--------

Create a stored procedure:

::

   CREATE OR REPLACE PROCEDURE prc_add
   (
       param1    IN   INTEGER,
       param2    IN OUT  INTEGER
   )
   AS
   BEGIN
      param2:= param1 + param2;
      dbms_output.put_line('result is: '||to_char(param2));
   END;
   /

Call the stored procedure:

::

   CALL prc_add(2,3);

Create a stored procedure whose parameter type is VARIADIC:

::

   CREATE OR REPLACE PROCEDURE pro_variadic (param1 VARIADIC int4[],param2 OUT TEXT)
   AS
   BEGIN
       param2:= param1::text;
   END;
   /

Execute the stored procedure:

::

   SELECT pro_variadic(VARIADIC param1=> array[1,2,3,4]);

Create a stored procedure with the **package** attribute:

::

   CREATE OR REPLACE PROCEDURE package_func_overload(col int, col2 out varchar)
   package
   AS
   DECLARE
       col_type text;
   BEGIN
        col2 := '122';
            dbms_output.put_line('two varchar parameters ' || col2);
   END;
   /

Helpful Links
-------------

:ref:`DROP PROCEDURE <dws_06_0201>`, :ref:`CALL <dws_06_0229>`
