:original_name: dws_04_0274.html

.. _dws_04_0274:

Exporting a Table
=================

You can use gs_dump to export data and all object definitions of a table-level object from GaussDB(DWS). Views, sequences, and foreign tables are special tables. You can export one or more specified tables as needed. You can specify the information to be exported as follows:

-  Export full information of a table, including its data and definition.
-  Export data of a table.
-  Export the definition of a table.

Procedure
---------

#. Prepare an ECS as the gsql client host. For details, see "Preparing an ECS as the gsql Client Host" in the *Data Warehouse Service (DWS) User Guide*.

#. Download the gsql client and use an SSH transfer tool (such as WinSCP) to upload it to the Linux server where gsql is to be installed. For details, see "Downloading the Client" in *Data Warehouse Service User Guide*.

   The user who uploads the client must have the full control permission on the target directory on the host to which the client is uploaded.

#. Run the following commands to decompress the client:

   .. code-block::

      cd <Path_for_storing_the_client>
      unzip dws_client_8.1.x_redhat_x64.zip

   Where,

   -  <*Path_for_storing_the_client*>: Replace it with the actual path.
   -  *dws_client_8.1.x_redhat_x86.zip*: This is the client tool package of **RedHat x86**. Replace it with the actual one.

#. Run the following command to configure the GaussDB(DWS) client:

   .. code-block::

      source gsql_env.sh

   If the following information is displayed, the GaussDB(DWS) client is successfully configured:

   .. code-block::

      All things done.

#. Use gs_dump to run the following command to export the **hr.staffs** and **hr.employments** tables.

   .. code-block::

      gs_dump -W password -U jack -f /home//backup/MPPDB_table_backup -p 8000 -h 10.10.10.100 human_resource -t hr.staffs -F d

   .. table:: **Table 1** Common parameters

      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------+
      | Parameter             | Description                                                                                                                                                                                                                                                                                           | Example Value                                          |
      +=======================+=======================================================================================================================================================================================================================================================================================================+========================================================+
      | -U                    | Username for connecting to the database. If this parameter is not configured, the username of the connected database is used.                                                                                                                                                                         | -U jack                                                |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------+
      | -W                    | User password for database connection.                                                                                                                                                                                                                                                                | -W *password*                                          |
      |                       |                                                                                                                                                                                                                                                                                                       |                                                        |
      |                       | -  This parameter is not required for database administrators if the trust policy is used for authentication.                                                                                                                                                                                         |                                                        |
      |                       | -  If you connect to the database without specifying this parameter and you are not a database administrator, you will be prompted to enter the password.                                                                                                                                             |                                                        |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------+
      | -f                    | Folder to store exported files. If this parameter is not specified, the exported files are stored in the standard output.                                                                                                                                                                             | -f /home//backup/MPPDB_table_backup                    |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------+
      | -p                    | Name extension of the TCP port on which the server is listening or the local Unix domain socket. This parameter is configured to ensure connections.                                                                                                                                                  | -p 8000                                                |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------+
      | -h                    | *Cluster address*: If a public network address is used for connection, set this parameter to **Public Network Address** or **Public Network Domain Name**. If a private network address is used for connection, set this parameter to **Private Network Address** or **Private Network Domain Name**. | -h 10.10.10.100                                        |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------+
      | dbname                | Name of the database to be exported.                                                                                                                                                                                                                                                                  | human_resource                                         |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------+
      | -t                    | Table (or view, sequence, foreign table) to be exported. You can specify multiple tables by listing them or using wildcard characters. When you use wildcard characters, quote wildcard patterns with single quotation marks ('') to prevent the shell from expanding the wildcard characters.        | -  Single table: **-t hr.staffs**                      |
      |                       |                                                                                                                                                                                                                                                                                                       | -  Multiple tables: **-t hr.staffs -t hr.employments** |
      |                       | -  Single table: Enter **-t** *schema.table*.                                                                                                                                                                                                                                                         |                                                        |
      |                       | -  Multiple tables: Enter **-t** *schema.table* for each table.                                                                                                                                                                                                                                       |                                                        |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------+
      | -F                    | Format of exported files. The values of **-F** are as follows:                                                                                                                                                                                                                                        | -F d                                                   |
      |                       |                                                                                                                                                                                                                                                                                                       |                                                        |
      |                       | -  **p**: plain text                                                                                                                                                                                                                                                                                  |                                                        |
      |                       | -  **c**: custom                                                                                                                                                                                                                                                                                      |                                                        |
      |                       | -  **d**: directory                                                                                                                                                                                                                                                                                   |                                                        |
      |                       | -  **t**: .tar                                                                                                                                                                                                                                                                                        |                                                        |
      +-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------+

   For details about other parameters, see "gs_dump" in the *Tool* *Guide*.

Examples
--------

Example 1: Use gs_dump to run the following command to export full information of the **hr.staffs** table. The exported files are in text format.

.. code-block::

   gs_dump -W password -U jack -f /home//backup/MPPDB_table_backup.sql -p 8000 -h 10.10.10.100 human_resource -t hr.staffs -Z 6 -F p
   gs_dump[port=''][human_resource][2017-07-21 17:05:10]: dump database human_resource successfully
   gs_dump[port=''][human_resource][2017-07-21 17:05:10]: total time: 3116  ms

Example 2: Use gs_dump to run the following command to export data of the **hr.staffs** table. The exported files are in .tar format.

.. code-block::

   gs_dump -W password -U jack -f /home//backup/MPPDB_table_data_backup.tar -p 8000 -h 10.10.10.100 human_resource -t hr.staffs -a -F t
   gs_dump[port=''][human_resource][2017-07-21 17:04:26]: dump database human_resource successfully
   gs_dump[port=''][human_resource][2017-07-21 17:04:26]: total time: 2570  ms

Example 3: Use gs_dump to run the following command to export the definition of the **hr.staffs** table. The exported files are stored in a directory.

.. code-block::

   gs_dump -W password -U jack -f /home//backup/MPPDB_table_def_backup -p 8000 -h 10.10.10.100 human_resource -t hr.staffs -s -F d
   gs_dump[port=''][human_resource][2017-07-21 17:03:09]: dump database human_resource successfully
   gs_dump[port=''][human_resource][2017-07-21 17:03:09]: total time: 2297  ms

Example 4: Use gs_dump to run the following command to export the **human_resource** database excluding the **hr.staffs** table. The exported files are in a custom format.

.. code-block::

   gs_dump -W password -U jack -f /home//backup/MPPDB_table_backup4.dmp -p 8000 -h 10.10.10.100 human_resource -T hr.staffs -F c
   gs_dump[port=''][human_resource][2017-07-21 17:14:11]: dump database human_resource successfully
   gs_dump[port=''][human_resource][2017-07-21 17:14:11]: total time: 2450  ms

Example 5: Use gs_dump to run the following command to export the **hr.staffs** and **hr.employments** tables. The exported files are in text format.

.. code-block::

   gs_dump -W password -U jack -f /home//backup/MPPDB_table_backup1.sql -p 8000 -h 10.10.10.100 human_resource -t hr.staffs -t hr.employments -F p
   gs_dump[port=''][human_resource][2017-07-21 17:19:42]: dump database human_resource successfully
   gs_dump[port=''][human_resource][2017-07-21 17:19:42]: total time: 2414  ms

Example 6: Use gs_dump to run the following command to export the **human_resource** database excluding the **hr.staffs** and **hr.employments** tables. The exported files are in text format.

.. code-block::

   gs_dump -W password -U jack -f /home//backup/MPPDB_table_backup2.sql -p 8000 -h 10.10.10.100 human_resource -T hr.staffs -T hr.employments -F p
   gs_dump[port=''][human_resource][2017-07-21 17:21:02]: dump database human_resource successfully
   gs_dump[port=''][human_resource][2017-07-21 17:21:02]: total time: 3165  ms

Example 7: Use gs_dump to run the following command to export data and definition of the **hr.staffs** table, and the definition of the **hr.employments** table. The exported files are in .tar format.

.. code-block::

   gs_dump -W password -U jack -f /home//backup/MPPDB_table_backup3.tar -p 8000 -h 10.10.10.100 human_resource -t hr.staffs -t hr.employments --exclude-table-data hr.employments -F t
   gs_dump[port=''][human_resource][2018-11-14 11:32:02]: dump database human_resource successfully
   gs_dump[port=''][human_resource][2018-11-14 11:32:02]: total time: 1645  ms

Example 8: Use gs_dump to run the following command to export data and definition of the **hr.staffs** table, encrypt the exported files, and store them in text format.

.. code-block::

   gs_dump -W password -U jack -f /home//backup/MPPDB_table_backup4.sql -p 8000 -h 10.10.10.100 human_resource -t hr.staffs --with-encryption AES128 --with-key 1212121212121212 -F p
   gs_dump[port=''][human_resource][2018-11-14 11:35:30]: dump database human_resource successfully
   gs_dump[port=''][human_resource][2018-11-14 11:35:30]: total time: 6708  ms

Example 9: Use gs_dump to run the following command to export all tables, including views, sequences, and foreign tables, in the **public** schema, and the **staffs** table in the **hr** schema, including data and table definition. The exported files are in a custom format.

.. code-block::

   gs_dump -W password -U jack -f /home//backup/MPPDB_table_backup5.dmp -p 8000 -h 10.10.10.100 human_resource -t public.* -t hr.staffs -F c
   gs_dump[port=''][human_resource][2018-12-13 09:40:24]: dump database human_resource successfully
   gs_dump[port=''][human_resource][2018-12-13 09:40:24]: total time: 896  ms

Example 10: Use gs_dump to run the following command to export the definition of the view referencing to the **test1** table in the **t1** schema. The exported files are in a custom format.

.. code-block::

   gs_dump -W password -U jack -f /home//backup/MPPDB_view_backup6 -p 8000 -h 10.10.10.100 human_resource -t t1.test1 --include-depend-objs --exclude-self -F d
   gs_dump[port=''][jack][2018-11-14 17:21:18]: dump database human_resource successfully
   gs_dump[port=''][jack][2018-11-14 17:21:23]: total time: 4239  ms
