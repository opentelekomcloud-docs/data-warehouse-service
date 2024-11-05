:original_name: dws_06_0204.html

.. _dws_06_0204:

DROP SCHEMA
===========

Function
--------

**DROP SCHEMA** deletes a schema in a database.

Precautions
-----------

Only the owner of a schema or a user granted with the DROP permission for the schema or a system administrator has the permission to execute the **Drop SCHEMA** statement.

Syntax
------

::

   DROP SCHEMA [ IF EXISTS ] schema_name [, ...] [ CASCADE | RESTRICT ];

Parameter Description
---------------------

-  **IF EXISTS**

   Sends a notice instead of an error if the specified schema does not exist.

-  **schema_name**

   Specifies the name of a schema.

   Value range: An existing schema name.

-  **CASCADE \| RESTRICT**

   -  **CASCADE**: automatically deletes all objects that are contained in the schema to be deleted.
   -  **RESTRICT**: refuses to delete the schema that contains any objects. This is the default.

.. important::

   Do not delete the schemas with the beginning of **pg_temp** or **pg_toast_temp**. They are internal system schemas, and deleting them may cause unexpected errors.

.. note::

   A user cannot delete the schema in use. To delete the schema in use, switch to another schema.

Example
-------

Delete the **ds_new** schema:

.. code-block::

   DROP SCHEMA ds_new;

Links
-----

:ref:`ALTER SCHEMA <dws_06_0136>`, :ref:`CREATE SCHEMA <dws_06_0173>`
