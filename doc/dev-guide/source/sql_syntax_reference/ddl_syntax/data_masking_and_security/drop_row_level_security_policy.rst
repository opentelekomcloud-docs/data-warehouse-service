:original_name: dws_06_0200.html

.. _dws_06_0200:

DROP ROW LEVEL SECURITY POLICY
==============================

Function
--------

**DROP ROW LEVEL SECURITY POLICY** deletes a specified row-level access control policy on a table.

Precautions
-----------

Only the table owner or an administrator can execute **DROP ROW LEVEL SECURITY POLICY**.

Syntax
------

::

   DROP [ ROW LEVEL SECURITY ] POLICY [ IF EXISTS ] policy_name ON table_name [ CASCADE | RESTRICT ]

Parameter Description
---------------------

.. table:: **Table 1** DROP ROW LEVEL SECURITY POLICY parameters

   +------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------+
   | Parameter        | Description                                                                                                                                                     | Value Range                                                                          |
   +==================+=================================================================================================================================================================+======================================================================================+
   | IF EXISTS        | If the specified row-level access control policy does not exist, a notice is issued instead of throwing an error.                                               | ``-``                                                                                |
   +------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------+
   | policy_name      | Specifies the name of the row-level access control policy to be deleted.                                                                                        | Name of an existing row-level access control policy.                                 |
   +------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------+
   | table_name       | Specifies the name of the data table on which the row-level access control policy to be deleted resides.                                                        | Name of the data table on which an existing row-level access control policy resides. |
   +------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------+
   | CASCADE/RESTRICT | This syntax is provided only for compatibility. No objects depend on the row-level access control policy, so **CASCADE** and **RESTRICT** have the same effect. | ``-``                                                                                |
   +------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------+

Examples
--------

Delete the row-level access control policy **all_data_rls** on the table **all_data**.

::

   DROP ROW LEVEL SECURITY POLICY all_data_rls ON all_data;

Helpful Links
-------------

:ref:`ALTER ROW LEVEL SECURITY POLICY <dws_06_0135>`, :ref:`CREATE ROW LEVEL SECURITY POLICY <dws_06_0169>`
