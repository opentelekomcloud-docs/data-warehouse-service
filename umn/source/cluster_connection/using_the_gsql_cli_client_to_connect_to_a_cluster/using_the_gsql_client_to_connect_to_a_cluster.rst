:original_name: dws_01_0037.html

.. _dws_01_0037:

Using the gsql Client to Connect to a Cluster
=============================================

This section describes how to connect to a database through an SQL client after you create a data warehouse cluster and before you use the cluster's database. GaussDB(DWS) provides the gsql client that matches the cluster version for you to access the cluster using the cluster's public or private network address.

Using the gsql CLI Client to Connect to a Cluster
-------------------------------------------------

#. Prepare a Linux ECS to install and run the gsql client.

   For details, see :ref:`Preparing an ECS as the gsql Client Host <dws_01_0128>`.

#. Download the gsql client by referring to :ref:`Downloading the Client <dws_01_0031>`, and use an SSH file transfer tool (such as WinSCP) to upload the client to a target Linux host.

   The user who uploads the client must have the full control permission on the target directory on the host to which the client is uploaded.

   Alternatively, you can remotely log in to the Linux host where the gsql is to be installed in SSH mode and run the following command in the Linux command window to download the gsql client:

   .. code-block::

      wget https://obs.otc.t-systems.com/dws/download/dws_client_redhat_x64.zip --no-check-certificate

#. Use the SSH tool to remotely log in to the host where the client is installed.

   For details about how to log in to an ECS, see "ECSs> Logging In to a Linux ECS > Login Using an SSH Password" in the *Elastic Cloud Server User Guide*.

#. (Optional) To connect to the cluster in SSL mode, configure SSL authentication parameters on the host where the client is installed. For details, see :ref:`Establishing Secure TCP/IP Connections in SSL Mode <dws_01_0038>`.

   .. note::

      The SSL connection mode is more secure than the non-SSL mode. You are advised to connect the client to the cluster in SSL mode.

#. Run the following commands to decompress the client:

   .. code-block::

      cd <Path for saving the client>
      unzip dws_client_8.1.x_redhat_x64.zip

   In the preceding commands:

   -  <*Path_for_storing_the_client*>: Replace it with the actual path.
   -  *dws_client_8.1.x_redhat_x64.zip*: This is the client tool package name of **RedHat x86**. Replace it with the actual name.

#. Run the following command to configure the GaussDB(DWS) client:

   .. code-block::

      source gsql_env.sh

   If the following information is displayed, the GaussDB(DWS) client is successfully configured:

   .. code-block::

      All things done.

#. Connect to the database in the GaussDB(DWS) cluster using the gsql client. Replace the values of each parameter with actual values.

   .. code-block::

      gsql -d <Database_name> -h <Cluster_address> -U <Database_user> -p <Database_port> -r

   The parameters are described as follows:

   -  *Database_name*: Enter the name of the database to be connected. If you use the client to connect to the cluster for the first time, enter the default database **gaussdb**.
   -  *Cluster_address*: For details about how to obtain this address, see :ref:`Obtaining the Cluster Connection Address <dws_01_0033>`. If a public network address is used for connection, set this parameter to **Public Network Address** or **Public Network Domain Name**. If a private network address is used for connection, set this parameter to **Private Network Address** or **Private Network Domain Name**.
   -  *Database_user*: Enter the username of the cluster's database. If you use the client to connect to the cluster for the first time, set this parameter to the default database administrator configured during cluster creation, for example, **dbadmin**.
   -  *Database_port*: Enter the database port set during cluster creation.

   For example, run the following command to connect to the default database **gaussdb** in the GaussDB(DWS) cluster:

   .. code-block::

      gsql -d gaussdb -h 10.168.0.74 -U dbadmin -p 8000 -W password -r

   If the following information is displayed, the connection succeeded:

   ::

      gaussdb=>

gsql Command Reference
----------------------

For more information about the gsql commands, see the *Data Warehouse Service (DWS) Tool Guide*.

(Optional) Importing TPC-DS Sample Data Using gsql
--------------------------------------------------

GaussDB(DWS) users can import data from external sources to data warehouse clusters. This section describes how to import sample data from OBS to a data warehouse cluster and perform querying and analysis operations on the sample data. The sample data is generated based on the standard TPC-DS benchmark test.

TPC-DS is the benchmark for testing the performance of decision support. With TPC-DS test data and cases, you can simulate complex scenarios, such as big data set statistics, report generation, online query, and data mining, to better understand functions and performance of database applications.

#. Use the SSH remote connection tool to log in to the host where the gsql client is installed and go to the gsql directory. The **/opt** directory is used as an example for storing the gsql client.

   .. code-block::

      cd /opt

#. Switch to the specified directory and set the AK and SK for importing sample data and the OBS access address.

   .. code-block::

      cd sample
      /bin/bash setup.sh -ak <Access_Key_Id> -sk <Secret_Access_Key> -obs_location obs.otc.t-systems.com

   If the following information is displayed, the settings are successful:

   .. code-block::

      setup successfully!

   .. note::

      *<Access_Key_Id>* and *<Secret_Access_Key>*: indicate the AK and SK, respectively. For details about how to obtain the AK and SK, see "Data Import > Concurrently Importing Data from OBS > Creating Access Keys (AK and SK)" in the *Data Warehouse Service (DWS) Developer Guide*. Then, replace the parameters in the statements with the obtained values.

#. Go back to previous directory and run the gsql environment variables.

   .. code-block::

      cd ..
      source gsql_env.sh
      cd bin

#. Import the sample data to the data warehouse.

   Command format:

   .. code-block::

      gsql -d <Database name> -h <Public network address of the cluster> -U <Administrator> -p <Data warehouse port number> -f <Path for storing the sample data script> -r

   Sample command:

   .. code-block::

      gsql -d gaussdb -h 10.168.0.74 -U dbadmin -p 8000 -f /opt/sample/tpcds_load_data_from_obs.sql -r

   .. note::

      In the preceding command, sample data script **tpcds_load_data_from_obs.sql** is stored in the sample directory (for example, **/opt/sample/**) of the GaussDB(DWS) client.

   After you enter the database administrator password and successfully connect to the database in the cluster, the system will automatically create a foreign table to associate the sample data outside the cluster. Then, the system creates a target table for saving the sample data and imports the data to the target table using the foreign table.

   The time required for importing a large dataset depends on the current GaussDB(DWS) cluster specifications. Generally, the import takes about 10 to 20 minutes. If information similar to the following is displayed, the import is successful.

   .. code-block::

      Time:1845600.524 ms

5. In the Linux command window, run the following commands to switch to a specific directory and query the sample data:

   .. code-block::

      cd /opt/sample/query_sql/
      /bin/bash tpcds100x.sh

6. Enter the cluster's public network IP address, access port, database name, user who accesses the database, and password of the user as prompted.

   -  The default database name is **gaussdb**.
   -  Use the database administrator username and password configured during cluster creation as the username and password for accessing the database.

   After the query is complete, a directory for storing the query result, such as **query_output_20170914_072341**, will be generated in the current query directory, for example, **sample/query_sql/**.
