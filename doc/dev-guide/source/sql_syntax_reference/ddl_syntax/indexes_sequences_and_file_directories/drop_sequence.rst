:original_name: dws_06_0205.html

.. _dws_06_0205:

DROP SEQUENCE
=============

Function
--------

**DROP SEQUENCE** deletes a sequence from the current database.

Precautions
-----------

Only a sequence owner or a system administrator can delete a sequence.

Syntax
------

::

   DROP SEQUENCE [ IF EXISTS ] {[schema.]sequence_name} [ , ... ] [ CASCADE | RESTRICT ];

Parameter Description
---------------------

.. table:: **Table 1** DROP SEQUENCE parameters

   +-----------------------+-----------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------+
   | Parameter             | Description                                                                                         | Value Range                                                                                          |
   +=======================+=====================================================================================================+======================================================================================================+
   | IF EXISTS             | Sends a notice instead of an error if the specified sequence does not exist.                        | ``-``                                                                                                |
   +-----------------------+-----------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------+
   | name                  | Specifies the name of the sequence to be deleted.                                                   | Specifies an existing sequence name.                                                                 |
   +-----------------------+-----------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------+
   | CASCADE \| RESTRICT   | Specifies how to process related data in the dependent object when a delete operation is performed. | -  CASCADE: Automatically deletes objects that depend on the sequence to be deleted.                 |
   |                       |                                                                                                     | -  RESTRICT: Refuses to delete the sequence if any objects depend on it. This is the default action. |
   +-----------------------+-----------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------+

Examples
--------

Delete sequence **serial**.

::

   DROP SEQUENCE IF EXISTS serial;

Helpful Links
-------------

:ref:`ALTER SEQUENCE <dws_06_0137>`, :ref:`CREATE SEQUENCE <dws_06_0174>`
