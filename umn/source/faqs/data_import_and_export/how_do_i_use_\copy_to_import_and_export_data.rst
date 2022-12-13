:original_name: dws_03_0073.html

.. _dws_03_0073:

How Do I Use \\Copy to Import and Export Data?
==============================================

GaussDB(DWS) is a fully managed service on the cloud. Users cannot log in to the background to import or export data by using **COPY**, so the **COPY** syntax is disabled. You are advised to store data files on OBS and use OBS foreign tables to import data. If you want to use **COPY** to import and export data, perform the following operations:

#. Place the data file on the client.

#. Use gsql to connect to the target cluster.

#. Run the following command to import data. Enter the directory name and file name of the data file on the client and specify the import option in **with**. The command is almost the same as the common **COPY** command. You only need to add a backslash (\\) before the command. When the data is successfully imported, no notification will be displayed.

   .. code-block::

      \copy table_name from '/directory_name/file_name' with(...);

#. Run the following command to export data to a local file. Retain the default settings of parameters.

   .. code-block::

      \copy table_name to '/directory_name/file_name';

#. Specify the **copy_option** parameter to export data to a CSV file.

   .. code-block::

      \copy table_name to '/directory_name/file_name' CSV;

#. Use **with** to specify parameters, exporting data as CSV files that use vertical bars (|) as delimiters.

   .. code-block::

      \copy table_name to '/directory_name/file_name' with(format 'csv',delimiter '|') ;
