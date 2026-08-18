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

.. table:: **Table 1** DROP EXCEPT RULE parameters

   +-----------+-------------------------------------------------------------------------------------+-----------------------------------------+
   | Parameter | Description                                                                         | Value Range                             |
   +===========+=====================================================================================+=========================================+
   | IF EXISTS | Sends a message instead of an error if the specified exception rule does not exist. | ``-``                                   |
   +-----------+-------------------------------------------------------------------------------------+-----------------------------------------+
   | rule_name | Indicates the name of the exception rule set.                                       | Name of an existing exception rule set. |
   +-----------+-------------------------------------------------------------------------------------+-----------------------------------------+

Examples
--------

Create exception rule set **except_rule_1**.

::

   DROP EXCEPT RULE IF EXISTS except_rule_1;
   CREATE EXCEPT RULE except_rule_1 WITH (blocktime=2000, spillsize=3000, action=abort);

Delete rule set **except_rule_1**.

::

   DROP EXCEPT RULE except_rule_1;

Delete rule set **except_rule_2** if it exists.

::

   DROP EXCEPT RULE IF EXISTS except_rule_2;

Links
-----

:ref:`CREATE EXCEPT RULE <dws_06_0281>`, :ref:`ALTER EXCEPT RULE <dws_06_0280>`
