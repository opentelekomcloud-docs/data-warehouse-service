:original_name: dws_01_0031.html

.. _dws_01_0031:

Downloading the Client
======================

DWS provides client tool packages that match the cluster versions. You can download the desired client tool package on the DWS console.

The client tool package contains the following items. The package can be downloaded on the console only for 8.1.3.101 and later versions of clusters.

-  **Linux database connection tool gsql and the script for testing sample data**

   Linux gsql is a Linux command line client running in Linux. It is used to connect to the database in a DWS cluster.

   The script for testing sample data is used to execute the introductory example.

-  **Windows gsql**

   Windows gsql is a command line client running on the Windows OS. It is used to connect to the database in a DWS cluster.

-  **GDS tool package**

   Gauss Data Service (GDS) is a data service tool. You can use the GDS tool to import a data file in a common file system to the DWS database. The GDS tool package must be installed on the server where the data source file is located. The server where the data source file is located is called a data server or GDS server.


Downloading the Client
----------------------

#. Log in to the DWS console.

#. In the navigation pane on the left, choose **Dedicated Clusters** > **Management** > **Client Connections**.

#. Select the DWS client of the target version from the drop-down list of **CLI** **Client**.

   Choose a corresponding client version according to the cluster version and operating system to which the client is to be installed. The CPU architecture of the client must be the same as that of the cluster. If the cluster uses x86 servers, select an x86 client.

#. Click **Download** to download the gsql CLI client matching the cluster version. (To view the cluster version, click a cluster name in the cluster list and then click the **Cluster Information** tab.)

   -  You are advised to download the gsql tool that matches the cluster version. That is, use gsql 8.1.\ *x* for clusters of 8.1.x (8.1.0 or later), use gsql 8.2.\ *x* for clusters of 8.2.\ *x* (8.2.0 or later), and use gsql 9.1.\ *x* for clusters of 9.1.x (9.1.0 or later).
   -  The following table describes the files and folders in the Linux gsql tool package.

      .. table:: **Table 1** Files and folders in the Linux gsql tool package

         +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | File or Folder                    | Description                                                                                                                                                                                 |
         +===================================+=============================================================================================================================================================================================+
         | bin                               | This folder holds the Linux executable files for gsql, which include tools gsql, GDS, gs_dump, gs_dumpall, and gs_restore. For details, see "Server Tool".                                  |
         +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | gds                               | This folder contains the files of the GDS data service tool. The GDS tool is used for parallel data loading and can import the data files stored in a common file system to a DWS database. |
         +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | lib                               | This folder contains the **lib** library required for executing the gsql client.                                                                                                            |
         +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | sample                            | This folder contains the following directories and files:                                                                                                                                   |
         |                                   |                                                                                                                                                                                             |
         |                                   | -  **setup.sh**: script file for configuring the AK/SK before using gsql to import sample data                                                                                              |
         |                                   | -  **tpcds_load_data_from_obs.sql**: script file for importing the TPC-DS sample data using the gsql client                                                                                 |
         |                                   | -  **query_sql** directory: script file for querying the TPC-DS sample data                                                                                                                 |
         +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | gsql_env.sh                       | Script file for configuring environment variables before running the gsql client.                                                                                                           |
         +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   -  The following table describes the files and folders in the Windows gsql tool package.

      .. table:: **Table 2** Files and folders in the Windows gsql tool package

         +----------------+---------------------------------------------------------------------------------------------+
         | File or Folder | Description                                                                                 |
         +================+=============================================================================================+
         | x64            | This folder contains the 64-bit Windows gsql execution binary file and the dynamic library. |
         +----------------+---------------------------------------------------------------------------------------------+
         | x86            | This folder contains the 32-bit Windows gsql execution binary file and the dynamic library. |
         +----------------+---------------------------------------------------------------------------------------------+
