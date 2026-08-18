:original_name: dws_06_0168.html

.. _dws_06_0168:

CREATE REDACTION POLICY
=======================

Function
--------

**CREATE REDACTION POLICY** creates a data redaction policy on a database table.

DWS provides a column-level data redaction policy (REDACTION POLICY) feature. For certain sensitive information (such as ID card numbers, mobile numbers, bank card numbers, etc.), the original data is transformed and rewritten by applying redaction functions, achieving reliable protection of sensitive private data and enhancing the product's capabilities in data security and privacy protection.

Precautions
-----------

-  The data redaction feature is controlled by the GUC parameter **feature_support_options**. If the error message "REDACTION POLICY is not yet supported, please add enable_data_redaction into feature_support_options." or "Cannot use the feature of data redaction, please drop existed redaction policy or add enable_data_redaction into feature_support_options." is displayed, the data redaction feature is not enabled. You need to contact technical support to set the **feature_support_options** parameter and enable the data redaction feature.
-  Only the owner of the table object and users granted the **gs_role_redaction** preset role have the permission to create a redaction policy.
-  Users with the **sysadmin** permission can bypass the redaction policy check and always have visibility of the redacted column data, meaning the redaction policy does not take effect for them.
-  Data redaction policies can only be created on regular tables and foreign tables that are not under an **EXTERNAL SCHEMA** (OBS, HDFS, GDS, collaborative analysis foreign tables). Redaction policies cannot be created for system tables, foreign tables under an **EXTERNAL SCHEMA**, temporary tables, UNLOGGED tables, views, or function objects.
-  Table objects and redaction policies have a one-to-one mapping relationship. A redaction policy is a collection of data redaction functions that can be applied to multiple columns in a table. You can set different redaction functions for different columns.
-  A redaction policy is enabled by default upon its creation, that is, the **enable** parameter of the policy is **true** by default.
-  Data redaction policies can be matched with specified roles.
-  Creating a redaction policy on a regular table object through a synonym is not supported.

Syntax
------

::

   CREATE REDACTION POLICY [IF NOT EXISTS] policy_name ON table_name [ { BEFORE | AFTER } old_policy_name ]
       [ WHEN (when_expression) ]
       [ ADD COLUMN column_name WITH redaction_function_name ( [ argument [, ...] ] )] [, ... ];

Parameter Description
---------------------

.. table:: **Table 1** CREATE REDACTION POLICY parameters

   +--------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                | Description                                                                                                                                                                                                                                                             | Value Range                                                                                                                                                                                                                                                                                                                                  |
   +==========================+=========================================================================================================================================================================================================================================================================+==============================================================================================================================================================================================================================================================================================================================================+
   | policy_name              | Specifies the name of a redaction policy.                                                                                                                                                                                                                               | ``-``                                                                                                                                                                                                                                                                                                                                        |
   +--------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_name               | Specifies the name of the table to which the redaction policy is applied.                                                                                                                                                                                               | A valid table name.                                                                                                                                                                                                                                                                                                                          |
   +--------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | BEFORE \| AFTER          | Specifies the relative location where the current policy is created. Generally, one redaction policy is used for one table.                                                                                                                                             | ``-``                                                                                                                                                                                                                                                                                                                                        |
   |                          |                                                                                                                                                                                                                                                                         |                                                                                                                                                                                                                                                                                                                                              |
   |                          | The default value indicates that the current policy is created after the last candidate policy of the target table recorded in the current system catalog.                                                                                                              |                                                                                                                                                                                                                                                                                                                                              |
   +--------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | WHEN ( when_expression ) | Specifies the expression used for the redaction policy to take effect. The redaction policy takes effect only when this expression is true.                                                                                                                             | When a query statement is querying a table where a redaction policy is enabled, the redacted data is invisible in the query only if the WHEN expression for the redaction policy is true. Generally, the WHEN clause is used to specify the users for which the redaction policy takes effect.                                               |
   |                          |                                                                                                                                                                                                                                                                         |                                                                                                                                                                                                                                                                                                                                              |
   |                          |                                                                                                                                                                                                                                                                         | The WHEN clause must comply with the following rules:                                                                                                                                                                                                                                                                                        |
   |                          |                                                                                                                                                                                                                                                                         |                                                                                                                                                                                                                                                                                                                                              |
   |                          |                                                                                                                                                                                                                                                                         | #. The expression can be a combination of multiple subexpressions connected by AND and/or OR.                                                                                                                                                                                                                                                |
   |                          |                                                                                                                                                                                                                                                                         | #. Each subexpression supports only the =, <>, !=, >=, >, <=, and < operators. The left and right operand values can only be constant values or one of the following system constant values: **SESSION_USER**, **CURRENT_USER**, **USER**, **CURRENT_ROLE**, and **CURRENT_SCHEMA** system constants or the **SYS_CONTEXT** system function. |
   |                          |                                                                                                                                                                                                                                                                         | #. Each subexpression can be an IN or NOT IN expression. The value for the left operand can be any of the system constant values listed in rule 2, and each element in the array of the right operand must be a constant value.                                                                                                              |
   |                          |                                                                                                                                                                                                                                                                         | #. Each subexpression can be a :ref:`pg_has_role(user, role, privilege) <en-us_topic_0000001811515573__section13942057572>` system function.                                                                                                                                                                                                 |
   |                          |                                                                                                                                                                                                                                                                         | #. If you want a redaction policy to be valid in all conditions, that is, you want it to take effect on all users (including the table owner), you are advised to use the (1=1) expression to create this policy.                                                                                                                            |
   |                          |                                                                                                                                                                                                                                                                         | #. If the WHEN clause is not specified, the redaction policy is disabled by default. You need to manually specify a WHEN expression for the policy to take effect.                                                                                                                                                                           |
   +--------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | column_name              | Specifies the name of the column on the table to which the redaction policy is applied.                                                                                                                                                                                 | ``-``                                                                                                                                                                                                                                                                                                                                        |
   +--------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | redaction_function_name  | Specifies the name of a redaction function.                                                                                                                                                                                                                             | ``-``                                                                                                                                                                                                                                                                                                                                        |
   +--------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | arguments                | Specifies the list of arguments of the redaction function.                                                                                                                                                                                                              | -  **MASK_NONE**: No redaction is performed.                                                                                                                                                                                                                                                                                                 |
   |                          |                                                                                                                                                                                                                                                                         | -  **MASK_FULL**: indicates that all data is redacted d to a fixed value (for example, a number is converted to 0).                                                                                                                                                                                                                          |
   |                          | DWS supports three built-in redaction functions: **MASK_NONE**, **MASK_FULL**, and **MASK_PARTIAL**. User-defined redaction functions created using C language or PL/pgSQL language are also supported. For details, see :ref:`Data Redaction Functions <dws_06_0064>`. | -  **MASK_PARTIAL**: Performs partial redaction based on the specified character type, numeric type, or time type (e.g., some digits of a mobile number are masked with asterisks (*)).                                                                                                                                                      |
   +--------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example 1: Creating a Redaction Policy for a Specified User
-----------------------------------------------------------

#. Enable the data redaction function.

   Contact technical support to set the **feature_support_options** parameter and enable the data redaction feature.

   .. code-block::

      gs_guc set -Z coordinator -Z datanode -N all -I all -c "feature_support_options=enable_data_redaction"
      gs_om -t stop && gs_om -t start

#. Create users **alice** and **matu**.

   ::

      CREATE ROLE alice PASSWORD '{password}';
      CREATE ROLE matu PASSWORD '{password}';

#. Create the table object **emp** as user **alice**, and insert data into the table.

   ::

      CREATE TABLE emp(id int, name varchar(20), salary NUMERIC(10,2));
      INSERT INTO emp VALUES(1, 'July', 1230.10), (2, 'David', 999.99);

#. View the table data.

   ::

      SELECT * FROM emp;

   |image1|

#. Create the redaction policy **mask_emp** for the **emp** table as user **alice** and make the **salary** column invisible to user **matu**.

   ::

      CREATE REDACTION POLICY mask_emp ON emp WHEN(current_user = 'matu') ADD COLUMN salary WITH mask_full(salary);

#. Grant the **SELECT** permission on the **emp** table to user **matu** as user **alice**.

   ::

      GRANT SELECT ON emp TO matu;

#. Switch to user **matu**.

   ::

      SET ROLE matu PASSWORD '{password}';

#. Query the table **emp** as user **matu**. Data in the **salary** column is redacted.

   ::

      SELECT * FROM emp;

   |image2|

Example 2: Creating a Redaction Policy for a Specified Role
-----------------------------------------------------------

#. Create the role **redact_role**.

   ::

      CREATE ROLE redact_role PASSWORD '{password}';

#. Add users **matu** and **alice** to the role **redact_role**.

   ::

      GRANT redact_role to matu,alice;

#. Create a table object **emp1** as the administrator and insert data.

   ::

      CREATE TABLE emp1(id int, name varchar(20), salary NUMERIC(10,2));
      INSERT INTO emp1 VALUES(3, 'Rose', 2230.20), (4, 'Jack', 899.88);

#. View the data in the table **emp1**.

   ::

      SELECT * FROM emp1;

   |image3|

#. Create a redaction policy **mask_emp1** for the table object **emp1** as the administrator to make the **salary** column invisible to role **redact_role**.

   ::

      CREATE REDACTION POLICY mask_emp1 ON emp1 WHEN(pg_has_role(current_user, 'redact_role', 'member')) ADD COLUMN salary WITH mask_full(salary);

   If no user is specified, the current user (**current_user**) is used by default.

   ::

      CREATE REDACTION POLICY mask_emp1 ON emp1 WHEN (pg_has_role('redact_role', 'member')) ADD COLUMN salary WITH mask_full(salary);

#. Grant the **SELECT** permission on the **emp1** table to user **matu** as the administrator.

   ::

      GRANT SELECT ON emp1 TO matu;

#. Switch to user **matu**.

   ::

      SET ROLE matu PASSWORD '{password}';

#. Query the table **emp** as user **matu**. Data in the **salary** column is redacted.

   ::

      SELECT * FROM emp1;

   |image4|

Helpful Links
-------------

:ref:`ALTER REDACTION POLICY <dws_06_0132>`, :ref:`DROP REDACTION POLICY <dws_06_0199>`

.. |image1| image:: /_static/images/en-us_image_0000002583981178.png
.. |image2| image:: /_static/images/en-us_image_0000002583986126.png
.. |image3| image:: /_static/images/en-us_image_0000002583833820.png
.. |image4| image:: /_static/images/en-us_image_0000002583834358.png
