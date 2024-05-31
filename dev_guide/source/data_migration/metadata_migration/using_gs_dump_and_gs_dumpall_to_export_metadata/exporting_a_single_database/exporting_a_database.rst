:original_name: dws_04_0272.html

.. _dws_04_0272:

.. _en-us_topic_0000001233761733:

Exporting a Database
====================

You can use gs_dump to export data and all object definitions of a database from GaussDB(DWS). You can specify the information to be exported as follows:

-  Export full information of a database, including its data and all object definitions.

   You can use the exported information to create a same database containing the same data as the current one.

-  Export all object definitions of a database, including the definitions of the database, functions, schemas, tables, indexes, and stored procedures.

   You can use the exported object definitions to quickly create a same database as the current one, without data.

-  Export data of a database.

Procedure
---------

#. Prepare an ECS as the gsql client host. For details, see "Preparing an ECS as the gsql Client Host" in the Data Warehouse Service (DWS) User Guide.

#. Download the gsql client and use an SSH transfer tool (such as WinSCP) to upload it to the Linux server where gsql is to be installed. For details, see "Downloading the Client" in *Data Warehouse Service User Guide*.

   The user who uploads the client must have the full control permission on the target directory on the host to which the client is uploaded.

#. Run the following commands to decompress the client:

   .. code-block::

      cd <Path_for_storing_the_client>
      unzip dws_client_8.x.x_redhat_x64.zip

   Where,

   -  <*Path_for_storing_the_client*>: Replace it with the actual path.
   -  *dws_client_8.1.x_redhat_x86.zip*: This is the client tool package of **RedHat x86**. Replace it with the actual one.

#. Run the following command to configure the GaussDB(DWS) client:

   .. code-block::

      source gsql_env.sh

   If the following information is displayed, the GaussDB(DWS) client is successfully configured:

   .. code-block::

      All things done.

#. Use gs_dump to export data of the database **gaussdb**.

   .. code-block::

      gs_dump -W password -U jack -f /home//backup/postgres_backup.tar -p 8000 gaussdb  -h 10.10.10.100 -F t

   .. table:: **Table 1** Common parameters

      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------+
      | Parameter             | Description                                                                                                                                                                                                                                                                                           | Example Value                             |
      +=======================+=======================================================================================================================================================================================================================================================================================================+===========================================+
      | -U                    | Username for connecting to the database. If this parameter is not configured, the username of the connected database is used.                                                                                                                                                                         | -U jack                                   |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------+
      | -W                    | User password for database connection.                                                                                                                                                                                                                                                                | -W *Password*                             |
      |                       |                                                                                                                                                                                                                                                                                                       |                                           |
      |                       | -  This parameter is not required for database administrators if the trust policy is used for authentication.                                                                                                                                                                                         |                                           |
      |                       | -  If you connect to the database without specifying this parameter and you are not a database administrator, you will be prompted to enter the password.                                                                                                                                             |                                           |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------+
      | -f                    | Folder to store exported files. If this parameter is not specified, the exported files are stored in the standard output.                                                                                                                                                                             | -f /home//backup/*postgres*\ \_backup.tar |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------+
      | -p                    | Name extension of the TCP port on which the server is listening or the local Unix domain socket. This parameter is configured to ensure connections.                                                                                                                                                  | -p 8000                                   |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------+
      | -h                    | *Cluster address*: If a public network address is used for connection, set this parameter to **Public Network Address** or **Public Network Domain Name**. If a private network address is used for connection, set this parameter to **Private Network Address** or **Private Network Domain Name**. | -h 10.10.10.100                           |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------+
      | dbname                | Name of the database to be exported.                                                                                                                                                                                                                                                                  | gaussdb                                   |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------+
      | -F                    | Format of exported files. The values of **-F** are as follows:                                                                                                                                                                                                                                        | -F t                                      |
      |                       |                                                                                                                                                                                                                                                                                                       |                                           |
      |                       | -  **p**: plain text                                                                                                                                                                                                                                                                                  |                                           |
      |                       | -  **c**: custom                                                                                                                                                                                                                                                                                      |                                           |
      |                       | -  **d**: directory                                                                                                                                                                                                                                                                                   |                                           |
      |                       | -  **t**: .tar                                                                                                                                                                                                                                                                                        |                                           |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------+

   For details about other parameters, see "gs_dump" in the *Tool* *Guide*.

Examples
--------

Example 1: Use gs_dump to run the following command to export full information of the database **gaussdb** and compress the exported files in SQL format.

.. code-block::

   gs_dump -W password -U jack -f /home//backup/postgres_backup.sql -p 8000  -h 10.10.10.100 gaussdb -Z 8 -F p
   gs_dump[port=''][gaussdb][2017-07-21 15:36:13]: dump database gaussdb successfully
   gs_dump[port=''][gaussdb][2017-07-21 15:36:13]: total time: 3793  ms

Example 2: Use gs_dump to run the following command to export data of the database **gaussdb**, excluding object definitions. The exported files are in a custom format.

.. code-block::

   gs_dump -W Password -U jack -f /home//backup/postgres_data_backup.dmp -p 8000 -h 10.10.10.100 gaussdb -a -F c
   gs_dump[port=''][gaussdb][2017-07-21 15:36:13]: dump database gaussdb successfully
   gs_dump[port=''][gaussdb][2017-07-21 15:36:13]: total time: 3793  ms

Example 3: Use gs_dump to run the following command to export object definitions of the database **gaussdb**. The exported files are in SQL format.

.. code-block::

   --Before the export, the nation table contains data.
   select n_nationkey,n_name,n_regionkey from nation limit 3;
    n_nationkey |          n_name           | n_regionkey
   -------------+---------------------------+-------------
              0 | ALGERIA                   |           0
              3 | CANADA                    |           1
             11 | IRAQ                      |           4
   (3 rows)

   gs_dump -W password -U jack -f /home//backup/postgres_def_backup.sql -p 8000 -h 10.10.10.100 gaussdb -s -F p
   gs_dump[port=''][gaussdb][2017-07-20 15:04:14]: dump database gaussdb successfully
   gs_dump[port=''][gaussdb][2017-07-20 15:04:14]: total time: 472 ms

Example 4: Use gs_dump to run the following command to export object definitions of the database **gaussdb**. The exported files are in text format and are encrypted.

.. code-block::

   gs_dump -W password -U jack -f /home//backup/postgres_def_backup.sql -p 8000 -h 10.10.10.100 gaussdb --with-encryption AES128 --with-key 1234567812345678 -s -F p
   gs_dump[port=''][gaussdb][2018-11-14 11:25:18]: dump database gaussdb successfully
   gs_dump[port=''][gaussdb][2018-11-14 11:25:18]: total time: 1161  ms
