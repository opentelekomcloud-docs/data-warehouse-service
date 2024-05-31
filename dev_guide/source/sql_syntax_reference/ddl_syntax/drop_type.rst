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

-  **IF EXISTS**

   Sends a notice instead of an error if the specified type does not exist.

-  **name**

   Specifies the name of the type to be deleted (schema-qualified).

-  **CASCADE**

   Deletes objects (such as columns, functions, and operators) that depend on the type.

   **RESTRICT**

   Refuses to delete the type if any objects depend on it. This is the default.

Examples
--------

Delete the **compfoo** type:

::

   DROP TYPE compfoo cascade;

Helpful Links
-------------

:ref:`ALTER TYPE <dws_06_0148>`, :ref:`CREATE TYPE <dws_06_0185>`
