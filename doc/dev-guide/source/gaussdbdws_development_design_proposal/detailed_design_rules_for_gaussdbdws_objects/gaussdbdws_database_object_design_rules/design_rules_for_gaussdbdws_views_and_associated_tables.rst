:original_name: dws_04_0082.html

.. _dws_04_0082:

Design Rules for GaussDB(DWS) Views and Associated Tables
=========================================================

View Design
-----------

-  [Proposal] Do not nest views unless they have strong dependency on each other.
-  [Proposal] Try to avoid sort operations in a view definition.

Joined Table Design
-------------------

-  [Proposal] Minimize joined columns across tables.
-  [Proposal] Joined columns should use the same data type.
-  [Proposal] The names of associated fields should show the associations. For example, they can use the same name.
