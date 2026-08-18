:original_name: dws_06_0132.html

.. _dws_06_0132:

ALTER REDACTION POLICY
======================

Function
--------

**ALTER REDACTION POLICY** modifies an existing data redactioon policy in a database table, including making the policy take effect or expire, and adding, deleting, or modifying the policy name and column.

Precautions
-----------

Only the table object owner and users with the **gs_role_redaction** preset role can modify the redaction policy.

Syntax
------

-  Modify the expression used for a redaction policy to take effect.

   ::

      ALTER REDACTION POLICY policy_name ON table_name WHEN (when_expression);

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

      [INHERIT] ADD COLUMN column_name WITH redaction_function_name ( [ argument [, ...] ] )
        | [INHERIT] MODIFY COLUMN column_name WITH redaction_function_name ( [ argument [, ...] ] )
        | DROP COLUMN column_name

Parameter Description
---------------------

.. table:: **Table 1** ALTER REDACTION POLICY parameters

   +-------------------------+-----------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+
   | Parameter               | Description                                                                                                                 | Value Range                                                                                                      |
   +=========================+=============================================================================================================================+==================================================================================================================+
   | policy_name             | Specifies the name of the redaction policy to be modified.                                                                  | Name of an existing redaction policy.                                                                            |
   +-------------------------+-----------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+
   | table_name              | Specifies the name of the table to which the redaction policy is applied.                                                   | Name of an existing table.                                                                                       |
   +-------------------------+-----------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+
   | when_expression         | Specifies the new expression used for the redaction policy to take effect.                                                  | ``-``                                                                                                            |
   +-------------------------+-----------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+
   | ENABLE \| DISABLE       | Specifies whether to enable or disable the current redaction policy.                                                        | ``-``                                                                                                            |
   |                         |                                                                                                                             |                                                                                                                  |
   |                         | -  **ENABLE**: makes the data redacting policy of the table take effect again.                                              |                                                                                                                  |
   |                         | -  **DISABLE**:makes the data redaction policy applied to the table invalid.                                                |                                                                                                                  |
   +-------------------------+-----------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+
   | new_policy_name         | Specifies the new name of the redaction policy.                                                                             | A string compliant with the :ref:`identifier naming rules <en-us_topic_0000001811634529__section1475018612353>`. |
   +-------------------------+-----------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+
   | column_name             | Specifies the name of the table column to which the redaction policy is applied.                                            | ``-``                                                                                                            |
   |                         |                                                                                                                             |                                                                                                                  |
   |                         | -  To add a column, use a column name that has not been bound to any redaction functions.                                   |                                                                                                                  |
   |                         | -  To modify a column, use the name of an existing column.                                                                  |                                                                                                                  |
   |                         | -  To delete a column, use the name of an existing column.                                                                  |                                                                                                                  |
   +-------------------------+-----------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+
   | redaction_function_name | Specifies the name of a redaction function.                                                                                 | For details about the supported functions, see :ref:`Data Redaction Functions <dws_06_0064>`.                    |
   +-------------------------+-----------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+
   | arguments               | Specifies the argument list of the redaction function.                                                                      | For details about the supported functions, see :ref:`Data Redaction Functions <dws_06_0064>`.                    |
   |                         |                                                                                                                             |                                                                                                                  |
   |                         | -  **MASK_NONE**: No redaction is performed.                                                                                |                                                                                                                  |
   |                         | -  **MASK_FULL**: indicates that all data is redacted to a fixed value.                                                     |                                                                                                                  |
   |                         | -  **MASK_PARTIAL**: indicates that data of the specified character type, numeric type, or time type is partially redacted. |                                                                                                                  |
   +-------------------------+-----------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+

Examples
--------

Enable the data redaction function.

Contact technical support to set the **feature_support_options** parameter and enable the data redaction feature.

.. code-block::

   gs_guc set -Z coordinator -Z datanode -N all -I all -c "feature_support_options=enable_data_redaction"
   gs_om -t stop && gs_om -t start

Create a user named **test_role** and an example table named **emp**, and insert data into the table.

::

   CREATE ROLE test_role PASSWORD '{Password}';

::

   DROP TABLE IF EXISTS emp;
   CREATE TABLE emp(id int, name varchar(20), salary NUMERIC(10,2));
   INSERT INTO emp VALUES(1, 'July', 1230.10), (2, 'David', 999.99);

Define a redaction policy **mask_emp** on the **emp** table that hides the **salary** column from the user **test_role**.

::

   CREATE REDACTION POLICY mask_emp ON emp WHEN(current_user = 'test_role') ADD COLUMN salary WITH mask_full(salary);

Modify the expression for the data redaction policy to take effect for the specified role. (If no user is specified, the policy takes effect for the current user by default.)

::

   ALTER REDACTION POLICY mask_emp ON emp WHEN (pg_has_role(current_user, 'redact_role', 'member'));
   ALTER REDACTION POLICY mask_emp ON emp WHEN (pg_has_role('redact_role', 'member'));

Modify the expression for the redaction policy to take effect for all users.

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

Use the redaction function **MASK_FULL** to fully mask the data in the **name** column.

::

   ALTER REDACTION POLICY mask_emp_new ON emp MODIFY COLUMN name WITH mask_full(name);

Delete an existing column where the redaction policy is used.

::

   ALTER REDACTION POLICY mask_emp_new ON emp DROP COLUMN name;

Helpful Links
-------------

:ref:`CREATE REDACTION POLICY <dws_06_0168>` and :ref:`DROP REDACTION POLICY <dws_06_0199>`
