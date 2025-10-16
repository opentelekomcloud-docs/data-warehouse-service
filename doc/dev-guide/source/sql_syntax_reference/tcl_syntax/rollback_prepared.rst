:original_name: dws_06_0268.html

.. _dws_06_0268:

ROLLBACK PREPARED
=================

Function
--------

**ROLLBACK PREPARED** cancels a transaction ready for two-phase committing.

Precautions
-----------

-  The function is only available in maintenance mode (when GUC parameter **xc_maintenance_mode** is **on**). Exercise caution when enabling the mode. It is used by maintenance engineers for troubleshooting. Common users should not use the mode.
-  Only the user that initiates a transaction or the system administrator can roll back the transaction.
-  The transaction function is maintained automatically by the database, and should be not visible to users.

Syntax
------

::

   ROLLBACK PREPARED transaction_id ;

Parameter Description
---------------------

**transaction_id**

Specifies the identifier of the transaction to be submitted. The identifier must be different from those for current prepared transactions.

Helpful Links
-------------

:ref:`COMMIT PREPARED <dws_06_0260>`, :ref:`PREPARE TRANSACTION <dws_06_0262>`
