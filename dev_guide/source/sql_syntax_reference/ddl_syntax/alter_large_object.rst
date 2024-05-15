:original_name: dws_06_0129.html

.. _dws_06_0129:

ALTER LARGE OBJECT
==================

Function
--------

**ALTER LARGE OBJECT** changes the owner of a large object.

Precautions
-----------

Only the owner of a large object or a system administrator can run this statement.

Syntax
------

::

   ALTER LARGE OBJECT large_object_oid
       OWNER TO new_owner;

Parameter Description
---------------------

-  **large_object_oid**

   Specifies the OID of the large object to be modified.

   Value range: An existing large object name.

-  **OWNER TO new_owner**

   Specifies the new owner of the large object.

   Value range: An existing user name/role.

Examples
--------

None.
