:original_name: dws_06_0282.html

.. _dws_06_0282:

DROP EXCEPT RULE
================

Function
--------

This syntax deletes a specified exception rule set.

Precautions
-----------

Only the system administrator can perform the **DROP EXCEPT RULE** operation.

Syntax
------

::

   DROP EXCEPT RULE [ IF EXISTS ]
       rule_name ;

Parameter Description
---------------------

-  **IF EXISTS**

   Sends a notice instead of an error if the specified exception rule set does not exist.

-  **rule_name**

   Indicates the name of the exception rule set.

   Value range: a string of 1 to 64 characters. It must comply with the naming convention.

Examples
--------

Remove the rule set named **except_rule1**:

::

   DROP EXCEPT RULE except_rule1;

Remove the rule set named **except_rule2** if it exists:

::

   DROP EXCEPT RULE IF EXISTS except_rule2;

Helpful Links
-------------

:ref:`ALTER EXCEPT RULE <dws_06_0280>`, :ref:`CREATE EXCEPT RULE <dws_06_0281>`
