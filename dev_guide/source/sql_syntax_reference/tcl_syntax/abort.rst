:original_name: dws_06_0256.html

.. _dws_06_0256:

ABORT
=====

Function
--------

**ABORT** rolls back the current transaction and cancels the changes in the transaction.

This command is equivalent to :ref:`ROLLBACK <dws_06_0266>`, and is present only for historical reasons. Now **ROLLBACK** is recommended.

Precautions
-----------

**ABORT** has no impact outside a transaction, but will provoke a warning.

Syntax
------

::

   ABORT [ WORK | TRANSACTION ] ;

Parameter Description
---------------------

**WORK \| TRANSACTION**

Optional keyword has no effect except increasing readability.

Examples
--------

Abort a transaction. Performed update operations will be undone.

::

   ABORT;

Helpful Links
-------------

:ref:`SET TRANSACTION <dws_06_0264>`, :ref:`COMMIT | END <dws_06_0259>`, :ref:`ROLLBACK <dws_06_0266>`
