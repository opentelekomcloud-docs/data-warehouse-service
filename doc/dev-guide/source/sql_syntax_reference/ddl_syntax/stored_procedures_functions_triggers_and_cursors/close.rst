:original_name: dws_06_0152.html

.. _dws_06_0152:

CLOSE
=====

Function
--------

**CLOSE** frees the resources associated with a cursor.

Precautions
-----------

-  After a cursor is closed, no subsequent operations are allowed on it.
-  Cursors that are no longer used should be closed.
-  Each non-held open cursor should be implicitly closed after the transaction that created the cursor is terminated with **COMMIT** or **ROLLBACK**.
-  A holdable cursor is implicitly closed when the transaction that creates the cursor exits through **ROLLBACK**.
-  If the transaction that creates a cursor is committed, the cursor remains open until an explicit **CLOSE** operation is performed or the client is disconnected.
-  DWS does not have an explicit **OPEN** cursor statement. A cursor opens when you define it with the **CURSOR** command. You can see all available cursors by querying the **pg_cursors** system view.

Syntax
------

::

   CLOSE { cursor_name | ALL };

Parameter Description
---------------------

.. table:: **Table 1** CLOSE parameters

   +-------------+------------------------------------------------+-----------------------------+
   | Parameter   | Description                                    | Value Range                 |
   +=============+================================================+=============================+
   | cursor_name | Specifies the name of the cursor to be closed. | Name of an existing cursor. |
   +-------------+------------------------------------------------+-----------------------------+
   | ALL         | Closes all open cursors.                       | ``-``                       |
   +-------------+------------------------------------------------+-----------------------------+

Examples
--------

Close a cursor.

::

   DROP TABLE IF EXISTS customer;
   CREATE TABLE customer (a int, b int);
   START TRANSACTION;
   CURSOR cursor1 FOR SELECT * FROM customer;
   CLOSE cursor1;

Links
-----

:ref:`FETCH <dws_06_0216>` and :ref:`MOVE <dws_06_0217>`
