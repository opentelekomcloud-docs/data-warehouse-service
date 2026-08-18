:original_name: dws_06_0265.html

.. _dws_06_0265:

START TRANSACTION
=================

Function
--------

**START TRANSACTION** starts a transaction. If the isolation level, read/write mode, or deferrable mode is specified, a new transaction will have those characteristics. You can also specify them using :ref:`SET TRANSACTION <dws_06_0264>`.

Precautions
-----------

None

Syntax
------

Format 1: START TRANSACTION

::

   START TRANSACTION
     [
       {
          ISOLATION LEVEL { READ COMMITTED | READ UNCOMMITTED | SERIALIZABLE | REPEATABLE READ }
          | { READ WRITE | READ ONLY }
        } [, ...]
     ];

Format 2: BEGIN

::

   BEGIN [ WORK | TRANSACTION ]
     [
       {
          ISOLATION LEVEL { READ COMMITTED | READ UNCOMMITTED | SERIALIZABLE | REPEATABLE READ }
          | { READ WRITE | READ ONLY }
         } [, ...]
     ];

Parameter Description
---------------------

.. table:: **Table 1** START TRANSACTION parameters

   +-------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter               | Description                                                                                                                                                                                 | Value Range                                                                                                                                                           |
   +=========================+=============================================================================================================================================================================================+=======================================================================================================================================================================+
   | WORK \| TRANSACTION     | Optional keyword in the **BEGIN** format, which does not affect the operation.                                                                                                              | ``-``                                                                                                                                                                 |
   +-------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | ISOLATION LEVEL         | Transaction isolation level. This parameter determines the data that a transaction can view if other concurrent transactions exist.                                                         | -  **READ COMMITTED**: Only committed data is read. The default value is **READ COMMITTED**.                                                                          |
   |                         |                                                                                                                                                                                             | -  **READ UNCOMMITTED**: DWS does not support **READ UNCOMMITTED**. If **READ UNCOMMITTED** is set, **READ COMMITTED** is executed instead.                           |
   |                         | .. note::                                                                                                                                                                                   | -  **REPEATABLE READ**: Only the data committed before transaction start is read. Uncommitted data or data committed in other concurrent transactions cannot be read. |
   |                         |                                                                                                                                                                                             | -  **SERIALIZABLE**: DWS does not support **SERIALIZABLE**. If **SERIALIZABLE** is set, **REPEATABLE READ** is executed instead.                                      |
   |                         |    The isolation level of a transaction cannot be reset after the first clause (**INSERT**, **DELETE**, **UPDATE**, **FETCH**, **COPY**) for modifying data is executed in the transaction. |                                                                                                                                                                       |
   +-------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | READ WRITE \| READ ONLY | Transaction access mode.                                                                                                                                                                    | Read-write mode or read-only mode.                                                                                                                                    |
   +-------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Examples
--------

-  Prepare data.

   .. code-block::

      DROP SCHEMA IF EXISTS tpcds CASCADE;
      CREATE SCHEMA tpcds;
      CREATE TABLE tpcds.reason (a int, b int);

-  Start a transaction in default mode.

   ::

      START TRANSACTION;
      SELECT * FROM tpcds.reason;
      END;

-  Start a transaction with the isolation level being **READ COMMITTED** and the access mode being **READ WRITE**:

   ::

      START TRANSACTION ISOLATION LEVEL READ COMMITTED READ WRITE;
      SELECT * FROM tpcds.reason;
      COMMIT;

Helpful Links
-------------

:ref:`COMMIT | END <dws_06_0259>`, :ref:`ROLLBACK <dws_06_0266>`, :ref:`SET TRANSACTION <dws_06_0264>`
