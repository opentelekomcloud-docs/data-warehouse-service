:original_name: dws_06_0293.html

.. _dws_06_0293:

DROP BLOCK RULE
===============

Function
--------

Delete a query filtering rule.

This syntax is supported only by clusters of 9.1.0.100 and later versions.

Precautions
-----------

Only a user with the database owner permission or the **gs_role_block** role permission can run the **DROP BLOCK RULE** command. A system administrator has this permission by default.

Syntax
------

::

   DROP BLOCK RULE [ IF EXISTS ] block_name;

Parameter Description
---------------------

-  **IF EXISTS**

   Sends a notice instead of an error if the specified query filtering rule does not exist.

-  **block_name**

   Name of the query filtering rule to be deleted.

   Value range: a string, which indicates the name of an existing query filtering rule.

Example
-------

Delete a query filtering rule named **query_block**.

::

   DROP BLOCK RULE query_block;
   DROP BLOCK RULE IF EXISTS query_block;
