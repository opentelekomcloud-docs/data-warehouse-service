:original_name: dws_04_0090.html

.. _dws_04_0090:

JDBC Package and Driver Class
=============================

JDBC Package
------------

Download the **dws_8.x.x_jdbc_driver.zip** software package from the console.

For details, see "Downloading the JDBC or ODBC Driver" in the *Data Warehouse Service (DWS) User Guide*.

JDBC driver JAR package obtained from decompression:

**gsjdbc4.jar**: Driver package compatible with PostgreSQL. The class name and class structure in the driver are the same as those in the PostgreSQL driver. All the applications running on PostgreSQL can be smoothly transferred to the current system.

Driver Class
------------

Before creating a database connection, you need to load the database driver class **org.postgresql.Driver** (decompressed from **gsjdbc4.jar**).
