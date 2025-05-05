:original_name: dws_06_0168.html

.. _dws_06_0168:

CREATE REDACTION POLICY
=======================

Function
--------

**CREATE REDACTION POLICY** creates a data redaction policy for a table.

Precautions
-----------

-  Only the table object owner and users who have been granted the **gs_redaction_policy** preset role are authorized to create data masking policies.
-  Data masking policies can be created for regular tables and non-EXTERNAL-SCHEMA foreign tables (such as OBS, HDFS, GDS, and foreign tables for coordinated analysis), but not for system catalogs, EXTERNAL SCHEMA foreign tables, temporary tables, UNLOGGED tables, views, or functions.
-  Synonyms cannot be used to create redaction policies for ordinary table objects.
-  Table objects and redaction policies have a one-to-one mapping relationship. A redaction policy is a collection of data redaction functions that can be applied to multiple columns in a table. You can set different redaction functions for different columns.
-  A redaction policy is enabled by default upon its creation, that is, the **enable** parameter of the policy is **true** by default.
-  Redaction policies do not take effect on users with the **sysadmin** permission. Data in the redacted columns is always visible to such users.
-  Data redaction policies can be matched with specified roles.

Syntax
------

::

   CREATE REDACTION POLICY policy_name ON table_name [ { AFTER | BEFORE } old_policy_name ]
       [INHERIT]
       [ WHEN (when_expression) ]
       [ ADD COLUMN column_name WITH redaction_function_name ( [ argument [, ...] ] )] [, ... ];

Parameter Description
---------------------

-  **policy_name**

   Specifies the name of a redaction policy.

-  **table_name**

   Specifies the name of the table to which the redaction policy is applied.

-  **AFTER \| BEFORE**

   Specifies the relative location where the current policy is created. Generally, one masking policy is used for one table. By default, the current policy is created after the last candidate policy of the target table recorded in the current system catalog.

-  **INHERIT**

   Specifies whether the masking policy is inherited from other masking policies. This parameter is not recommended.

-  **WHEN ( when_expression )**

   Specifies the expression used for the redaction policy to take effect. The redaction policy takes effect only when this expression is true.

   .. note::

      When a query statement is querying a table where a redaction policy is enabled, the redacted data is invisible in the query only if the WHEN expression for the redaction policy is true. Generally, the WHEN clause is used to specify the users for which the redaction policy takes effect.

      The WHEN clause must comply with the following rules:

      #. The expression can be a combination of multiple subexpressions connected by AND and/or OR.
      #. Each subexpression supports only the =, <>, !=, >=, >, <=, and < operators. The left and right operand values can only be constant values or one of the following system constant values: **SESSION_USER**, **CURRENT_USER**, **USER**, **CURRENT_ROLE**, and **CURRENT_SCHEMA** system constants or the **SYS_CONTEXT** system function.
      #. Each subexpression can be an IN or NOT IN expression. The value for the left operand can be any of the system constant values listed in rule 2, and each element in the array of the right operand must be a constant value.
      #. Each subexpression can be a :ref:`pg_has_role(user, role, privilege) <en-us_topic_0000001811515573__section13942057572>` system function.
      #. If you want a redaction policy to be valid in all conditions, that is, you want it to take effect on all users (including the table owner), you are advised to use the (1=1) expression to create this policy.
      #. If the WHEN clause is not specified, the redaction policy is disabled by default. You need to manually specify a WHEN expression for the policy to take effect.

-  **column_name**

   Specifies the name of the table column to which the redaction policy is applied.

-  **function_name**

   Specifies the redaction function applied to the specified table column.

-  **arguments**

   Specifies the list of arguments of the redaction function.

   -  **MASK_NONE**: indicates that no masking is performed.
   -  **MASK_FULL**: indicates that all data is masked to a fixed value.
   -  **MASK_PARTIAL**: indicates that partial masking is performed based on the specified character type, numeric type, or time type.

      .. note::

         You can use the built-in masking functions **MASK_NONE**, **MASK_FULL**, and **MASK_PARTIAL**, or create your own masking functions by using the C language or PL/pgSQL. For details, see :ref:`Data Redaction Functions <dws_06_0064>`.

Examples
--------

**Create redaction policy for a specified user.**

#. Create users **alice** and **matu**:

   ::

      CREATE ROLE alice PASSWORD '{password}';
      CREATE ROLE matu PASSWORD '{password}';

#. Create a table object **emp** as user **alice**, and insert data into the table.

   ::

      CREATE TABLE emp(id int, name varchar(20), salary NUMERIC(10,2));
      INSERT INTO emp VALUES(1, 'July', 1230.10), (2, 'David', 999.99);

#. Create a redaction policy **mask_emp** for the **emp** table as user **alice** to make the **salary** column invisible to user **matu**.

   ::

      CREATE REDACTION POLICY mask_emp ON emp WHEN(current_user = 'matu') ADD COLUMN salary WITH mask_full(salary);

#. Grant the **SELECT** permission on the **emp** table to user **matu** as user **alice**.

   ::

      GRANT SELECT ON emp TO matu;

#. Switch to user **matu**.

   ::

      SET ROLE matu PASSWORD '{password}';

#. Query the **emp** table. Data in the **salary** column has been redacted.

   ::

      SELECT * FROM emp;

**Create redaction policy for the role.**

#. Create a role **redact_role**.

   ::

      CREATE ROLE redact_role PASSWORD '{password}';

#. Add users **matu** and **alice** to the role **redact_role**.

   ::

      GRANT redact_role to matu,alice;

#. Create a table object **emp1** as the administrator and insert data.

   ::

      CREATE TABLE emp1(id int, name varchar(20), salary NUMERIC(10,2));
      INSERT INTO emp1 VALUES(3, 'Rose', 2230.20), (4, 'Jack', 899.88);

#. Create a redaction policy **mask_emp1** for the table object **emp1** as the administrator to make the **salary** column invisible to role **redact_role**.

   ::

      CREATE REDACTION POLICY mask_emp1 ON emp1 WHEN(pg_has_role(current_user, 'redact_role', 'member')) ADD COLUMN salary WITH mask_full(salary);

   If no user is specified, the current user (**current_user**) is used by default.

   ::

      CREATE REDACTION POLICY mask_emp1 ON emp1 WHEN (pg_has_role('redact_role', 'member')) ADD COLUMN salary WITH mask_full(salary);

#. The administrator grants the SELECT permission on the table **emp1** to the user **matu**.

   ::

      GRANT SELECT ON emp1 TO matu;

#. Switch to user **matu**.

   ::

      SET ROLE matu PASSWORD '{password}';

#. Query the **emp** table. Data in the **salary** column has been redacted.

   ::

      SELECT * FROM emp1;

Helpful Links
-------------

:ref:`ALTER REDACTION POLICY <dws_06_0132>`, :ref:`DROP REDACTION POLICY <dws_06_0199>`
