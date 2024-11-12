:original_name: dws_01_0106.html

.. _dws_01_0106:

Development Specifications
==========================

If the connection pool mechanism is used during application development, comply with the following specifications: If you do not do so, the status of connections in the connection pool will remain, which affects subsequent operations using the connection pool.

-  If the GUC parameter is set in a connection, you must execute **SET SESSION AUTHORIZATION DEFAULT;RESET ALL;** to clear the connection status before returning the connection to the connection pool.
-  If a temporary table is used, it must be deleted before the connection is returned to the connection pool.
