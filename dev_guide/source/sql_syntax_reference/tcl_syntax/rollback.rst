:original_name: dws_06_0266.html

.. _dws_06_0266:

ROLLBACK
========

Function
--------

Rolls back the current transaction and backs out all updates in the transaction.

**ROLLBACK** backs out of all changes that a transaction makes to a database if the transaction fails to be executed due to a fault.

Precautions
-----------

If a **ROLLBACK** statement is executed out of a transaction, no error occurs, but a warning information is displayed.

Syntax
------

::

   ROLLBACK [ WORK | TRANSACTION ];

Parameter Description
---------------------

**WORK \| TRANSACTION**

Optional keyword that more clearly illustrates the syntax.

Examples
--------

Undo all changes in the current transaction.

::

   ROLLBACK;

Helpful Links
-------------

:ref:`COMMIT | END <dws_06_0259>`
