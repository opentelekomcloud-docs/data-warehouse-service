:original_name: dws_06_0135.html

.. _dws_06_0135:

ALTER ROW LEVEL SECURITY POLICY
===============================

Function
--------

**ALTER ROW LEVEL SECURITY POLICY** modifies the definition of an existing row-level access control policy, including the name of the policy, the user to which the policy applies, and the policy expression.

Precautions
-----------

Only the table owner or administrator can execute **ALTER ROW LEVEL SECURITY POLICY**.

Syntax
------

::

   ALTER [ ROW LEVEL SECURITY ] POLICY [ IF EXISTS ] policy_name ON table_name RENAME TO new_policy_name

   ALTER [ ROW LEVEL SECURITY ] POLICY policy_name ON table_name
       [ TO { role_name | PUBLIC } [, ...] ]
       [ USING ( using_expression ) ]

Parameter Description
---------------------

.. table:: **Table 1** ALTER ROW LEVEL SECURITY POLICY parameters

   +------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------+
   | Parameter        | Description                                                                                                                                                                  | Value Range                            |
   +==================+==============================================================================================================================================================================+========================================+
   | policy_name      | Specifies the name of the row-level access control policy to be modified.                                                                                                    | An existing row-level security policy. |
   +------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------+
   | table_name       | Specifies the name of the table to which the row-level access control policy is applied.                                                                                     | Name of an existing table.             |
   +------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------+
   | new_policy_name  | Specifies the new name of a row-level access control policy.                                                                                                                 | ``-``                                  |
   +------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------+
   | role_name        | Specifies names of users affected by a row-level access control policy will be applied. **PUBLIC** indicates that the row-level access control policy will affect all users. | Name of an existing role.              |
   +------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------+
   | using_expression | Specifies an expression defined for a row-level access control policy. The return value is of the boolean type.                                                              | ``-``                                  |
   +------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------+

Examples
--------

Create the sample table **all_data** and users **alice** and **bob**.

::

   DROP TABLE IF EXISTS all_data;
   CREATE TABLE all_data (a int, b int);
   DROP USER IF EXISTS alice CASCADE;
   DROP USER IF EXISTS bob CASCADE;
   CREATE USER alice PASSWORD '{password}';
   CREATE USER bob PASSWORD '{password}';

Enable the row-level security policy for the **all_data** table.

::

   ALTER TABLE all_data ENABLE ROW LEVEL SECURITY;

Create a row-level security policy named **all_data_rls** on the **all_data** table.

::

   CREATE ROW LEVEL SECURITY POLICY all_data_rls ON all_data USING(b = CURRENT_USER);

Change the name of the row-level security policy from **all_data_rls** to **all_data_new_rls**.

::

   ALTER ROW LEVEL SECURITY POLICY all_data_rls ON all_data RENAME TO all_data_new_rls;

Change the users to which the row-level access control policy **all_data_new_rls** is applied to **alice** and **bob**.

::

   ALTER ROW LEVEL SECURITY POLICY all_data_new_rls ON all_data TO alice, bob;

Modify the filter criteria of the row-level access control policy **all_data_new_rls**. (**alice** and **bob** can access only the data rows whose value in column **a** is greater than 100 and the value in column **b** is the same as the current login username.)

::

   ALTER ROW LEVEL SECURITY POLICY all_data_new_rls ON all_data USING (a > 100 AND b = current_user);

Helpful Links
-------------

:ref:`CREATE ROW LEVEL SECURITY POLICY <dws_06_0169>` and :ref:`DROP ROW LEVEL SECURITY POLICY <dws_06_0200>`
