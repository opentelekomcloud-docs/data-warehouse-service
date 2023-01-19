:original_name: dws_06_0117.html

.. _dws_06_0117:

Controlling Transactions
========================

A transaction is a user-defined sequence of database operations, which form an integral unit of work.

Starting Transactions
---------------------

GaussDB(DWS) starts a transaction using **START TRANSACTION** and **BEGIN**. For details, see :ref:`START TRANSACTION <dws_06_0265>` and :ref:`BEGIN <dws_06_0257>`.

Setting Transactions
--------------------

GaussDB(DWS) sets a transaction using **SET TRANSACTION** or **SET LOCAL TRANSACTION**. For details, see :ref:`SET TRANSACTION <dws_06_0264>`.

Submitting Transactions
-----------------------

GaussDB(DWS) commits all operations of a transaction using **COMMIT** or **END**. For details, see :ref:`COMMIT | END <dws_06_0259>`.

Rolling Back Transactions
-------------------------

If a fault occurs during a transaction and the transaction cannot proceed, the system performs rollback to cancel all the completed database operations related to the transaction. For details, see :ref:`ROLLBACK <dws_06_0266>`.

.. note::

   If an execution request (not in a transaction block) received in the database contains multiple statements, the statements will be packed into a transaction. If one of the statements fails, the entire request will be rolled back.
