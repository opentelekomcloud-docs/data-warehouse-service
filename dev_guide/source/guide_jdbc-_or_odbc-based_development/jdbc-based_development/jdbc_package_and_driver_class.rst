:original_name: dws_04_0090.html

.. _dws_04_0090:

JDBC Package and Driver Class
=============================

JDBC Package
------------

Obtain the package **dws_8.1.x_jdbc_driver.zip** from the management console. For details, see :ref:`Downloading Drivers <dws_04_0087>`.

JDBC driver JAR package obtained from decompression:

**gsjdbc4.jar**: Driver package compatible with PostgreSQL. The class name and class structure in the driver are the same as those in the PostgreSQL driver. All the applications running on PostgreSQL can be smoothly transferred to the current system.

Driver Class
------------

Before creating a database connection, you need to load the database driver class **org.postgresql.Driver** (decompressed from **gsjdbc4.jar**).
