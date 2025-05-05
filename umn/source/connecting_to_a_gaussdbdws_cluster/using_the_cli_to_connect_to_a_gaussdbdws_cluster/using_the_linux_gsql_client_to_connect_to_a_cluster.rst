:original_name: dws_01_0037.html

.. _dws_01_0037:

Using the Linux gsql Client to Connect to a Cluster
===================================================

This section describes how to connect to a database through an SQL client after you create a data warehouse cluster and before you use the cluster's database. GaussDB(DWS) provides the Linux gsql client that matches the cluster version for you to access the cluster through the cluster's public or private network address.

The gsql command line client provided by GaussDB(DWS) runs on Linux. Before using it to remotely connect to a GaussDB(DWS) cluster, you need to prepare a Linux server for installing and running the gsql client. If you use a public network address to access the cluster, you can install the Linux gsql client on your own Linux server. Ensure that the Linux server has a public network address. If no EIPs are configured for your GaussDB(DWS) cluster, you are advised to create a Linux ECS for convenience purposes. For more information, see :ref:`(Optional) Preparing an ECS as the gsql Client Server <en-us_topic_0000002203312241__section634463134117>`.

.. _en-us_topic_0000002203312241__section634463134117:

(Optional) Preparing an ECS as the gsql Client Server
-----------------------------------------------------

For how to create an ECS, see "Getting Started" > "Creating an ECS" in the *Elastic Cloud Server User Guide*.

The created ECS must meet the following requirements:

-  ECS and GaussDB(DWS) clusters must belong to the same region and AZ.

-  If you use the gsql client provided by GaussDB(DWS) to connect to the GaussDB(DWS) cluster, the ECS image must meet the following requirements:

   The image's OS must be one of the following Linux OSs supported by the gsql client:

   -  The **Redhat x86_64** client can be used on the following OSs:

      -  RHEL 6.4~7.6
      -  CentOS 6.4~7.4
      -  EulerOS 2.3

   -  The **SUSE x86_64** client can be used on the following OSs:

      -  SLES 11.1~11.4
      -  SLES 12.0~12.3

-  If the client accesses the cluster using the private network address, ensure that the created ECS is in the same VPC as the GaussDB(DWS) cluster.

   For details about VPC operations, see "VPC and Subnet" in the *Virtual Private Cloud User Guide*.

-  If the client accesses the cluster using the public network address, ensure that both the created ECS and GaussDB(DWS) cluster have an EIP.

   When creating an ECS, set **EIP** to **Automatically assign** or **Specify**.

-  The security group rules of the ECS must enable communication between the ECS and the port that the GaussDB(DWS) cluster uses to provide services.

   For details about security group operations, see "Security Group" in the *Virtual Private Cloud User Guide*.

   Ensure that the security group of the ECS contains rules meeting the following requirements. If the rules do not exist, add them to the security group:

   -  **Transfer Direction**: **Outbound**
   -  Protocol: The protocol must contain TCP. For example, **TCP** or **All**.
   -  **Port**: The value must contain the database port that provides services in the GaussDB(DWS) cluster. For example, set this parameter to **1-65535** or a specific GaussDB(DWS) database port.
   -  Destination: The IP address set here must contain the IP address of the GaussDB(DWS) cluster to be connected. **0.0.0.0/0** indicates any IP address.

-  The security group rules of the data warehouse cluster must ensure that GaussDB(DWS) can receive network access requests from clients.

   Ensure that the cluster's security group contains rules meeting the following requirements. If the rules do not exist, add them to the security group:

   -  **Transfer Direction**: **Inbound**
   -  **Protocol**: The protocol must contain TCP. For example, **TCP** or **All**.
   -  **Port**: Set this parameter to the servicing database port of the GaussDB(DWS) cluster. Example: 8000.
   -  Source IP Address: The IP address set here must contain the IP address of the GaussDB(DWS) client host. Example: 192.168.0.10/32.

Downloading the Linux gsql Client and Connecting to a Cluster
-------------------------------------------------------------

#. Download the Linux gsql client by referring to :ref:`Downloading the Client <dws_01_0031>`, and use an SSH file transfer tool (such as WinSCP) to upload the client to a target Linux server.

   You are advised to download the gsql tool that matches the cluster version. That is, use gsql 8.1.x for clusters of 8.1.0 or later, and use gsql 8.2.x for clusters of 8.2.0 or later. To download gsql 8.2.x, replace **dws_client_8.1.x_redhat_x64.zip** with **dws_client_8.2.x_redhat_x64.zip**. The **dws_client_8.1.x_redhat_x64.zip** is used as an example.

   The user who uploads the client must have the full control permission on the target directory on the host to which the client is uploaded.

   Alternatively, you can remotely manage the Linux server where the gsql is to be installed in SSH mode and run the following command in the Linux command window to download the Linux gsql client:

   ::

      wget https://obs.otc.t-systems.com/dws/download/dws_client_8.1.x_redhat_x64.zip --no-check-certificate

#. Use the SSH tool to remotely manage the host where the client is installed.

   For details about how to log in to an ECS, see "ECSs> Logging In to a Linux ECS > Login Using an SSH Password" in the *Elastic Cloud Server User Guide*.

#. (Optional) To connect to the cluster in SSL mode, configure SSL authentication parameters on the server where the client is installed. For details, see :ref:`Establishing Secure TCP/IP Connections in SSL Mode <dws_01_0038>`.

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

   If the following information is displayed, the gsql client is successfully configured:

   .. code-block::

      All things done.

#. Connect to the database in the GaussDB(DWS) cluster using the gsql client. Replace the values of each parameter with actual values.

   .. code-block::

      gsql -d <Database_name> -h <Cluster_address> -U <Database_user> -p <Database_port> -W <Cluster_password> -r

   The parameters are described as follows:

   -  *Database_name*: Enter the name of the database to be connected. If you use the client to connect to the cluster for the first time, enter the default database **gaussdb**.
   -  *Cluster_address*: For details about how to obtain this address, see :ref:`Obtaining the Connection Address of a GaussDB(DWS) Cluster <dws_01_0033>`. If a public network address is used for connection, set this parameter to **Public Network Address** or **Public Network Domain Name**. If a private network address is used for connection, set this parameter to **Private Network Address** or **Private Network Domain Name**.
   -  *Database_user*: Enter the username of the cluster's database. If you use the client to connect to the cluster for the first time, set this parameter to the default administrator configured during cluster creation, for example, **dbadmin**.
   -  *Database_port*: Enter the database port set during cluster creation.

   For example, run the following command to connect to the default database **gaussdb** in the GaussDB(DWS) cluster:

   ::

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

#. Use the SSH remote connection tool to log in to the server where the gsql client is installed and go to the gsql directory. The **/opt** directory is used as an example for storing the gsql client.

   .. code-block::

      cd /opt

#. Switch to the specified directory and set the AK and SK for importing sample data and the OBS access address.

   ::

      cd sample
      /bin/bash setup.sh -ak <Access_Key_Id> -sk <Secret_Access_Key> -obs_location obs.otc.t-systems.com

   If the following information is displayed, the settings are successful:

   .. code-block::

      setup successfully!

   .. note::

      *<Access_Key_Id>* and *<Secret_Access_Key>*: indicate the AK and SK, respectively. For how to obtain the AK and SK, see "Importing Data" > "Importing Data from OBS in Parallel" > "Creating Access Keys (AK and SK)" in the *Data Warehouse Service (DWS) Developer Guide*. Replace the parameters in the statements with the obtained values.

#. Go back to previous directory and run the gsql environment variables.

   ::

      cd ..
      source gsql_env.sh
      cd bin

#. Import the sample data to the data warehouse.

   Command format:

   ::

      gsql -d <Database name> -h <Public network address of the cluster> -U <Administrator> -p <Data warehouse port number> -f <Path for storing the sample data script> -r

   Sample command:

   ::

      gsql -d gaussdb -h 10.168.0.74 -U dbadmin -p 8000 -f /opt/sample/tpcds_load_data_from_obs.sql -r

   .. note::

      In the preceding command, sample data script **tpcds_load_data_from_obs.sql** is stored in the sample directory (for example, **/opt/sample/**) of the GaussDB(DWS) client.

   After you enter the administrator password and successfully connect to the database in the cluster, the system will automatically create a foreign table to associate the sample data outside the cluster. Then, the system creates a target table for saving the sample data and imports the data to the target table using the foreign table.

   The time required for importing a large dataset depends on the current GaussDB(DWS) cluster specifications. Generally, the import takes about 10 to 20 minutes. If information similar to the following is displayed, the import is successful.

   ::

      Time:1845600.524 ms

5. In the Linux command window, run the following commands to switch to a specific directory and query the sample data:

   ::

      cd /opt/sample/query_sql/
      /bin/bash tpcds100x.sh

6. Enter the cluster's public network IP address, access port, database name, user who accesses the database, and password of the user as prompted.

   -  The default database name is **gaussdb**.
   -  Use the administrator username and password configured during cluster creation as the username and password for accessing the database.

   After the query is complete, a directory for storing the query result, such as **query_output_20170914_072341**, will be generated in the current query directory, for example, **sample/query_sql/**.
