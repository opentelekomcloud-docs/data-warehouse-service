:original_name: dws_03_0017.html

.. _dws_03_0017:

Does GaussDB(DWS) Support Third-Party Clients and JDBC and ODBC Drivers?
========================================================================

Yes, but GaussDB(DWS) clients and drivers are recommended. Unlike open-source PostgreSQL clients and drivers, GaussDB(DWS) clients and drivers have two key advantages:

-  **Security hardening**: PostgreSQL drivers only support MD5 authentication, but GaussDB(DWS) drivers support SHA256 and MD5.
-  **Data type enhancement**: GaussDB(DWS) drivers support new data types smalldatetime and tinyint.

GaussDB(DWS) supports open-source PostgreSQL clients and JDBC and ODBC drivers.

The compatible client and driver versions are:

-  PostgreSQL psql 9.2.4 or later
-  PostgreSQL JDBC Driver 9.3-1103 or later
-  PSQL ODBC 09.01.0200 or later

For details about how to use JDBC/ODBC to connect to GaussDB(DWS), see *Tutorial: Development Using JDBC or ODBC*.
