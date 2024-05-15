:original_name: dws_06_0200.html

.. _dws_06_0200:

DROP ROW LEVEL SECURITY POLICY
==============================

Function
--------

Deletes a row-level access control policy from a table.

Precautions
-----------

Only the table owner or administrators can delete a row-level access control policy from the table.

Syntax
------

::

   DROP [ ROW LEVEL SECURITY ] POLICY [ IF EXISTS ] policy_name ON table_name [ CASCADE | RESTRICT ]

Parameter Description
---------------------

-  **IF EXISTS**

   Reports a notice instead of an error if the specified row-level access control policy does not exist.

-  *policy_name*

   Specifies the name of a row-level access control policy to be deleted.

   -  *table_name*

      Specifies the name of a table to which a row-level access control policy is applied.

   -  CASCADE/RESTRICT

      The two parameters are used only for syntax compatibility. No objects depend on access control policies and thereby **CASCADE** is equivalent to **RESTRICT**.

Examples
--------

Delete the row-level access control policy **all_data_rls** from table **all_data**:

::

   DROP ROW LEVEL SECURITY POLICY all_data_rls ON all_data;

Helpful Links
-------------

:ref:`ALTER ROW LEVEL SECURITY POLICY <dws_06_0135>`, :ref:`CREATE ROW LEVEL SECURITY POLICY <dws_06_0169>`
