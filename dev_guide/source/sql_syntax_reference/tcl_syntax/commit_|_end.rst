:original_name: dws_06_0259.html

.. _dws_06_0259:

COMMIT \| END
=============

Function
--------

**COMMIT** or **END** commits all operations of a transaction.

Precautions
-----------

Only the transaction creators or system administrators can run the **COMMIT** command. The creation and commit operations must be in different sessions.

Syntax
------

::

   { COMMIT | END } [ WORK | TRANSACTION ] ;

Parameter Description
---------------------

-  **COMMIT \| END**

   Commits the current transaction and makes all changes made by the transaction become visible to others.

-  **WORK \| TRANSACTION**

   Optional keyword has no effect except increasing readability.

Examples
--------

Commit the transaction to make all changes permanent.

::

   COMMIT;

Helpful Links
-------------

:ref:`ROLLBACK <dws_06_0266>`
