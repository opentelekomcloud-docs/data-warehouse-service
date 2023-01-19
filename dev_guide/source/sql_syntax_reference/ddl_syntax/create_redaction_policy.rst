:original_name: dws_06_0168.html

.. _dws_06_0168:

CREATE REDACTION POLICY
=======================

Function
--------

**CREATE REDACTION POLICY** creates a data redaction policy for a table.

Precautions
-----------

-  Only the table owner has the permission to create a data redaction policy.
-  You can create data redaction policies only for ordinary tables. Redaction policies are unavailable to system catalogs, HDFS tables, foreign tables, temporary tables, UNLOGGED tables, views, and functions.
-  Synonyms cannot be used to create redaction policies for ordinary table objects.
-  Table objects and redaction policies have a one-to-one mapping relationship. A redaction policy is a collection of data redaction functions that can be applied to multiple columns in a table. You can set different redaction functions for different columns.
-  A redaction policy is enabled by default upon its creation, that is, the **enable** parameter of the policy is **true** by default.
-  Redaction policies do not take effect on users with the **sysadmin** permission. Data in the redacted columns is always visible to such users.

Syntax
------

::

   CREATE REDACTION POLICY policy_name ON table_name
       [ WHEN (when_expression) ]
       [ ADD COLUMN column_name WITH redaction_function_name ( [ argument [, ...] ] )] [, ... ];

Parameter Description
---------------------

-  **policy_name**

   Specifies the name of a redaction policy.

-  **table_name**

   Specifies the name of the table to which the redaction policy is applied.

-  **WHEN ( when_expression )**

   Specifies the expression used for the redaction policy to take effect. The redaction policy takes effect only when this expression is true.

   .. note::

      When a query statement is querying a table where a redaction policy is enabled, the redacted data is invisible in the query only if the WHEN expression for the redaction policy is true. Generally, the WHEN clause is used to specify the users for which the redaction policy takes effect.

      The WHEN clause must comply with the following rules:

      #. The expression can be a combination of multiple subexpressions connected by AND and/or OR.
      #. Each subexpression supports only the =, <>, !=, >=, >, <=, and < operators. The left and right operand values can only be constant values or one of the following system constant values: **SESSION_USER**, **CURRENT_USER**, **USER**, **CURRENT_ROLE**, and **CURRENT_SCHEMA** system constants or the **SYS_CONTEXT** system function.
      #. Each subexpression can be an IN or NOT IN expression. The value for the left operand can be any of the system constant values listed in rule 2, and each element in the array of the right operand must be a constant value.
      #. If you want a redaction policy to be valid in all conditions, that is, you want it to take effect on all users (including the table owner), you are advised to use the (1=1) expression to create this policy.
      #. If the WHEN clause is not specified, the redaction policy is disabled by default. You need to manually specify a WHEN expression for the policy to take effect.

-  **column_name**

   Specifies the name of the table column to which the redaction policy is applied.

-  **function_name**

   Specifies the redaction function applied to the specified table column.

-  **arguments**

   Specifies the list of arguments of the redaction function.

   .. note::

      The system provides three built-in redaction functions: **MASK_NONE**, **MASK_FULL**, and **MASK_PARTIAL**. For details about the function specifications, see :ref:`Data Redaction Functions <dws_06_0064>`. You can also define your own redaction functions, which must comply with the following rules:

      #. In addition to the redaction format, only one column can be specified in the argument list for data redaction.
      #. The return type must be the same as the data type of the redacted column.
      #. The functions can be pushed down.
      #. The functions only implement the formatting for specific data types and do not involve complex association operations with other table objects.

      Built-in redaction functions can cover common redaction scenarios of sensitive information. Therefore, you are advised to use built-in redaction functions to create redaction policies.

Examples
--------

Create a table object **emp** as user **alice**, and insert data into the table.

::

   CREATE TABLE emp(id int, name varchar(20), salary NUMERIC(10,2));
   INSERT INTO emp VALUES(1, 'July', 1230.10), (2, 'David', 999.99);

Create a redaction policy **mask_emp** for the **emp** table as user **alice** to make the **salary** column invisible to user **matu**.

::

   CREATE REDACTION POLICY mask_emp ON emp WHEN(current_user = 'matu') ADD COLUMN salary WITH mask_full(salary);

Grant the **SELECT** permission on the **emp** table to user **matu** as user **alice**.

::

   GRANT SELECT ON emp TO matu;

Switch to user **matu**.

::

   SET ROLE matu PASSWORD '{password}';

Query the **emp** table. Data in the **salary** column has been redacted.

::

   SELECT * FROM emp;

Helpful Links
-------------

:ref:`ALTER REDACTION POLICY <dws_06_0132>`, :ref:`DROP REDACTION POLICY <dws_06_0199>`
