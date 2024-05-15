:original_name: dws_06_0267.html

.. _dws_06_0267:

RELEASE SAVEPOINT
=================

Function
--------

**RELEASE SAVEPOINT** destroys a savepoint previously defined in the current transaction.

Destroying a savepoint makes it unavailable as a rollback point, but it has no other user visible behavior. It does not undo the effects of commands executed after the savepoint was established. To do that, use **ROLLBACK TO SAVEPOINT**. Destroying a savepoint when it is no longer needed allows the system to reclaim some resources earlier than transaction end.

**RELEASE SAVEPOINT** also destroys all savepoints that were established after the named savepoint was established.

Precautions
-----------

-  Releasing a savepoint name that was not previously defined causes an error.
-  It is not possible to release a savepoint when the transaction is in an aborted state.
-  If multiple savepoints have the same name, only the one that was most recently defined is released.

Syntax
------

::

   RELEASE [ SAVEPOINT ] savepoint_name;

Parameter Description
---------------------

**savepoint_name**

Specifies the name of the savepoint you want to destroy.

Examples
--------

Create and then destroy a savepoint:

::

   BEGIN;
       CREATE TABLE IF NOT EXISTS table1 (a int,b int);
       INSERT INTO table1 VALUES (3);
       SAVEPOINT my_savepoint;
       INSERT INTO table1 VALUES (4);
       RELEASE SAVEPOINT my_savepoint;
   COMMIT;

Helpful Links
-------------

:ref:`SAVEPOINT <dws_06_0263>`, :ref:`ROLLBACK TO SAVEPOINT <dws_06_0269>`
