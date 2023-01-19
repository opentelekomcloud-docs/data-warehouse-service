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

-  **IF EXISTS**

   Sends a notice instead of an error if the specified sequence does not exist.

-  **name**

   Specifies the name of the sequence.

-  **CASCADE**

   Automatically deletes objects that depend on the sequence to be deleted.

-  **RESTRICT**

   Refuses to delete the sequence if any objects depend on it. This is the default.

Examples
--------

Delete the sequence.

::

   DROP SEQUENCE serial;

Helpful Links
-------------

:ref:`CREATE SEQUENCE <dws_06_0174>` :ref:`ALTER SEQUENCE <dws_06_0137>`
