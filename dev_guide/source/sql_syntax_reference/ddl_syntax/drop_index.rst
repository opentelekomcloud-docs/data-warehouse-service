:original_name: dws_06_0195.html

.. _dws_06_0195:

DROP INDEX
==========

Function
--------

Deletes an index.

Precautions
-----------

Only the owner of an index or a system administrator can run the **DROP INDEX** command.

Syntax
------

::

   DROP INDEX [ CONCURRENTLY ][ IF EXISTS ]
       index_name [, ...] [ CASCADE | RESTRICT ];

Parameter Description
---------------------

-  **CONCURRENTLY**

   Deletes an index without locking concurrent selections, inserts, updates, and deletes on the index table. A regular **DROP INDEX** obtains an exclusive lock on the table to prevent other access until the index is deleted. With this option, the command waits until any conflicting transaction is complete.

   Note that only one index name can be specified and the **CASCADE** option is not supported. (Therefore, indexes that support **UNIQUE** or **PRIMARY KEY** constraints cannot be deleted in this way.) Regular **DROP INDEX** commands can be executed within a transaction block, but **DROP INDEX CONCURRENTLY** cannot.

-  **IF EXISTS**

   Sends a notice instead of an error if the specified index does not exist.

-  **index_name**

   Specifies the name of the index to be deleted.

   Value range: an existing index.

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
