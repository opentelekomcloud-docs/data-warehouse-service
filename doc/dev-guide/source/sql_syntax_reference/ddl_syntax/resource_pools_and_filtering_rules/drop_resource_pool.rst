:original_name: dws_06_0202.html

.. _dws_06_0202:

DROP RESOURCE POOL
==================

Function
--------

**DROP RESOURCE POOL** deletes a resource pool.

.. note::

   If a role has been associated with a resource pool, the resource pool cannot be deleted.

Precautions
-----------

The user must have the DROP permission in order to delete a resource pool.

Syntax
------

::

   DROP RESOURCE POOL [ IF EXISTS ] pool_name;

Parameter Description
---------------------

.. table:: **Table 1** DROP RESOURCE POOL parameters

   +-----------+---------------------------------------------------------------------------------------------------+---------------------------------------------------------+
   | Parameter | Description                                                                                       | Value Range                                             |
   +===========+===================================================================================================+=========================================================+
   | IF EXISTS | If the specified resource pool does not exist, the system displays a message instead of an error. | ``-``                                                   |
   +-----------+---------------------------------------------------------------------------------------------------+---------------------------------------------------------+
   | pool_name | Specifies the name of a created resource pool.                                                    | A string, which must comply with the naming convention. |
   +-----------+---------------------------------------------------------------------------------------------------+---------------------------------------------------------+

Example
-------

Delete the resource pool **pool**.

::

   DROP RESOURCE POOL IF EXISTS pool;

Helpful Links
-------------

:ref:`ALTER RESOURCE POOL <dws_06_0133>`, :ref:`CREATE RESOURCE POOL <dws_06_0171>`
