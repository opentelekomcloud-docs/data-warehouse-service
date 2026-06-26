:original_name: dws_03_0027.html

.. _dws_03_0027:

How Is Data Stored in DWS?
==========================

DWS efficiently imports data from various sources through several modes. For details, see section "Importing Data" in the *Data Warehouse Service (DWS) Developer Guide*.

-  Importing data from the OBS

   Upload data to OBS and then export it to DWS clusters. Data formats such as CSV and TEXT are supported.

-  Inserting data with **INSERT** statements

   Use the gsql client tool provided by DWS or the JDBC/ODBC driver to write data to DWS from upper-layer applications. DWS supports complete database transaction-level CRUD operations. This is the simplest method and is applicable to scenarios with small data volume and low concurrency.

-  Importing data from MRS with MRS as the ETL.

-  Importing data with the **COPY FROM STDIN** command

   Run the **COPY FROM STDIN** command to write data to a table.

-  Importing data from a remote server to DWS using GDS

   Use the GDS data import function provided by DWS to import data files from a common file system (for example, an ECS).
