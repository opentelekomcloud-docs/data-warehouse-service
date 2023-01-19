:original_name: dws_06_0136.html

.. _dws_06_0136:

ALTER SCHEMA
============

Function
--------

**ALTER SCHEMA** changes the attributes of a schema.

Precautions
-----------

Only the owner of an index or a system administrator can run this statement.

Syntax
------

-  Rename a schema.

   ::

      ALTER SCHEMA schema_name
          RENAME TO new_name;

-  Changes the owner of a schema.

   ::

      ALTER SCHEMA schema_name
          OWNER TO new_owner;

-  Changes the storage space limit of the permanent table in the schema.

   ::

      ALTER SCHEMA schema_name
          WITH PERM SPACE 'space_limit';

Parameter Description
---------------------

-  **schema_name**

   Indicates the name of the current schema.

   Value range: An existing schema name.

-  **RENAME TO new_name**

   Renames a schema.

   **new_name**: new name of the schema

   Value range: A string. It must comply with the naming convention.

-  **OWNER TO new_owner**

   Changes the owner of a schema. To do this as a non-administrator, you must be a direct or indirect member of the new owning role, and that role must have CREATE permission in the database.

   **new_owner**: new owner of a schema

   Value range: An existing user name/role.

-  **WITH PERM SPACE**

   Changes the storage upper limit of the permanent table in the schema. If a non-administrator user wants to change the storage upper limit, the user must be a direct or indirect member of all new roles, and the member must have the CREATE permission on the database.

   **new_owner**: new owner of a schema

   Value range: A string consists of an integer and unit. The unit can be K/M/G/T/P currently. The unit of parsed value is K and cannot exceed the range that can be expressed in 64 bits, which is 1 KB to 9007199254740991 KB.

Examples
--------

Rename the **ds** schema to **ds_new**.

::

   ALTER SCHEMA ds RENAME TO ds_new;

Change the owner of **ds_new** to **jack**.

::

   ALTER SCHEMA ds_new OWNER TO jack;

Helpful Links
-------------

:ref:`CREATE SCHEMA <dws_06_0173>`, :ref:`DROP SCHEMA <dws_06_0204>`
