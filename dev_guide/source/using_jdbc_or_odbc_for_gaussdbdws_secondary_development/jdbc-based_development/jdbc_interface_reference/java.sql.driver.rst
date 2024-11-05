:original_name: dws_04_0106.html

.. _dws_04_0106:

java.sql.Driver
===============

This section describes **java.sql.Driver**, the database driver interface.

.. table:: **Table 1** Support status for java.sql.Driver

   ==================================== =========== ==============
   Method Name                          Return Type Support JDBC 4
   ==================================== =========== ==============
   acceptsURL(String url)               boolean     Yes
   connect(String url, Properties info) Connection  Yes
   jdbcCompliant()                      boolean     Yes
   getMajorVersion()                    int         Yes
   getMinorVersion()                    int         Yes
   ==================================== =========== ==============
