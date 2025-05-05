:original_name: dws_06_0342.html

.. _dws_06_0342:

System Function Checking Function
=================================

pv_builtin_functions()
----------------------

Description: Queries information about system functions.

Return type: record

pg_get_functiondef(func_oid)
----------------------------

Description: Gets definition of a function.

Return type: text

**func_oid** is the OID of the function, which can be queried in the **PG_PROC** system catalog.

Example: Query the OID and definition of the **justify_days** function.

::

   SELECT oid FROM pg_proc WHERE proname ='justify_days';
    oid
   ------
    1295
   (1 row)

   SELECT * FROM pg_get_functiondef(1295);
    headerlines |                          definition
   -------------+--------------------------------------------------------------
              4 | CREATE OR REPLACE FUNCTION pg_catalog.justify_days(interval)+
                |  RETURNS interval                                           +
                |  LANGUAGE internal                                          +
                |  IMMUTABLE STRICT NOT FENCED NOT SHIPPABLE                  +
                | AS $function$interval_justify_days$function$                +
                |
   (1 row)

Note: The query result returned by **pg_get_functiondef** is in the original text format of the stored procedure. The escape character (\\) is used to facilitate subsequent parsing.

pg_get_function_arguments(func_oid)
-----------------------------------

Description: Gets argument list of function's definition (with default values).

Return type: text

Note: **pg_get_function_arguments** returns the argument list of a function, in the form it would need to appear in within **CREATE FUNCTION**.

pg_get_function_identity_arguments(func_oid)
--------------------------------------------

Description: Gets argument list to identify a function (without default values).

Return type: text

Note: **pg_get_function_identity_arguments** returns the argument list necessary to identify a function, in the form it would need to appear in within **ALTER FUNCTION**. This form omits default values.

pg_get_function_result(func_oid)
--------------------------------

Description: Gets **RETURNS** clause for function.

Return type: text

Note: **pg_get_function_result** returns the appropriate **RETURNS** clause for the function.
