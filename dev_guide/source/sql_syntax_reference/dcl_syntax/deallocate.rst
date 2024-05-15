:original_name: dws_06_0246.html

.. _dws_06_0246:

DEALLOCATE
==========

Function
--------

Removes the prepared statements that were created earlier. If a prepared statement is not explicitly deleted, it is deleted at the end of the session.

For details about prepared statements, see :ref:`PREPARE <dws_06_0251>`.

Precautions
-----------

None

Syntax
------

::

   DEALLOCATE [ PREPARE ] { name | ALL };

Parameter Description
---------------------

-  **PREPARE**

   This keyword is optional and is often ignored.

-  **name**

   Specifies the name of the prepared statement to deallocate.

-  **ALL**

   Deallocates all prepared statements.

Examples
--------

None
