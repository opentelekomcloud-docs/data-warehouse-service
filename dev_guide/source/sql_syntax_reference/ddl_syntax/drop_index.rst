:original_name: dws_06_0195.html

.. _dws_06_0195:

DROP INDEX
==========

Function
--------

**DROP INDEX** deletes an index.

Precautions
-----------

Only the owner of an index or a system administrator can run **DROP INDEX** command.

Syntax
------

::

   DROP INDEX [ IF EXISTS ]
       index_name [, ...] [ CASCADE | RESTRICT ];

Parameter Description
---------------------

-  **IF EXISTS**

   Sends a notice instead of an error if the specified index does not exist.

-  **index_name**

   Specifies the name of the index to be deleted.

   Value range: An existing index.

-  **CASCADE \| RESTRICT**

   -  **CASCADE**: automatically deletes all objects that depend on the index to be deleted.
   -  **RESTRICT** (default): refuses to delete the index if any objects depend on it.

Examples
--------

Delete the **ds_ship_mode_t1_index2** index.

::

   DROP INDEX tpcds.ds_ship_mode_t1_index2;

Helpful Links
-------------

:ref:`ALTER INDEX <dws_06_0128>`, :ref:`CREATE INDEX <dws_06_0165>`
