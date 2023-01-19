:original_name: dws_06_0201.html

.. _dws_06_0201:

DROP PROCEDURE
==============

Function
--------

**DROP PROCEDURE** deletes an existing stored procedure.

Precautions
-----------

None.

Syntax
------

::

   DROP PROCEDURE [ IF EXISTS  ] procedure_name ;

Parameter Description
---------------------

-  **IF EXISTS**

   Sends a notice instead of an error if the stored procedure does not exist.

-  **procedure_name**

   Specifies the name of the stored procedure to be deleted.

   Value range: An existing stored procedure name.

Examples
--------

Delete a stored procedure.

::

   DROP PROCEDURE prc_add;

Helpful Links
-------------

:ref:`CREATE PROCEDURE <dws_06_0170>`
