:original_name: dws_04_0092.html

.. _dws_04_0092:

Loading a Driver
================

Load the database driver before creating a database connection.

You can load the driver in the following ways:

-  Implicitly loading the driver before creating a connection in the code: **Class.forName ("org.postgresql.Driver")**
-  Transferring a parameter during the JVM startup: **java -Djdbc.drivers=org.postgresql.Driver jdbctest**
