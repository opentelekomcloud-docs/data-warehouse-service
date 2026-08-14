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

**savepoint_name**

Rolls back the name of the specified savepoint.

Examples
--------

Undo the effects of commands executed after creating **my_savepoint** (such as the command inserting data 2 below):

::

   DROP SCHEMA IF EXISTS tpcds CASCADE;
   CREATE SCHEMA tpcds;
   CREATE TABLE tpcds.table1 (
       id integer
   );

   BEGIN;  -- Start a transaction.
       INSERT INTO tpcds.table1 VALUES (1);  -- Insert data 1.
       SAVEPOINT my_savepoint;              -- Create a savepoint.
       INSERT INTO tpcds.table1 VALUES (2);  -- Insert data 2.
       ROLLBACK TO SAVEPOINT my_savepoint;   -- Roll back to the savepoint (delete data 2).
       INSERT INTO tpcds.table1 VALUES (3);  -- Continue inserting data 3.
   COMMIT;  -- Commit the transaction.

   SELECT * FROM tpcds.table1;  -- Verify the data. The command inserting data 2 has been undone.

|image1|

Cursor positions are not affected by savepoint rollback:

::

   BEGIN;
   DECLARE foo CURSOR FOR SELECT 1 UNION SELECT 2;
   SAVEPOINT foo;
   FETCH 1 FROM foo;

|image2|

::

   ROLLBACK TO SAVEPOINT foo;
   FETCH 1 FROM foo;

|image3|

::

   COMMIT;

Helpful Links
-------------

:ref:`SAVEPOINT <dws_06_0263>`, :ref:`RELEASE SAVEPOINT <dws_06_0267>`

.. |image1| image:: /_static/images/en-us_image_0000002587595598.png
.. |image2| image:: /_static/images/en-us_image_0000002587446474.png
.. |image3| image:: /_static/images/en-us_image_0000002587606436.png
