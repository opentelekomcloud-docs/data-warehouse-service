:original_name: dws_04_0086.html

.. _dws_04_0086:

Development Specifications
==========================

If the connection pool mechanism is used during application development, comply with the following specifications:

-  If GUC parameters are set in the connection, before you return the connection to the connection pool, run **SET SESSION AUTHORIZATION DEFAULT;RESET ALL;** to clear the connection status.
-  If a temporary table is used, delete it before you return the connection to the connection pool.

If you do not do so, the status of connections in the connection pool will remain, which affects subsequent operations using the connection pool.
