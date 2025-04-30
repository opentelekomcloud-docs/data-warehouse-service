:original_name: dws_06_0286.html

.. _dws_06_0286:

DROP PUBLICATION
================

Function
--------

**DROP PUBLICATION** deletes a publication.

Precautions
-----------

-  This statement is supported by version 8.2.0.100 or later clusters.
-  This statement can be used by the publication owner and system administrators only.

Syntax
------

::

   DROP PUBLICATION [ IF EXISTS ] name [, ...] [ CASCADE | RESTRICT ]

Parameter Description
---------------------

-  **IF EXISTS**

   If the specified publication does not exist, no error is thrown. Instead, a notification is reported indicating that the publication does not exist.

-  **name**

   Specifies the name of the publication you want to delete.

   Value range: an existing publication

-  **CASCADE \| RESTRICT**

   Currently, these keywords do not work because there is no dependency on publications.

Examples
--------

Delete a publication.

.. code-block::

   DROP PUBLICATION mypublication;

Helpful Links
-------------

:ref:`ALTER PUBLICATION <dws_06_0284>`, :ref:`CREATE PUBLICATION <dws_06_0285>`
