:original_name: dws_06_0116.html

.. _dws_06_0116:

System Operation
================

GaussDB(DWS) runs SQL statements to perform different system operations, such as setting variables, displaying the execution plan, and collecting garbage data.

Setting Variables
-----------------

For details about how to set various parameters for a session or transaction, see :ref:`SET <dws_06_0220>`.

Displaying the Execution Plan
-----------------------------

For details about how to display the execution plan that GaussDB(DWS) makes for SQL statements, see :ref:`EXPLAIN <dws_06_0232>`.

Specifying a Checkpoint in Transaction Logs
-------------------------------------------

By default, WALs periodically specify checkpoints in a transaction log. **CHECKPOINT** forces an immediate checkpoint when the related command is issued, without waiting for a regular checkpoint scheduled by the system. For details, see :ref:`CHECKPOINT <dws_06_0258>`.

Collecting Unnecessary Data
---------------------------

For details about how to collect garbage data and analyze a database as required, For details, see :ref:`VACUUM <dws_06_0226>`.

Collecting statistics
---------------------

For details about how to collect statistics on tables in databases, For details, see :ref:`ANALYZE | ANALYSE <dws_06_0245>`.

Setting the Constraint Check Mode for the Current Transaction
-------------------------------------------------------------

For details about how to set the constraint check mode for the current transaction, For details, see :ref:`SET CONSTRAINTS <dws_06_0221>`.
