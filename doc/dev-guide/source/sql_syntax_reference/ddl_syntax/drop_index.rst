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

   DROP INDEX [ CONCURRENTLY ][ IF EXISTS ]
       index_name [, ...] [ CASCADE | RESTRICT ];

Parameters
----------

-  **CONCURRENTLY**

   Deletes an index without locking concurrent selections, inserts, updates, and deletes on the index table. A normal **DROP INDEX** obtains an exclusive lock on the table to prevent other access until the index is deleted. With this option, the command waits until the conflicting transaction is complete.

   Note that only one index name can be specified and the **CASCADE** option is not supported. (Therefore, indexes that support **UNIQUE** or **PRIMARY KEY** constraints cannot be deleted in this way.) Regular **DROP INDEX** commands can be executed within a transaction block, but cannot be executed in **DROP INDEX CONCURRENTLY** mode.

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

Delete the **ds_ship_mode_t1_index2** index:

::

   DROP INDEX tpcds.ds_ship_mode_t1_index2;

Helpful Links
-------------

:ref:`ALTER INDEX <dws_06_0128>`, :ref:`CREATE INDEX <dws_06_0165>`
