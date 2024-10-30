:original_name: dws_06_0135.html

.. _dws_06_0135:

ALTER ROW LEVEL SECURITY POLICY
===============================

Function
--------

Modifies an existing row-level access control policy, including the policy name and the users and expressions affected by the policy.

Precautions
-----------

Only the table owner or administrators can perform this operation.

Syntax
------

::

   ALTER [ ROW LEVEL SECURITY ] POLICY [ IF EXISTS ] policy_name ON table_name RENAME TO new_policy_name

   ALTER [ ROW LEVEL SECURITY ] POLICY policy_name ON table_name
       [ TO { role_name | PUBLIC } [, ...] ]
       [ USING ( using_expression ) ]

Parameter Description
---------------------

-  **policy_name**

   Specifies the name of a row-level access control policy to be modified.

-  **table_name**

   Specifies the name of a table to which a row-level access control policy is applied.

-  **new_policy_name**

   Specifies the new name of a row-level access control policy.

-  **role_name**

   Specifies names of users affected by a row-level access control policy will be applied. **PUBLIC** indicates that the row-level access control policy will affect all users.

-  **using_expression**

   Specifies an expression defined for a row-level access control policy. The return value is of the boolean type.

Examples
--------

Enable row-level access control.

::

   ALTER TABLE all_data ENABLE ROW LEVEL SECURITY;

Change the name of the **all_data_rls** policy.

::

   ALTER ROW LEVEL SECURITY POLICY all_data_rls ON all_data RENAME TO all_data_new_rls;

Change the users affected by the row-level access control policy.

::

   ALTER ROW LEVEL SECURITY POLICY all_data_new_rls ON all_data TO alice, bob;

Modify the expression defined for the access control policy.

::

   ALTER ROW LEVEL SECURITY POLICY all_data_new_rls ON all_data USING (id > 100 AND role = current_user);

Helpful Links
-------------

:ref:`CREATE ROW LEVEL SECURITY POLICY <dws_06_0169>`, :ref:`DROP ROW LEVEL SECURITY POLICY <dws_06_0200>`
