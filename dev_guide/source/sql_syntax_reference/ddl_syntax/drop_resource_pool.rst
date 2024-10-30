:original_name: dws_06_0202.html

.. _dws_06_0202:

DROP RESOURCE POOL
==================

Function
--------

Deletes a resource pool.

.. note::

   If a role has been associated with a resource pool, this resource pool cannot be deleted.

Precautions
-----------

The user must have the DROP permission in order to delete a resource pool.

Syntax
------

::

   DROP RESOURCE POOL [ IF EXISTS ] pool_name;

Parameter Description
---------------------

-  **IF EXISTS**

   Sends a notice instead of an error if the stored procedure does not exist.

-  **pool_name**

   Specifies the name of a created resource pool.

   Value range: a string. It must comply with the naming convention.

Examples
--------

Delete the resource pool named **pool**:

::

   DROP RESOURCE POOL pool;

Helpful Links
-------------

:ref:`ALTER RESOURCE POOL <dws_06_0133>`, :ref:`CREATE RESOURCE POOL <dws_06_0171>`
