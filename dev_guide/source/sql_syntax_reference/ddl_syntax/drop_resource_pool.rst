:original_name: dws_06_0202.html

.. _dws_06_0202:

DROP RESOURCE POOL
==================

Function
--------

**DROP RESOURCE POOL** deletes a resource pool.

.. note::

   The resource pool cannot be deleted if it is associated with a role.

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

.. note::

   A resource pool can be independently deleted only when it is not associated with any users.

Example
-------

Delete a resource pool.

.. code-block::

   DROP RESOURCE POOL pool1;

Links
-----

:ref:`ALTER RESOURCE POOL <dws_06_0133>`, :ref:`CREATE RESOURCE POOL <dws_06_0171>`
