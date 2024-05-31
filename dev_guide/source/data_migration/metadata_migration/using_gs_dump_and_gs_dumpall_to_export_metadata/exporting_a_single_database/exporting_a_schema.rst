:original_name: dws_04_0273.html

.. _dws_04_0273:

.. _en-us_topic_0000001233883273:

Exporting a Schema
==================

You can use gs_dump to export data and all object definitions of a schema from GaussDB(DWS). You can export one or more specified schemas as needed. You can specify the information to be exported as follows:

-  Export full information of a schema, including its data and object definitions.
-  Export data of a schema, excluding its object definitions.
-  Export the object definitions of a schema, including the definitions of tables, stored procedures, and indexes.

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

#. Use gs_dump to run the following command to export the **hr** and **public** schemas.

   .. code-block::

      gs_dump -W Password -U jack -f /home//backup/MPPDB_schema_backup -p 8000 -h 10.10.10.100 human_resource -n hr -F d

   .. table:: **Table 1** Common parameters

      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------+
      | Parameter             | Description                                                                                                                                                                                                                                                                                           | Example Value                               |
      +=======================+=======================================================================================================================================================================================================================================================================================================+=============================================+
      | -U                    | Username for connecting to the database. If this parameter is not configured, the username of the connected database is used.                                                                                                                                                                         | -U jack                                     |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------+
      | -W                    | User password for database connection.                                                                                                                                                                                                                                                                | -W *Password*                               |
      |                       |                                                                                                                                                                                                                                                                                                       |                                             |
      |                       | -  This parameter is not required for database administrators if the trust policy is used for authentication.                                                                                                                                                                                         |                                             |
      |                       | -  If you connect to the database without specifying this parameter and you are not a database administrator, you will be prompted to enter the password.                                                                                                                                             |                                             |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------+
      | -f                    | Folder to store exported files. If this parameter is not specified, the exported files are stored in the standard output.                                                                                                                                                                             | -f /home//backup/MPPDB\ *\_*\ schema_backup |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------+
      | -p                    | Name extension of the TCP port on which the server is listening or the local Unix domain socket. This parameter is configured to ensure connections.                                                                                                                                                  | -p 8000                                     |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------+
      | -h                    | *Cluster address*: If a public network address is used for connection, set this parameter to **Public Network Address** or **Public Network Domain Name**. If a private network address is used for connection, set this parameter to **Private Network Address** or **Private Network Domain Name**. | -h 10.10.10.100                             |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------+
      | dbname                | Name of the database to be exported.                                                                                                                                                                                                                                                                  | human_resource                              |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------+
      | -n                    | Names of schemas to be exported. Data of the specified schemas will also be exported.                                                                                                                                                                                                                 | -  Single schema: **-n hr**                 |
      |                       |                                                                                                                                                                                                                                                                                                       | -  Multiple schemas: **-n hr -n public**    |
      |                       | -  Single schema: Enter **-n** *schemaname*.                                                                                                                                                                                                                                                          |                                             |
      |                       | -  Multiple schemas: Enter **-n** *schemaname* for each schema.                                                                                                                                                                                                                                       |                                             |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------+
      | -F                    | Format of exported files. The values of **-F** are as follows:                                                                                                                                                                                                                                        | -F d                                        |
      |                       |                                                                                                                                                                                                                                                                                                       |                                             |
      |                       | -  **p**: plain text                                                                                                                                                                                                                                                                                  |                                             |
      |                       | -  **c**: custom                                                                                                                                                                                                                                                                                      |                                             |
      |                       | -  **d**: directory                                                                                                                                                                                                                                                                                   |                                             |
      |                       | -  **t**: .tar                                                                                                                                                                                                                                                                                        |                                             |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------+

   For details about other parameters, see "gs_dump" in the *Tool* *Guide*.

Examples
--------

Example 1: Use gs_dump to run the following command to export full information of the **hr** schema. The exported files are compressed and stored in text format.

.. code-block::

   gs_dump -W password -U jack -f /home//backup/MPPDB_schema_backup.sql -p 8000 -h 10.10.10.100 human_resource -n hr -Z 6 -F p
   gs_dump[port=''][human_resource][2017-07-21 16:05:55]: dump database human_resource successfully
   gs_dump[port=''][human_resource][2017-07-21 16:05:55]: total time: 2425  ms

Example 2: Use gs_dump to run the following command to export data of the **hr** schema. The exported files are in .tar format.

.. code-block::

   gs_dump -W password -U jack -f /home//backup/MPPDB_schema_data_backup.tar -p 8000 -h 10.10.10.100 human_resource -n hr -a -F t
   gs_dump[port=''][human_resource][2018-11-14 15:07:16]: dump database human_resource successfully
   gs_dump[port=''][human_resource][2018-11-14 15:07:16]: total time: 1865  ms

Example 3: Use gs_dump to run the following command to export the definition of the **hr** schema. The exported files are stored in a directory.

.. code-block::

   gs_dump -W password -U jack -f /home//backup/MPPDB_schema_def_backup -p 8000 -h 10.10.10.100 human_resource -n hr -s -F d
   gs_dump[port=''][human_resource][2018-11-14 15:11:34]: dump database human_resource successfully
   gs_dump[port=''][human_resource][2018-11-14 15:11:34]: total time: 1652  ms

Example 4: Use gs_dump to run the following command to export the **human_resource** database excluding the **hr** schema. The exported files are in a custom format.

.. code-block::

   gs_dump -W password -U jack -f /home//backup/MPPDB_schema_backup.dmp -p 8000 -h 10.10.10.100 human_resource -N hr -F c
   gs_dump[port=''][human_resource][2017-07-21 16:06:31]: dump database human_resource successfully
   gs_dump[port=''][human_resource][2017-07-21 16:06:31]: total time: 2522  ms

Example 5: Use gs_dump to run the following command to export the object definitions of the **hr** and **public** schemas, encrypt the exported files, and store them in .tar format.

.. code-block::

   gs_dump -W password -U jack -f /home//backup/MPPDB_schema_backup1.tar -p 8000 -h 10.10.10.100 human_resource -n hr -n public -s --with-encryption AES128 --with-key 1234567812345678 -F t
   gs_dump[port=''][human_resource][2017-07-21 16:07:16]: dump database human_resource successfully
   gs_dump[port=''][human_resource][2017-07-21 16:07:16]: total time: 2132  ms

Example 6: Use gs_dump to run the following command to export the **human_resource** database excluding the **hr** and **public** schemas. The exported files are in a custom format.

.. code-block::

   gs_dump -W password -U jack -f /home//backup/MPPDB_schema_backup2.dmp -p 8000 -h 10.10.10.100 human_resource -N hr -N public -F c
   gs_dump[port=''][human_resource][2017-07-21 16:07:55]: dump database human_resource successfully
   gs_dump[port=''][human_resource][2017-07-21 16:07:55]: total time: 2296  ms

Example 7: Use gs_dump to run the following command to export all tables, including views, sequences, and foreign tables, in the **public** schema, and the **staffs** table in the **hr** schema, including data and table definition. The exported files are in a custom format.

.. code-block::

   gs_dump -W password -U jack -f /home//backup/MPPDB_backup3.dmp -p 8000 -h 10.10.10.100 human_resource -t public.* -t hr.staffs -F c
   gs_dump[port=''][human_resource][2018-12-13 09:40:24]: dump database human_resource successfully
   gs_dump[port=''][human_resource][2018-12-13 09:40:24]: total time: 896  ms
