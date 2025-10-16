:original_name: dws_04_0278.html

.. _dws_04_0278:

.. _en-us_topic_0000001764491960:

Data Export By a User Without Required Permissions
==================================================

gs_dump and gs_dumpall use **-U** to specify the user that performs the export. If the specified user does not have the required permission, data cannot be exported. In this case, you can set **--role** in the export command to the role that has the permission. Then, gs_dump or gs_dumpall uses the specified role to export data.

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

#. Use gs_dump to export data of the **human_resource** database.

   User **jack** does not have the permission for exporting data of the **human_resource** database and the role **role1** has this permission. To export data of the **human_resource** database, you can set **--role** to **role1** in the export command. The exported files are in .tar format.

   .. code-block::

      gs_dump -U jack -W password -f /home//backup/MPPDB_backup.tar -p 8000 -h 10.10.10.100 human_resource --role role1 --rolepassword password -F t

   .. table:: **Table 1** Common parameters

      +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------+
      | Parameter             | Description                                                                                                                                                                                                                                                                                                                                       | Example Value (dbadmin)           |
      +=======================+===================================================================================================================================================================================================================================================================================================================================================+===================================+
      | -U                    | Username for database connection.                                                                                                                                                                                                                                                                                                                 | -U jack                           |
      +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------+
      | -W                    | User password for database connection.                                                                                                                                                                                                                                                                                                            | -W *Password*                     |
      |                       |                                                                                                                                                                                                                                                                                                                                                   |                                   |
      |                       | -  This parameter is not required for database administrators if the trust policy is used for authentication.                                                                                                                                                                                                                                     |                                   |
      |                       | -  If you connect to the database without specifying this parameter and you are not a database administrator, you will be prompted to enter the password.                                                                                                                                                                                         |                                   |
      +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------+
      | -f                    | Folder to store exported files. If this parameter is not specified, the exported files are stored in the standard output.                                                                                                                                                                                                                         | -f /home//backup/MPPDB_backup.tar |
      +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------+
      | -p                    | Name extension of the TCP port on which the server is listening or the local Unix domain socket. This parameter is configured to ensure connections.                                                                                                                                                                                              | -p 8000                           |
      +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------+
      | -h                    | *Cluster address*: If a public network address is used for connection, set this parameter to **Public Network Address** or **Public Network Domain Name**. If a private network address is used for connection, set this parameter to **Private Network Address** or **Private Network Domain Name**.                                             | -h 10.10.10.100                   |
      +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------+
      | dbname                | Name of the database to be exported.                                                                                                                                                                                                                                                                                                              | human_resource                    |
      +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------+
      | --role                | Role name for the export operation. After this parameter is set and gs_dump or gs_dumpall connects to the database, the **SET ROLE** command will be issued. When the user specified by **-U** does not have the permissions required by gs_dump or gs_dumpall, this parameter allows the user to switch to a role with the required permissions. | -r role1                          |
      +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------+
      | --rolepassword        | Role password.                                                                                                                                                                                                                                                                                                                                    | --rolepassword password           |
      +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------+
      | -F                    | Format of exported files. The values of **-F** are as follows:                                                                                                                                                                                                                                                                                    | -F t                              |
      |                       |                                                                                                                                                                                                                                                                                                                                                   |                                   |
      |                       | -  **p**: plain text                                                                                                                                                                                                                                                                                                                              |                                   |
      |                       | -  **c**: custom                                                                                                                                                                                                                                                                                                                                  |                                   |
      |                       | -  **d**: directory                                                                                                                                                                                                                                                                                                                               |                                   |
      |                       | -  **t**: .tar                                                                                                                                                                                                                                                                                                                                    |                                   |
      +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------+

   For details about other parameters, see "gs_dump" or "gs_dumpall" in the *Tool Guide*.

Examples
--------

Example 1: User **jack** does not have the permission for exporting data of the **human_resource** database and the role **role1** has this permission. To export data of the **human_resource** database, you can set **--role** to **role1** in the export command. The exported files are in .tar format.

.. code-block::

   human_resource=# CREATE USER jack IDENTIFIED BY "password";

   gs_dump -U jack -W password -f /home//backup/MPPDB_backup11.tar -p 8000 -h 10.10.10.100 human_resource --role role1 --rolepassword password -F t
   gs_dump[port='8000'][human_resource][2017-07-21 16:21:10]: dump database human_resource successfully
   gs_dump[port='8000'][human_resource][2017-07-21 16:21:10]: total time: 4239  ms

Example 2: User **jack** does not have the permission for exporting the **public** schema and the role **role1** has this permission. To export the **public** schema, you can set **--role** to **role1** in the export command. The exported files are in .tar format.

.. code-block::

   human_resource=# CREATE USER jack IDENTIFIED BY "1234@abc";

   gs_dump -U jack -W password -f /home//backup/MPPDB_backup12.tar -p 8000 -h 10.10.10.100 human_resource -n public --role role1 --rolepassword password -F t
   gs_dump[port='8000'][human_resource][2017-07-21 16:21:10]: dump database human_resource successfully
   gs_dump[port='8000'][human_resource][2017-07-21 16:21:10]: total time: 3278  ms

Example 3: User **jack** does not have the permission for exporting all databases in a cluster and the role **role1** has this permission. To export all databases, you can set **--role** to **role1** in the export command. The exported files are in text format.

.. code-block::

   human_resource=# CREATE USER jack IDENTIFIED BY "password";

   gs_dumpall -U jack -W password -f /home//backup/MPPDB_backup.sql -p 8000 -h 10.10.10.100 --role role1 --rolepassword password
   gs_dumpall[port='8000'][human_resource][2018-11-14 17:26:18]: dumpall operation successful
   gs_dumpall[port='8000'][human_resource][2018-11-14 17:26:18]: total time: 6437  ms
