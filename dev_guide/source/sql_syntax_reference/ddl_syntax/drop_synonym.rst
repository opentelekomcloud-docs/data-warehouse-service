:original_name: dws_06_0207.html

.. _dws_06_0207:

DROP SYNONYM
============

Function
--------

**DROP SYNONYM** is used to delete a synonym object.

Precautions
-----------

Only a synonym owner or a system administrator can run the **DROP SYNONYM** command.

Syntax
------

::

   DROP SYNONYM [ IF EXISTS ] synonym_name [ CASCADE | RESTRICT ];

Parameter Description
---------------------

-  **IF EXISTS**

   Send a notice instead of reporting an error if the specified synonym does not exist.

-  **synonym_name**

   Name of a synonym (optionally with schema names)

-  **CASCADE \| RESTRICT**

   -  **CASCADE**: automatically deletes objects (such as views) that depend on the synonym to be deleted.
   -  **RESTRICT**: refuses to delete the synonym if any objects depend on it. This is the default.

Examples
--------

Delete a synonym.

::

   DROP SYNONYM t1;
   DROP SCHEMA ot CASCADE;

Helpful Links
-------------

:ref:`ALTER SYNONYM <dws_06_0140>` and :ref:`CREATE SYNONYM <dws_06_0176>`
