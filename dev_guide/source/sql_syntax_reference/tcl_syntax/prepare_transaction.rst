:original_name: dws_06_0262.html

.. _dws_06_0262:

PREPARE TRANSACTION
===================

Function
--------

Prepares the current transaction for two-phase commit.

After this command is executed, the transaction is no longer associated with the current session. Instead, its state is completely stored on disk, and it is highly likely to be committed successfully, even if the database crashes before.

Once prepared, a transaction can later be committed or rolled back with :ref:`COMMIT PREPARED <dws_06_0260>` or :ref:`ROLLBACK PREPARED <dws_06_0268>`, respectively. Those commands can be issued from any session, not only the one that executed the original transaction.

From the point of view of the issuing session, **PREPARE TRANSACTION** is not unlike a **ROLLBACK** command: after executing it, there is no active current transaction, and the effects of the prepared transaction are no longer visible. (The effects will become visible again if the transaction is committed.)

If the **PREPARE TRANSACTION** command fails for any reason, it becomes a **ROLLBACK** and the current transaction is canceled.

Precautions
-----------

-  The transaction function is maintained automatically by the database, and should be not visible to users.
-  When running the **PREPARE TRANSACTION** command, increasing the value of **max_prepared_transactions** in configuration file **postgresql.conf**. You are advised to set **max_prepared_transactions** to a value not less than that of **max_connections** so that one pending prepared transaction is available for each session.

Syntax
------

.. code-block::

   PREPARE TRANSACTION transaction_id;

Parameter Description
---------------------

**transaction_id**

An arbitrary identifier that later identifies this transaction for **COMMIT PREPARED** or **ROLLBACK PREPARED**. The identifier must be different from those for current prepared transactions.

Value range: The identifier must be written as a string literal, and must be less than 200 bytes long.

Helpful Links
-------------

:ref:`COMMIT PREPARED <dws_06_0260>`, :ref:`ROLLBACK PREPARED <dws_06_0268>`
