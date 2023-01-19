:original_name: dws_06_0152.html

.. _dws_06_0152:

CLOSE
=====

Function
--------

**CLOSE** frees the resources associated with an open cursor.

Precautions
-----------

-  After a cursor is closed, no subsequent operations are allowed on it.
-  A cursor should be closed when it is no longer needed.
-  Every non-holdable open cursor is implicitly closed when a transaction is terminated by **COMMIT** or **ROLLBACK**.
-  A holdable cursor is implicitly closed if the transaction that created it aborts via **ROLLBACK**.
-  If the creating transaction successfully commits, the holdable cursor remains open until an explicit **CLOSE** is executed, or the client disconnects.
-  GaussDB(DWS) does not have an explicit **OPEN** cursor statement. A cursor is considered open when it is declared. You can see all available cursors by querying the **pg_cursors** system view.

Syntax
------

::

   CLOSE { cursor_name | ALL } ;

Parameter Description
---------------------

-  **cursor_name**

   Specifies the name of a cursor to be closed.

-  **ALL**

   Closes all open cursors.

Example
-------

Close a cursor.

::

   CLOSE cursor1;

Links
-----

:ref:`FETCH <dws_06_0216>`, :ref:`MOVE <dws_06_0217>`
