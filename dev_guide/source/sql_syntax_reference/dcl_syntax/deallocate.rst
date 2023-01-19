:original_name: dws_06_0246.html

.. _dws_06_0246:

DEALLOCATE
==========

Function
--------

**DEALLOCATE** deallocates a previously prepared statement. If you do not explicitly deallocate a prepared statement, it is deallocated when the session ends.

The **PREPARE** key word is always ignored.

Precautions
-----------

None

Syntax
------

::

   DEALLOCATE [ PREPARE ] { name | ALL };

Parameter Description
---------------------

-  **name**

   Specifies the name of the prepared statement to deallocate.

-  **ALL**

   Deallocates all prepared statements.

Examples
--------

None
