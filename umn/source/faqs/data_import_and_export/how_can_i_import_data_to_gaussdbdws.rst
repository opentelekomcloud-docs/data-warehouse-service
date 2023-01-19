:original_name: dws_03_0027.html

.. _dws_03_0027:

How Can I Import Data to GaussDB(DWS)?
======================================

GaussDB(DWS) supports efficient data import from multiple data sources. The following lists typical data import modes. For details, see "Import Modes" in the *Data Warehouse Service (DWS) Developer Guide*.

-  Importing data from the OBS

   Upload data to OBS and then export it to GaussDB(DWS) clusters. Data formats such as CSV and TEXT are supported.

-  Inserting data with **INSERT** statements

   Use the gsql client tool provided by GaussDB(DWS) or the JDBC/ODBC driver to write data to GaussDB(DWS) from upper-layer applications. GaussDB(DWS) supports complete database transaction-level CRUD operations. This is the simplest method and is applicable to scenarios with small data volume and low concurrency.

-  Importing data from MRS with MRS as the ETL.

-  Importing data with the **COPY FROM STDIN** command

   Run the **COPY FROM STDIN** command to write data to a table.

-  Importing data from a remote server to GaussDB(DWS) using GDS

   Use the GDS data import function provided by GaussDB(DWS) to import data files from a common file system (for example, an ECS).
