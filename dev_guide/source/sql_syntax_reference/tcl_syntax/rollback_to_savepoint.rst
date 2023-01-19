:original_name: dws_06_0269.html

.. _dws_06_0269:

ROLLBACK TO SAVEPOINT
=====================

Function
--------

**ROLLBACK TO SAVEPOINT** rolls back to a savepoint. It implicitly destroys all savepoints that were established after the named savepoint.

Rolls back all commands that were executed after the savepoint was established. The savepoint remains valid and can be rolled back to again later, if needed.

Precautions
-----------

-  Specifying a savepoint name that has not been established is an error.
-  Cursors have somewhat non-transactional behavior with respect to savepoints. Any cursor that is opened inside a savepoint will be closed when the savepoint is rolled back. If a previously opened cursor is affected by a **FETCH** or **MOVE** command inside a savepoint that is later rolled back, the cursor remains at the position that **FETCH** left it pointing to (that is, the cursor motion caused by **FETCH** is not rolled back). Closing a cursor is not undone by rolling back, either. A cursor whose execution causes a transaction to abort is put in a cannot-execute state, so while the transaction can be restored using **ROLLBACK TO SAVEPOINT**, the cursor can no longer be used.
-  Use **ROLLBACK TO SAVEPOINT** to roll back to a savepoint. Use **RELEASE SAVEPOINT** to destroy a savepoint but keep the effects of the commands executed after the savepoint was established.

Syntax
------

::

   ROLLBACK [ WORK | TRANSACTION ] TO [ SAVEPOINT ] savepoint_name;

Parameter Description
---------------------

savepoint_name

Rolls back to a savepoint.

Examples
--------

Undo the effects of the commands executed after my_savepoint was established.

::

   ROLLBACK TO SAVEPOINT my_savepoint;

Cursor positions are not affected by savepoint rollback.

::

   BEGIN;
   DECLARE foo CURSOR FOR SELECT 1 UNION SELECT 2;
   SAVEPOINT foo;
   FETCH 1 FROM foo;
    ?column?
   ----------
           1
   ROLLBACK TO SAVEPOINT foo;
   FETCH 1 FROM foo;
    ?column?
   ----------
           2
   COMMIT;

Helpful Links
-------------

:ref:`SAVEPOINT <dws_06_0263>`, :ref:`RELEASE SAVEPOINT <dws_06_0267>`
