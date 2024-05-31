:original_name: dws_06_0135.html

.. _dws_06_0135:

ALTER ROW LEVEL SECURITY POLICY
===============================

Function
--------

**ALTER ROW LEVEL SECURITY POLICY** modifies an existing row-level access control policy, including the policy name and the users and expressions affected by the policy.

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

Create example users **role_a** and **role_b**.

::

   CREATE ROLE role_a PASSWORD '{Password}';
   CREATE ROLE role_b PASSWORD '{Password}';

Create example data table **public.all_data_t** and insert data into it.

::

   CREATE TABLE public.all_data_t(id int, role varchar(100), data varchar(100));
   INSERT INTO all_data_t VALUES(1, 'role_a', 'r_a_data');
   INSERT INTO all_data_t VALUES(2, 'role_b', 'r_b_data');
   INSERT INTO all_data_t VALUES(3, 'role_c', 'r_c_data');

Create a row-level access control policy.

::

   CREATE ROW LEVEL SECURITY POLICY all_data_t_rls ON all_data_t USING(role = CURRENT_USER);

Enable row-level access control.

::

   ALTER TABLE all_data_t ENABLE ROW LEVEL SECURITY;

Change the name of the **all_data_rls** policy.

::

   ALTER ROW LEVEL SECURITY POLICY all_data_t_rls ON all_data_t RENAME TO all_data_t_newrls;

Change the users affected by the row-level access control policy.

::

   ALTER ROW LEVEL SECURITY POLICY all_data_t_newrls ON all_data_t TO role_a, role_b;

Modify the expression defined for the access control policy.

::

   ALTER ROW LEVEL SECURITY POLICY all_data_t_newrls ON all_data_t USING (id > 100 AND role = current_user);

Helpful Links
-------------

:ref:`CREATE ROW LEVEL SECURITY POLICY <dws_06_0169>`, :ref:`DROP ROW LEVEL SECURITY POLICY <dws_06_0200>`
