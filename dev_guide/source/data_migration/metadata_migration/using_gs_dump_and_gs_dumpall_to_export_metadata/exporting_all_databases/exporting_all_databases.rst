:original_name: dws_04_0276.html

.. _dws_04_0276:

.. _en-us_topic_0000001233883223:

Exporting All Databases
=======================

You can use gs_dumpall to export full information of all databases in a cluster from GaussDB(DWS), including information about each database and global objects in the cluster. You can specify the information to be exported as follows:

-  Export full information of all databases, including information about each database and global objects (such as roles and tablespaces) in the cluster.

   You can use the exported information to create a same cluster containing the same databases, global objects, and data as the current one.

-  Export data of all databases, excluding all object definitions and global objects.

-  Export all object definitions of all databases, including the definitions of tablespaces, databases, functions, schemas, tables, indexes, and stored procedures.

   You can use the exported object definitions to quickly create a same cluster as the current one, containing the same databases and tablespaces but without data.

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

#. Use gs_dumpall to run the following command to export information of all databases.

   .. code-block::

      gs_dumpall -W password -U dbadmin -f /home/dbadmin/backup/MPPDB_backup.sql -p 8000 -h 10.10.10.100

   .. table:: **Table 1** Common parameters

      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------+
      | Parameter             | Description                                                                                                                                                                                                                                                                                           | Example Value                            |
      +=======================+=======================================================================================================================================================================================================================================================================================================+==========================================+
      | -U                    | Username for database connection. The user must be a cluster administrator.                                                                                                                                                                                                                           | -U dbadmin                               |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------+
      | -W                    | User password for database connection.                                                                                                                                                                                                                                                                | -W *Password*                            |
      |                       |                                                                                                                                                                                                                                                                                                       |                                          |
      |                       | -  This parameter is not required for database administrators if the trust policy is used for authentication.                                                                                                                                                                                         |                                          |
      |                       | -  If you connect to the database without specifying this parameter and you are not a database administrator, you will be prompted to enter the password.                                                                                                                                             |                                          |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------+
      | -f                    | Folder to store exported files. If this parameter is not specified, the exported files are stored in the standard output.                                                                                                                                                                             | -f /home/dbadmin/backup/MPPDB_backup.sql |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------+
      | -p                    | Name extension of the TCP port on which the server is listening or the local Unix domain socket. This parameter is configured to ensure connections.                                                                                                                                                  | -p 8000                                  |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------+
      | -h                    | *Cluster address*: If a public network address is used for connection, set this parameter to **Public Network Address** or **Public Network Domain Name**. If a private network address is used for connection, set this parameter to **Private Network Address** or **Private Network Domain Name**. | -h 10.10.10.100                          |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------+

   For details about other parameters, see "gs_dumpall" in the *Tool* *Guide*.

Examples
--------

Example 1: Use **gs_dumpall** to run the following command as the cluster administrator **dbadmin** to export information of all databases in a cluster. The exported files are in text format. After the command is executed, a large amount of output information will be displayed. **total time** will be displayed at the end of the information, indicating that the export is successful. In this example, only related output information is included.

.. code-block::

   gs_dumpall -W password -U dbadmin -f /home/dbadmin/backup/MPPDB_backup.sql -p 8000 -h 10.10.10.100
   gs_dumpall[port=''][2017-07-21 15:57:31]: dumpall operation successful
   gs_dumpall[port=''][2017-07-21 15:57:31]: total time: 9627  ms

Example 2: Use **gs_dumpall** to run the following command as the cluster administrator **dbadmin** to export definitions of all databases in a cluster. The exported files are in text format. After the command is executed, a large amount of output information will be displayed. **total time** will be displayed at the end of the information, indicating that the export is successful. In this example, only related output information is included.

.. code-block::

   gs_dumpall -W password -U dbadmin -f /home/dbadmin/backup/MPPDB_backup.sql -p 8000 -h 10.10.10.100 -s
   gs_dumpall[port=''][2018-11-14 11:28:14]: dumpall operation successful
   gs_dumpall[port=''][2018-11-14 11:28:14]: total time: 4147  ms

Example 3: Use gs_dumpall to run the following command export data of all databases in a cluster, encrypt the exported files, and store them in text format. After the command is executed, a large amount of output information will be displayed. **total time** will be displayed at the end of the information, indicating that the export is successful. In this example, only related output information is included.

.. code-block::

   gs_dumpall -W password -U dbadmin -f /home/dbadmin/backup/MPPDB_backup.sql -p 8000 -h 10.10.10.100 -a --with-encryption AES128 --with-key 1234567812345678
   gs_dumpall[port=''][2018-11-14 11:32:26]: dumpall operation successful
   gs_dumpall[port=''][2018-11-14 11:23:26]: total time: 4147  ms
