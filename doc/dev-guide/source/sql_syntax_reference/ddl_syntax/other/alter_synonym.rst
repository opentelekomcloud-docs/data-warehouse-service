:original_name: dws_06_0140.html

.. _dws_06_0140:

ALTER SYNONYM
=============

Function
--------

**ALTER SYNONYM** is used to modify the attribute of a synonym.

Precautions
-----------

-  Only the owner of the **SYNONYM** object can be changed.
-  Only the system administrator and the owner of the **SYNONYM** object can change its owner information.
-  The modifier must be a direct or indirect member of the new owner, and the new owner must have the **CREATE** permission on the schema to which the synonym belongs.

Syntax
------

::

   ALTER SYNONYM synonym_name
       OWNER TO new_owner;

Parameter Description
---------------------

.. table:: **Table 1** ALTER SYNONYM parameters

   +--------------+------------------------------------------------------------------------+------------------------------+
   | Parameter    | Description                                                            | Value Range                  |
   +==============+========================================================================+==============================+
   | synonym_name | Name of the synonym to be modified, which can include the schema name. | Name of an existing synonym. |
   +--------------+------------------------------------------------------------------------+------------------------------+
   | OWNER TO     | Clause used to modify the owner of a synonym.                          | ``-``                        |
   +--------------+------------------------------------------------------------------------+------------------------------+
   | new_owner    | New owner of a synonym.                                                | Valid username.              |
   +--------------+------------------------------------------------------------------------+------------------------------+

Examples
--------

Create synonym **t1**.

::

   CREATE OR REPLACE SYNONYM t1 FOR ot.t1;

Create user **u1**.

::

   CREATE USER u1 PASSWORD '{password}';

Change the owner of the synonym **t1** to **u1**.

::

   ALTER SYNONYM t1 OWNER TO u1;

Helpful Links
-------------

:ref:`CREATE SYNONYM <dws_06_0176>` and :ref:`DROP SYNONYM <dws_06_0207>`
