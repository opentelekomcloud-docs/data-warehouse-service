:original_name: dws_06_0206.html

.. _dws_06_0206:

DROP SERVER
===========

Function
--------

**DROP SERVER** deletes an existing data server.

Precautions
-----------

Only the server owner can delete a server.

Syntax
------

::

   DROP SERVER [ IF EXISTS ] server_name [ {CASCADE | RESTRICT} ] ;

Parameter Description
---------------------

-  **IF EXISTS**

   Sends a notice instead of an error if the specified table does not exist.

-  **server_name**

   Specifies the name of a server.

-  **CASCADE \| RESTRICT**

   -  **CASCADE**: automatically drops objects that depend on the server to be deleted.
   -  **RESTRICT** (default): refuses to delete the server if any objects depend on it.

Examples
--------

Delete the **hdfs_server** server:

::

   DROP SERVER hdfs_server;

Helpful Links
-------------

:ref:`CREATE SERVER <dws_06_0175>`, :ref:`ALTER SERVER <dws_06_0138>`
