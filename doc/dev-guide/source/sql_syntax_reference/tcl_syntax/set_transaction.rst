:original_name: dws_06_0264.html

.. _dws_06_0264:

SET TRANSACTION
===============

Function
--------

**SET TRANSACTION** sets the characteristics of the current transaction. It has no effect on any subsequent transactions. Available transaction characteristics include the transaction separation level and transaction access mode (read/write or read only).

Precautions
-----------

None

Syntax
------

Set the isolation level and access mode of the transaction.

::

   { SET [ LOCAL ] TRANSACTION|SET SESSION CHARACTERISTICS AS TRANSACTION }
     { ISOLATION LEVEL { READ COMMITTED | READ UNCOMMITTED | SERIALIZABLE | REPEATABLE READ }
     | { READ WRITE | READ ONLY } } [, ...]

Parameter Description
---------------------

.. table:: **Table 1** SET TRANSACTION parameters

   +-------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter               | Description                                                                                                                                                                                 | Value Range                                                                                                                                                           |
   +=========================+=============================================================================================================================================================================================+=======================================================================================================================================================================+
   | LOCAL                   | Indicates that the specified command takes effect only for the current transaction.                                                                                                         | ``-``                                                                                                                                                                 |
   +-------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | SESSION                 | Indicates that the specified command takes effect only for the current session.                                                                                                             | A string, which must comply with the naming convention.                                                                                                               |
   +-------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | ISOLATION LEVEL         | Specifies the transaction isolation level that determines the data that a transaction can view if other concurrent transactions exist.                                                      | -  **READ COMMITTED**: Only committed data is read. The default value is **READ COMMITTED**.                                                                          |
   |                         |                                                                                                                                                                                             | -  **READ UNCOMMITTED**: DWS does not support **READ UNCOMMITTED**. If **READ UNCOMMITTED** is set, **READ COMMITTED** is executed instead.                           |
   |                         | .. note::                                                                                                                                                                                   | -  **REPEATABLE READ**: Only the data committed before transaction start is read. Uncommitted data or data committed in other concurrent transactions cannot be read. |
   |                         |                                                                                                                                                                                             | -  **SERIALIZABLE**: DWS does not support **SERIALIZABLE**. If **SERIALIZABLE** is set, **REPEATABLE READ** is executed instead.                                      |
   |                         |    The isolation level of a transaction cannot be reset after the first clause (**INSERT**, **DELETE**, **UPDATE**, **FETCH**, **COPY**) for modifying data is executed in the transaction. |                                                                                                                                                                       |
   +-------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | READ WRITE \| READ ONLY | Specifies the transaction access mode.                                                                                                                                                      | Read-write mode or read-only mode.                                                                                                                                    |
   +-------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Examples
--------

Set the isolation level of the current transaction to **READ COMMITTED** and the access mode to **READ ONLY**:

::

   START TRANSACTION;
   SET LOCAL TRANSACTION ISOLATION LEVEL READ COMMITTED READ ONLY;
   COMMIT;
