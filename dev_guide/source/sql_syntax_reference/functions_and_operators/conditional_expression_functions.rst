:original_name: dws_06_0050.html

.. _dws_06_0050:

Conditional Expression Functions
================================


Conditional Expression Functions
--------------------------------

-  coalesce(expr1, expr2, ..., exprn)

   Description: Returns the first argument that is not **NULL** in the argument list.

   **COALESCE(expr1, expr2)** is equivalent to **CASE WHEN expr1 IS NOT NULL THEN expr1 ELSE expr2 END**.

   For example:

   ::

      SELECT coalesce(NULL,'hello');
       coalesce
      ----------
       hello
      (1 row)

   Note:

   -  **NULL** is returned only if all parameters are **NULL**.
   -  This value is replaced by the default value when data is displayed.
   -  Like a CASE expression, COALESCE only evaluates the parameters that are needed to determine the result. That is, parameters to the right of the first non-null parameter are not evaluated.

-  decode(base_expr, compare1, value1, Compare2,value2, ... default)

   Description: Compares base_expr with each compare(n) and returns value(n) if they are matched. If base_expr does not match each **compare(n)**, the default value is returned.

   For example:

   ::

      SELECT decode('A','A',1,'B',2,0);
       case
      ------
       1
      (1 row)

-  if(bool_expr, expr1, expr2)

   Description: Returns **expr1** or **expr2**. If the value of **bool_expr** is **true**, **expr1** is returned. Otherwise, **expr2** is returned.

   This function is equivalent to **CASE WHEN bool_expr = true THEN expr1 ELSE expr2 END**.

   Example:

   ::

      SELECT if(1 < 2, 'yes', 'no');
       if
      -----
       yes
      (1 row)

   Note: **expr1** and **expr2** can be of any type. For details about the available types, see :ref:`UNION, CASE, and Related Constructs <dws_06_0080>`.

-  ifnull(expr1, expr2)

   Description: Returns **expr1** or **expr2**. If **expr1** is not **NULL**, **expr1** is returned. Otherwise, **expr2** is returned.

   This function is logically equivalent to **CASE WHEN expr1 IS NOT NULL THEN expr1 ELSE expr2 END**.

   Example:

   ::

      SELECT ifnull(NULL,'hello');
       ifnull
      --------
       hello
      (1 row)

   Note: **expr1** and **expr2** can be of any type. For details about the available types, see :ref:`UNION, CASE, and Related Constructs <dws_06_0080>`.

-  isnull(expr)

   Description: Checks whether **expr** is **NULL**. If it is **NULL**, **true** is returned. Otherwise, **false** is returned.

   This function is logically equivalent to **expr IS NULL**.

   Example:

   ::

      SELECT isnull(NULL), isnull('abc');
       isnull | isnull
      --------+--------
       t      | f
      (1 row)

-  nullif(expr1, expr2)

   Description: Returns **NULL** or **expr1**. If **expr1** is equal to **expr2**, **NULL** is returned. Otherwise, **expr1** is returned.

   **nullif(expr1, expr2)** is equivalent to **CASE WHEN expr1 = expr2 THEN NULL ELSE expr1 END**.

   For example:

   ::

      SELECT nullif('hello','world');
       nullif
      --------
       hello
      (1 row)

   Note:

   Assume the two parameter data types are different:

   -  If implicit conversion exists between the two data types, implicitly convert the parameter of lower priority to this data type using the data type of higher priority. If the conversion succeeds, computation is performed. Otherwise, an error is returned. For example:

      ::

         SELECT nullif('1234'::VARCHAR,123::INT4);
          nullif
         --------
            1234
         (1 row)

      ::

         SELECT nullif('1234'::VARCHAR,'2012-12-24'::DATE);
         ERROR:  invalid input syntax for type timestamp: "1234"

   -  If implicit conversion is not applied between two data types, an error is displayed. For example:

      ::

         SELECT nullif(TRUE::BOOLEAN,'2012-12-24'::DATE);
         ERROR:  operator does not exist: boolean = timestamp without time zone
         LINE 1: SELECT nullif(TRUE::BOOLEAN,'2012-12-24'::DATE) FROM DUAL;
         ^
         HINT:  No operator matches the given name and argument type(s). You might need to add explicit type casts.

-  nvl( expr1 , expr2 )

   Returns **expr1** or **expr2**. If **expr1** is **NULL**, **expr2** is returned. Otherwise, **expr1** is returned.

   For example:

   ::

      SELECT nvl('hello','world');
        nvl
      -------
       hello
      (1 row)

   Parameters expr1 and expr2 can be of any data type. If expr1 and expr2 are of different data types, NVL checks whether expr2 can be implicitly converted to expr1. If it can, the expr1 data type is returned. If epr2 cannot be implicitly converted to expr1 but epr1 can be implicitly converted to expr2, the expr2 data type is returned. If no implicit type conversion exists between the two parameters and the parameters are different data types, an error is reported.

-  sys_context( 'namespace' , 'parameter')

   Description: Obtains and returns the parameter values of a specified **namespace**.

   Return type: VARCHAR

   For example:

   ::

      SELECT sys_context('USERENV', 'CURRENT_SCHEMA');
       sys_context
      -------------
       public
      (1 row)

   The result varies according to the current actual schema.

   Note: Currently, only the following formats are supported: SYS_CONTEXT('USERENV', 'CURRENT_SCHEMA') and SYS_CONTEXT('USERENV', 'CURRENT_USER').

-  greatest(expr1 [, ...])

   Description: Selects the largest value from a list of any number of expressions.

   Return type:

   For example:

   ::

      SELECT greatest(1*2,2-3,4-1);
       greatest
      ----------
              3
      (1 row)

   ::

      SELECT greatest('ABC', 'BCD', 'CDE');
       greatest
      ----------
       CDE
      (1 row)

-  least(expr1 [, ...])

   Description: Selects the smallest value from a list of any number of expressions.

   For example:

   ::

      SELECT least(1*2,2-3,4-1);
       least
      -------
          -1
      (1 row)

   ::

      SELECT least('ABC','BCD','CDE');
       least
      --------
       ABC
      (1 row)

-  EMPTY_BLOB()

   Description: Initiates a BLOB variable in an INSERT or an UPDATE statement to a NULL value.

   Return type: BLOB

   For example:

   ::

      -- Create a table:
      CREATE TABLE blob_tb(b blob,id int) DISTRIBUTE BY REPLICATION;
      -- Insert data:
      INSERT INTO blob_tb VALUES (empty_blob(),1);
      --Delete the table.
      DROP TABLE blob_tb;

   Note: The length is 0 obtained using **DBMS.GETLENGTH** in a parallel mode.
