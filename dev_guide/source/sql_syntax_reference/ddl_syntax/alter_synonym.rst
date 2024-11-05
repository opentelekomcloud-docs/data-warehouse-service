:original_name: dws_06_0140.html

.. _dws_06_0140:

ALTER SYNONYM
=============

Function
--------

**ALTER SYNONYM** is used to modify the attribute of a synonym.

Precautions
-----------

-  Only the synonym owner can be changed.
-  Only the system administrator and the synonym owner have the permission to modify the synonym owner information.
-  The modifier must be a direct or indirect member of the new owner, and the new owner must have the CREATE permission on the schema to which the synonym belongs.

Syntax
------

::

   ALTER SYNONYM synonym_name
       OWNER TO new_owner;

Parameter Description
---------------------

-  **synonym**

   Name of a synonym to be modified (optionally with schema names)

   Value range: A string compliant with the identifier naming rules

-  **new_owner**

   New owner of a synonym object

   Value range: A string. It must be a valid username.

Examples
--------

Create synonym **t1**:

::

   CREATE OR REPLACE SYNONYM t1 FOR ot.t1;

Create user **u1**:

::

   CREATE USER u1 PASSWORD '{password}';

Change the owner of the synonym **t1** to **u1**:

::

   ALTER SYNONYM t1 OWNER TO u1;

Helpful Links
-------------

:ref:`CREATE SYNONYM <dws_06_0176>` and :ref:`DROP SYNONYM <dws_06_0207>`
