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

.. table:: **Table 1** DROP SYNONYM parameters

   +-----------------------+-----------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------+
   | Parameter             | Description                                                                                         | Value Range                                                                                             |
   +=======================+=====================================================================================================+=========================================================================================================+
   | IF EXISTS             | Sends a notice instead of reporting an error if the specified synonym does not exist.               | ``-``                                                                                                   |
   +-----------------------+-----------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------+
   | synonym_name          | Name of a synonym which is deleted (optionally with schema names)                                   | An existing synonym name.                                                                               |
   +-----------------------+-----------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------+
   | CASCADE \| RESTRICT   | Specifies how to process related data in the dependent object when a delete operation is performed. | -  **CASCADE**: automatically deletes objects (such as views) that depend on the synonym to be deleted. |
   |                       |                                                                                                     | -  **RESTRICT**: refuses to delete the synonym if any objects depend on it. This is the default.        |
   +-----------------------+-----------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------+

Examples
--------

Delete a synonym.

::

   DROP SYNONYM IF EXISTS t1;

Helpful Links
-------------

:ref:`ALTER SYNONYM <dws_06_0140>` and :ref:`CREATE SYNONYM <dws_06_0176>`
