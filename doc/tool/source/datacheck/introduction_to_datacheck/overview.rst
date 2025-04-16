:original_name: dws_07_0202.html

.. _dws_07_0202:

Overview
========

To ensure a complete and accurate database migration when switching to GaussDB(DWS) database, you need to migrate your data and SQL scripts, and check the data after migration.

DataCheck is a command-line tool that works on Linux and Windows OSs. It offers fast, reliable, and user-friendly data check services. By connecting to both the source and destination databases, DataCheck checks table data in both locations to ensure consistency before and after migration.

DataCheck requires a connection to the database and can perform data check in offline mode. The verification results are sequentially recorded in an Excel file, and any errors encountered during the operation are logged for efficient fault diagnosis.

Supported Source Databases
--------------------------

-  MySQL (including AnalyticDB for MySQL)
-  PostgreSQL
-  GaussDB(DWS)

Data Check Process
------------------

The DataCheck process is as follows:

#. Download the DataCheck tool package to a Linux or Windows server and decompress it.
#. Run the encryption command to encrypt the login passwords of the source and destination databases.
#. Configure the **dbinfo.properties** file, including connection details for the source and destination databases, as well as function switch information.
#. Edit the **check_input.xlsx** file to input the schema, source database table name, GaussDB(DWS) table name, and check level.
#. Run the DataCheck startup command to verify data. The verification result is saved in **check_input_result.xlsx**.


.. figure:: /_static/images/en-us_image_0000002079758432.png
   :alt: **Figure 1** DataCheck process

   **Figure 1** DataCheck process
