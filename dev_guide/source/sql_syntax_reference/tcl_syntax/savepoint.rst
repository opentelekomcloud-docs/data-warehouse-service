:original_name: dws_06_0263.html

.. _dws_06_0263:

SAVEPOINT
=========

Function
--------

Establishes a new savepoint within the current transaction.

A savepoint is a special mark inside a transaction that rolls back all commands that are executed after the savepoint was established, restoring the transaction state to what it was at the time of the savepoint.

Precautions
-----------

-  Use **ROLLBACK TO SAVEPOINT** to roll back to a savepoint. Use **RELEASE SAVEPOINT** to destroy a savepoint but keep the effects of the commands executed after the savepoint was established.
-  Savepoints can only be established when inside a transaction block. There can be multiple savepoints defined within a transaction.
-  **SAVEPOINT** cannot be used for functions, anonymous blocks, or stored procedures.
-  In the case of an unexpected termination of a distributed thread or process caused by a node or connection failure, or of an error caused by the inconsistency between source and destination table structures in a COPY FROM operation, the transaction cannot be rolled back to the established savepoint. Instead, the entire transaction will be rolled back.
-  According to the SQL standard, a savepoint is destroyed automatically when another savepoint with the same name is established. In GaussDB(DWS), old savepoints are kept, though only the most recent one will be used for rollback or release. Releasing the newer savepoint with **RELEASE SAVEPOINT** will cause the older one to again become accessible to **ROLLBACK TO SAVEPOINT** and **RELEASE SAVEPOINT**. Except for this, **SAVEPOINT** is fully SQL conforming.

Syntax
------

.. code-block::

   SAVEPOINT savepoint_name;

Parameter Description
---------------------

savepoint_name

Specifies the name of a new savepoint.

Examples
--------

-  Create a savepoint and undo all commands executed after the savepoint is created:

   ::

      START TRANSACTION;
      INSERT INTO table1 VALUES (1);
      SAVEPOINT my_savepoint;
      INSERT INTO table1 VALUES (2);
      ROLLBACK TO SAVEPOINT my_savepoint;
      INSERT INTO table1 VALUES (3);
      COMMIT;

   Query the table content, which should contain 1 and 3 but not 2, because 2 has been rolled back.

-  Create and then destroy a savepoint.

   ::

      START TRANSACTION;
      INSERT INTO table1 VALUES (3);
      SAVEPOINT my_savepoint;
      INSERT INTO table1 VALUES (4);
      RELEASE SAVEPOINT my_savepoint;
      COMMIT;

   Query the table content, which should contain both 3 and 4.

Helpful Links
-------------

:ref:`RELEASE SAVEPOINT <dws_06_0267>`, :ref:`ROLLBACK TO SAVEPOINT <dws_06_0269>`
