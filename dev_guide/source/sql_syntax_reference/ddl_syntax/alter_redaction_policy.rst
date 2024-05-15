:original_name: dws_06_0132.html

.. _dws_06_0132:

ALTER REDACTION POLICY
======================

Function
--------

**ALTER REDACTION POLICY** modifies a data redaction policy applied to a specified table.

Precautions
-----------

Only the owner of the table to which the redaction policy is applied has the permission to modify the redaction policy.

Syntax
------

-  Modify the expression used for a redaction policy to take effect.

   ::

      ALTER REDACTION POLICY policy_name ON table_name WHEN (new_when_expression);

-  Enable or disable a redaction policy.

   ::

      ALTER REDACTION POLICY policy_name ON table_name ENABLE | DISABLE;

-  Rename a redaction policy.

   ::

      ALTER REDACTION POLICY policy_name ON table_name RENAME TO new_policy_name;

-  Add, modify, or delete a column on which the redaction policy is used.

   ::

      ALTER REDACTION POLICY policy_name ON table_name
          action;

   There are several clauses of **action**:

   ::

      ADD COLUMN column_name WITH function_name ( arguments )
        | MODIFY COLUMN column_name WITH function_name ( arguments )
        | DROP COLUMN column_name

Parameter Description
---------------------

-  **policy_name**

   Specifies the name of the redaction policy to be modified.

-  **table_name**

   Specifies the name of the table to which the redaction policy is applied.

-  **new_when_expression**

   Specifies the new expression used for the redaction policy to take effect.

-  **ENABLE \| DISABLE**

   Specifies whether to enable or disable the current redaction policy.

   -  ENABLE

      Enables the redaction policy that was previously disabled for the table.

   -  DISABLE

      Disables the redaction policy currently applied to the table.

-  **new_policy_name**

   Specifies the new name of the redaction policy.

-  **column_name**

   Specifies the name of the table column to which the redaction policy is applied.

   To add a column, use a column name that has not been bound to any redaction functions.

   To modify a column, use the name of an existing column.

   To delete a column, use the name of an existing column.

-  **function_name**

   Specifies the name of a redaction function.

-  **arguments**

   Specifies the list of arguments of the redaction function.

   -  MASK_NONE: indicates that no masking is performed.
   -  MASK_FULL: indicates that all data is masked to a fixed value.
   -  MASK_PARTIAL: indicates that partial masking is performed based on the specified character type, numeric type, or time type.

Examples
--------

Create a user named **test_role** and an example table named **emp**, and insert data into the table.

::

   CREATE ROLE test_role PASSWORD '{Password}';

::

   DROP TABLE IF EXISTS emp;
   CREATE TABLE emp(id int, name varchar(20), salary NUMERIC(10,2));
   INSERT INTO emp VALUES(1, 'July', 1230.10), (2, 'David', 999.99);

Define a masking policy **mask_emp** on the **emp** table that hides the **salary** column from the **user test**\ \_role.

::

   CREATE REDACTION POLICY mask_emp ON emp WHEN(current_user = 'test_role') ADD COLUMN salary WITH mask_full(salary);

Modify the expression for a redaction policy to make it take effect for the specified role (If no user is specified, the redaction policy takes effect for the current user by default.):

::

   ALTER REDACTION POLICY mask_emp ON emp WHEN (pg_has_role(current_user, 'redact_role', 'member'));
   ALTER REDACTION POLICY mask_emp ON emp WHEN (pg_has_role('redact_role', 'member'));

Modify the expression for the data redaction policy to make it take effect for all users.

::

   ALTER REDACTION POLICY mask_emp ON emp WHEN (1=1);

Disable the redaction policy.

::

   ALTER REDACTION POLICY mask_emp ON emp DISABLE;

Enable the redaction policy again.

::

   ALTER REDACTION POLICY mask_emp ON emp ENABLE;

Change the redaction policy name to **mask_emp_new**.

::

   ALTER REDACTION POLICY mask_emp ON emp RENAME TO mask_emp_new;

Add a column with the redaction policy used.

::

   ALTER REDACTION POLICY mask_emp_new ON emp ADD COLUMN name WITH mask_partial(name, '*', 1, length(name));

Modify the redaction policy for the **name** column. Use the **MASK_FULL** function to redact all data in the **name** column.

::

   ALTER REDACTION POLICY mask_emp_new ON emp MODIFY COLUMN name WITH mask_full(name);

Delete an existing column where the redaction policy is used.

::

   ALTER REDACTION POLICY mask_emp_new ON emp DROP COLUMN name;

Helpful Links
-------------

:ref:`CREATE REDACTION POLICY <dws_06_0168>`, :ref:`DROP REDACTION POLICY <dws_06_0199>`
