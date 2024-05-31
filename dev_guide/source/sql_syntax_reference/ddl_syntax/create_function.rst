:original_name: dws_06_0163.html

.. _dws_06_0163:

CREATE FUNCTION
===============

Function
--------

**CREATE FUNCTION** creates a function.

Precautions
-----------

-  The precision values (if any) of the parameters or return values of a function are not checked.
-  When creating a function, you are advised to explicitly specify the schemas of tables in the function definition. Otherwise, the function may fail to be executed.
-  **current_schema** and **search_path** specified by **SET** during function creation are invalid. **search_path** and **current_schema** before and after function execution should be the same.
-  If a function has output parameters, the **SELECT** statement uses the default values of the output parameters when calling the function. When the **CALL** statement calls the function, it requires that the output parameter values are adapted to Oracle. When the **CALL** statement calls an overloaded PACKAGE function, it can use the default values of the output parameters. For details, see examples in :ref:`CALL <dws_06_0229>`.
-  Only the functions compatible with PostgreSQL or those with the **PACKAGE** attribute can be overloaded. After **REPLACE** is specified, a new function is created instead of replacing a function if the number of parameters, parameter type, or return value is different.
-  You can use the **SELECT** statement to specify different parameters using identical functions, but cannot use the **CALL** statement to call identical functions without the **PACKAGE** attribute because **CALL** aligns with Oracle syntax.
-  When you create a function, you cannot insert other agg functions out of the avg function or other functions.
-  In non-logical cluster mode, return values, parameters, and variables cannot be set to the tables of the Node Groups that are not installed in the system by default. The internal statements of SQL functions cannot be executed on such tables.
-  In logical cluster mode, if return values and parameters of the function are user tables, all the tables must be in the same logical cluster. If the function body involves operations on multiple logical cluster tables, the function cannot be set to **IMMUTABLE** or **SHIPPABLE**, preventing the function from being pushed down to a DN.
-  In logical cluster mode, the parameters and return values of the function cannot use the **%type** to reference a table column type. Otherwise, the function will fail to be created.
-  By default, the permissions to execute new functions are granted to **PUBLIC**. For details, see :ref:`GRANT <dws_06_0250>`. You can revoke the default execution permissions from **PUBLIC** and grant them to other users as needed. To avoid the time window during which new functions can be accessed by all users, create functions in transactions and set function execution permissions.

Syntax
------

-  Syntax (compatible with PostgreSQL) for creating a user-defined function:

   ::

      CREATE [ OR REPLACE  ] FUNCTION function_name
          ( [  { argname [ argmode  ] argtype [  { DEFAULT  | :=  | =  } expression  ]}  [, ...]  ] )
          [ RETURNS rettype [ DETERMINISTIC  ]  | RETURNS TABLE (  { column_name column_type  }  [, ...] )]
          LANGUAGE lang_name
          [
             {IMMUTABLE  | STABLE  | VOLATILE }
              | {SHIPPABLE | NOT SHIPPABLE}
              | WINDOW
              | [ NOT  ] LEAKPROOF
              | {CALLED ON NULL INPUT  | RETURNS NULL ON NULL INPUT | STRICT }
              | {[ EXTERNAL  ] SECURITY INVOKER | [ EXTERNAL  ] SECURITY DEFINER | AUTHID DEFINER  | AUTHID CURRENT_USER}
              | {FENCED | NOT FENCED}
              | {PACKAGE}

              | COST execution_cost
              | ROWS result_rows
              | SET configuration_parameter { {TO | =} value | FROM CURRENT }}
           ][...]
          {
              AS 'definition'
              | AS 'obj_file', 'link_symbol'
          }

-  Oracle syntax of creating a customized function:

   ::

      CREATE [ OR REPLACE  ] FUNCTION function_name
          ( [  { argname [ argmode  ] argtype [  { DEFAULT | := | =  } expression  ] }  [, ...]  ] )
          RETURN rettype [ DETERMINISTIC  ]
          [
              {IMMUTABLE  | STABLE  | VOLATILE }
              | {SHIPPABLE | NOT SHIPPABLE}
              | {PACKAGE}
              | {FENCED | NOT FENCED}
              | [ NOT  ] LEAKPROOF
              | {CALLED ON NULL INPUT | RETURNS NULL ON NULL INPUT | STRICT }
              | {[ EXTERNAL  ] SECURITY INVOKER  | [ EXTERNAL  ] SECURITY DEFINER |
      AUTHID DEFINER | AUTHID CURRENT_USER
      }
              | COST execution_cost
              | ROWS result_rows
              | SET configuration_parameter { {TO | =} value  | FROM CURRENT

          ][...]

          {
            IS  | AS
      } plsql_body
      /

Parameter Description
---------------------

-  **OR REPLACE**

   Redefines an existing function.

-  **function_name**

   Indicates the name of the function to create (optionally schema-qualified).

   Value range: a string. It must comply with the naming convention.

   .. note::

      If the name of the function to be created is the same as that of a system function, you are advised to specify a schema. When invoking a user-defined function, you need to specify a schema. Otherwise, the system preferentially invokes the system function.

-  **argname**

   Indicates the name of a function parameter.

   Value range: a string. It must comply with the naming convention.

-  **argmode**

   Indicates the mode of a parameter.

   Value range: **IN**, **OUT**, **IN OUT**, **INOUT**, and **VARIADIC**. The default value is **IN**. Only the parameter of **OUT** mode can be followed by **VARIADIC**. The parameters of **OUT** and **INOUT** cannot be used in function definition of **RETURNS TABLE**.

   .. note::

      **VARIADIC** specifies parameters of array types.

-  **argtype**

   Indicates the data types of the function's parameters.

-  **expression**

   Indicates the default expression of a parameter.

-  **rettype**

   Indicates the return data type.

   When there is **OUT** or **IN OUT** parameter, the **RETURNS** clause can be omitted. If the clause exists, it must be the same as the result type indicated by the output parameter. If there are multiple output parameters, the value is **RECORD**. Otherwise, the value is the same as the type of a single output parameter.

   The **SETOF** modifier indicates that the function will return a set of items, rather than a single item.

-  **DETERMINISTIC**

   Adapted to Oracle SQL syntax. You are not advised to use it.

-  **column_name**

   Specifies the column name.

-  **column_type**

   Specifies the column type.

-  **definition**

   Specifies a string constant defining the function; the meaning depends on the language. It can be an internal function name, a path pointing to a target file, a SQL query, or text in a procedural language.

-  **LANGUAGE lang_name**

   Indicates the name of the language that is used to implement the function. It can be **SQL**, **internal**, or the name of user-defined process language. To ensure downward compatibility, the name can use single quotation marks. Contents in single quotation marks must be capitalized.

-  **WINDOW**

   Indicates that the function is a window function. The WINDOW attribute cannot be changed when the function definition is replaced.

   .. important::

      For a user-defined window function, the value of **LANGUAGE** can only be **internal**, and the referenced internal function must be a window function.

-  **IMMUTABLE**

   Indicates that the function always returns the same result if the parameter values are the same.

   If the input argument of the function is a constant, the function value is calculated at the optimizer stage. The advantage is that the expression value can be obtained as early as possible, so the cost estimation is more accurate and the execution plan generated is better.

   A user-defined **IMMUTABLE** function is automatically pushed down to DNs for execution, which may cause potential risks. If a function is defined as **IMMUTABLE** but the function execution process is in fact not **IMMUTABLE**, serious problems such as result errors may occur. Therefore, exercise caution when defining the **IMMUTABLE** attribute for a function.

   Examples:

   #. .. _en-us_topic_0000001188110530__li144181920184412:

      If a user-defined function references objects such as tables and views, the function cannot be defined as **IMMUTABLE**, because the function may return different results when the data in a referenced table changes.

   #. .. _en-us_topic_0000001188110530__li1341819203448:

      If a user-defined function references a **STABLE** or **VOLATILE** function, the function cannot be defined as IMMUTABLE.

   #. If a user-defined function contains factors that cannot be pushed down, the function cannot be defined as **IMMUTABLE**, because the **IMMUTABLE** attribute conflicts with factors that cannot be pushed down. Typical scenarios include functions and syntax that cannot be pushed down.

   #. If a user-defined function contains an aggregation operation that will generate **STREAM** plans to complete the operation (meaning that DNs and CNs are involved for results calculation, such as the **LISTAGG** function), the function cannot be defined as **IMMUTABLE**.

   To prevent possible problems, you can set **behavior_compat_options** to **check_function_conflicts** in the database to check definition conflicts. This method can identify the :ref:`1 <en-us_topic_0000001188110530__li144181920184412>` and :ref:`2 <en-us_topic_0000001188110530__li1341819203448>` scenarios described above.

-  **STABLE**

   Indicates that the function cannot modify the database, and that within a single table scan it will consistently return the same result for the same parameter values, but that its result varies by SQL statements.

-  **VOLATILE**

   Indicates that the function value can change even within a single table scan, so no optimizations can be made.

-  **SHIPPABLE**

   **NOT SHIPPABLE**

   Indicates whether the function can be pushed down to DNs for execution.

   -  Functions of the IMMUTABLE type can always be pushed down to the DNs.

   -  Functions of the STABLE or VOLATILE type can be pushed down to DNs only if their attribute is **SHIPPABLE**.

      Exercise caution when defining the **SHIPPABLE** attribute for a function. **SHIPPABLE** means that the entire function will be pushed down to DNs for execution. If the attribute is incorrectly set, serious problems such as result errors may occur.

      Similar to the **IMMUTABLE** attribute, the **SHIPPABLE** attribute has use restrictions. The function cannot contain factors that do not allow the function to be pushed down for execution. If a function is pushed down to a single DN for execution, the function's calculation logic will depend only on the data set of the DN.

      Examples:

      #. If a function references a hash table, you cannot define the function as **SHIPPABLE**.
      #. If a function contains factors, functions, or syntax that cannot be pushed down, the function cannot be defined as SHIPPABLE. For details, see Optimizing Statement Pushdown.
      #. If a function's calculation process involves data across DNs, the function cannot be defined as **SHIPPABLE**. For example, some aggregation operations involve data across DNs.

-  **PACKAGE**

   Indicates whether the function can be overloaded. PostgreSQL-style functions can be overloaded, and this parameter is designed for Oracle-style functions.

   -  All PACKAGE and non-PACKAGE functions cannot be overloaded or replaced.
   -  PACKAGE functions do not support parameters of the VARIADIC type.
   -  The PACKAGE attribute of functions cannot be modified.

-  **LEAKPROOF**

   Indicates that the function has no side effects. **LEAKPROOF** can be set only by the system administrator.

-  **CALLED ON NULL INPUT**

   Declares that some parameters of the function can be invoked in normal mode if the parameter values are **NULL**. This parameter can be omitted.

-  **RETURNS NULL ON NULL INPUT**

   **STRICT**

   Indicates that the function always returns **NULL** whenever any of its parameters are **NULL**. If this parameter is specified, the function is not executed when there are **NULL** parameters; instead a **NULL** result is returned automatically.

   The usage of **RETURNS NULL ON NULL INPUT** is the same as that of **STRICT**.

-  **EXTERNAL**

   The keyword EXTERNAL is allowed for SQL conformance, but it is optional since, unlike in SQL, this feature applies to all functions not only external ones.

-  **SECURITY INVOKER**

   **AUTHID CURRENT_USER**

   Indicates that the function is to be executed with the permissions of the user that calls it. This parameter can be omitted.

   **SECURITY INVOKER** and **AUTHID CURRENT_USER** have the same functions.

-  **SECURITY DEFINER**

   **AUTHID DEFINER**

   Specifies that the function is to be executed with the permissions of the user that created it.

   The usage of **AUTHID DEFINER** is the same as that of **SECURITY DEFINER**.

-  **FENCED**

   **NOT FENCED**

   (Effective only for C functions) Specifies whether functions are executed in fenced mode. In NOT FENCED mode, a function is executed in a CN or DN process. In FENCED mode, a function is executed in a new fork process, which does not affect CN or DN processes.

   Application scenarios:

   -  Develop or debug a function in FENCED mode and execute it in NOT FENCED mode. This reduces the cost of the fork process and communication.
   -  Perform complex OS operations, such as open a file, process signals and threads, in FENCED mode so that GaussDB(DWS) running is not affected.
   -  The default value is **FENCED**.

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

-  **plsql_body**

   Indicates the PL/SQL stored procedure body.

   .. important::

      When the function is creating users, the log will record unencrypted passwords. You are not advised to do it.

Examples
--------

Create the function **func_add_sql** as an SQL query.

::

   DROP FUNCTION IF EXISTS func_add_sql;
   CREATE FUNCTION func_add_sql(integer, integer) RETURNS integer
       AS 'select $1 + $2;'
       LANGUAGE SQL
       IMMUTABLE
       RETURNS NULL ON NULL INPUT;

Create the function **func_increment_plsql** that accepts a parameter name and returns an integer value that is one greater than the input.

::

   DROP FUNCTION IF EXISTS func_increment_plsql;
   CREATE OR REPLACE FUNCTION func_increment_plsql(i integer) RETURNS integer AS $$
           BEGIN
                   RETURN i + 1;
           END;
   $$ LANGUAGE plpgsql;

Create a function that returns the RECORD type.

::

   DROP FUNCTION IF EXISTS compute;
   CREATE OR REPLACE FUNCTION compute(i int, out result_1 bigint, out result_2 bigint)
   returns SETOF RECORD
   as $$
   begin
       result_1 = i + 1;
       result_2 = i * 10;
   return next;
   end;
   $$language plpgsql;

Create a function that that returns multiple output parameters.

::

   DROP FUNCTION IF EXISTS func_dup_sql;
   CREATE FUNCTION func_dup_sql(in int, out f1 int, out f2 text)
       AS $$ SELECT $1, CAST($1 AS text) || ' is text' $$
       LANGUAGE SQL;
   SELECT * FROM func_dup_sql(42);

Create a function that calculates the sum of two integers and gets the result. If the input is null, null will be returned.

::

   DROP FUNCTION IF EXISTS func_add_sql2;
   CREATE FUNCTION func_add_sql2(num1 integer, num2 integer) RETURN integer
   AS
   BEGIN
   RETURN num1 + num2;
   END;
   /

Create an overloaded function with the PACKAGE attribute.

::

   CREATE OR REPLACE FUNCTION package_func_overload(col int, col2  int)
   return integer package
   as
   declare
       col_type text;
   begin
        col := 122;
            dbms_output.put_line('two int parameters ' || col2);
            return 0;
   end;
   /

   CREATE OR REPLACE FUNCTION package_func_overload(col int, col2 smallint)
   return integer package
   as
   declare
       col_type text;
   begin
        col := 122;
            dbms_output.put_line('two smallint parameters ' || col2);
            return 0;
   end;
   /

Helpful Links
-------------

:ref:`ALTER FUNCTION <dws_06_0126>`, :ref:`DROP FUNCTION <dws_06_0193>`
