:original_name: dws_06_0255.html

.. _dws_06_0255:

TCL Syntax Overview
===================

Transaction Control Language (TCL) controls the time and effect of database transactions and monitors the database.

Commit
------

DWS uses the COMMIT or END statement to commit transactions. For details, see :ref:`COMMIT | END <dws_06_0259>`.

Setting a Savepoint
-------------------

DWS creates a new savepoint in the current transaction. For details, see :ref:`SAVEPOINT <dws_06_0263>`.

Rollback
--------

DWS rolls back the current transaction to the last committed state. For details, see :ref:`ROLLBACK <dws_06_0266>`.
