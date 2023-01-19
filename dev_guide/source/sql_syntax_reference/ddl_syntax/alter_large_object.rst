:original_name: dws_06_0129.html

.. _dws_06_0129:

ALTER LARGE OBJECT
==================

Function
--------

**ALTER LARGE OBJECT** modifies the definition of a large object. It can only assign a new owner to a large object.

Precautions
-----------

Only the administrator or the owner of the to-be-modified large object can run **ALTER LARGE OBJECT**.

Syntax
------

::

   ALTER LARGE OBJECT large_object_oid
       OWNER TO new_owner;

Parameter Description
---------------------

-  **large_object_oid**

   OID of a large object.

   Value range: an existing large object name

-  **OWNER TO new_owner**

   New owner of a large object

   Value range: an existing user name/role

Examples
--------

None.
