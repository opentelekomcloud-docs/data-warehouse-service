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

.. table:: **Table 1** DROP SERVER parameters

   +-----------------------+-----------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------+
   | Parameter             | Description                                                                                         | Value Range                                                                          |
   +=======================+=====================================================================================================+======================================================================================+
   | IF EXISTS             | Sends a notice instead of an error if the specified server does not exist.                          | ``-``                                                                                |
   +-----------------------+-----------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------+
   | server_name           | Specifies the name of the server to be deleted.                                                     | An existing server name.                                                             |
   +-----------------------+-----------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------+
   | CASCADE \| RESTRICT   | Specifies how to process related data in the dependent object when a delete operation is performed. | -  **CASCADE**: automatically drops objects that depend on the server to be deleted. |
   |                       |                                                                                                     | -  **RESTRICT** (default): refuses to delete the server if any objects depend on it. |
   +-----------------------+-----------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------+

Examples
--------

Delete **hdfs_server** server.

::

   DROP SERVER IF EXISTS  hdfs_server;

Helpful Links
-------------

:ref:`ALTER SERVER <dws_06_0138>`, :ref:`CREATE SERVER <dws_06_0175>`
