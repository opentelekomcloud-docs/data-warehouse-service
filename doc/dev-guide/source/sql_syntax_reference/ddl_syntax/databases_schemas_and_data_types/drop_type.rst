:original_name: dws_06_0213.html

.. _dws_06_0213:

DROP TYPE
=========

Function
--------

**DROP TYPE** deletes a user-defined data type. Only the type owner has permission to run this statement.

Syntax
------

::

   DROP TYPE [ IF EXISTS ] name [, ...] [ CASCADE | RESTRICT ]

Parameter Description
---------------------

.. table:: **Table 1** DROP TYPE parameters

   +-----------------------+-----------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------+
   | Parameter             | Description                                                                                         | Value Range                                                                                      |
   +=======================+=====================================================================================================+==================================================================================================+
   | IF EXISTS             | If the specified type does not exist, a message is displayed instead of an error.                   | ``-``                                                                                            |
   +-----------------------+-----------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------+
   | name                  | Specifies the name of the type to be deleted (schema-qualified).                                    | Specifies an existing domain type.                                                               |
   +-----------------------+-----------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------+
   | CASCADE \| RESTRICT   | Specifies how to process related data in the dependent object when a delete operation is performed. | -  CASCADE: Deletes objects (such as columns, functions, and operators) that depend on the type. |
   |                       |                                                                                                     | -  RESTRICT: Refuses to delete the type if any objects depend on it. This is the default.        |
   +-----------------------+-----------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------+

Examples
--------

Create the **compfoo** type.

::

   CREATE TYPE compfoo AS (f1 int, f2 text);

Delete **compfoo** type.

::

   DROP TYPE IF EXISTS compfoo cascade;

Helpful Links
-------------

:ref:`ALTER TYPE <dws_06_0148>`, :ref:`CREATE TYPE <dws_06_0185>`
