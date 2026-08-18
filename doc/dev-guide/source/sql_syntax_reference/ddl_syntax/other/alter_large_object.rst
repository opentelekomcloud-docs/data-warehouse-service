:original_name: dws_06_0129.html

.. _dws_06_0129:

ALTER LARGE OBJECT
==================

Function
--------

This syntax modifies the definition of a large object. The only function of ALTER LARGE OBJECT is to assign a new owner.

Precautions
-----------

Only the owner of a large object can run this command. System administrators have this permission by default.

Syntax
------

::

   ALTER LARGE OBJECT large_object_oid
       OWNER TO new_owner;

Parameter Description
---------------------

.. table:: **Table 1** ALTER LARGE OBJECT parameters

   +------------------+---------------------------------------------------+-----------------------------------+
   | Parameter        | Description                                       | Value Range                       |
   +==================+===================================================+===================================+
   | large_object_oid | OID of the large object to be modified            | Name of an existing large object. |
   +------------------+---------------------------------------------------+-----------------------------------+
   | OWNER TO         | Clause used to modify the owner of a large object | ``-``                             |
   +------------------+---------------------------------------------------+-----------------------------------+
   | new_owner        | Owner of a large object                           | Name of an existing user or role. |
   +------------------+---------------------------------------------------+-----------------------------------+

Examples
--------

None.
