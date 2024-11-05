:original_name: dws_06_0260.html

.. _dws_06_0260:

COMMIT PREPARED
===============

Function
--------

Commits a prepared two-phase transaction.

Precautions
-----------

-  The function is available only in maintenance mode (when GUC parameter **xc_maintenance_mode** is **on**). It is usually used by maintenance engineers for troubleshooting. Common users should not use this mode.
-  Only the transaction creators or system administrators can run the **COMMIT** command. The creation and commit operations must be in different sessions.
-  The transaction function is maintained automatically by the database, and should be not visible to users.

Syntax
------

::

   COMMIT PREPARED transaction_id ;
   COMMIT PREPARED transaction_id WITH CSN;

Parameter Description
---------------------

-  **transaction_id**

   Specifies the identifier of the transaction to be submitted. The identifier must be different from those for current prepared transactions.

-  **CSN(commit sequence number)**

   Specifies the sequence number of the transaction to be committed. It is a 64-bit, incremental, unsigned number.

Helpful Links
-------------

:ref:`PREPARE TRANSACTION <dws_06_0262>`, :ref:`ROLLBACK PREPARED <dws_06_0268>`
