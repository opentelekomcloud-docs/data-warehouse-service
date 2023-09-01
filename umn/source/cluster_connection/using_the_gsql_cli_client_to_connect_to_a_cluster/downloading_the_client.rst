:original_name: dws_01_0031.html

.. _dws_01_0031:

Downloading the Client
======================

GaussDB(DWS) provides client tool packages that match the cluster versions. You can download the desired client tool package on the GaussDB(DWS) management console.

The client tool package contains the following:

-  **Linux database connection tool gsql and the script for testing sample data**

   Linux gsql is a Linux command line client running in Linux. It is used to connect to the database in a data warehouse cluster.

   The script for testing sample data is used when you start an example.

-  **Windows gsql**

   Windows gsql is a command line client running on the Windows OS. It is used to connect to the database in a data warehouse cluster.

   .. note::

      Only 8.1.3.101 and later versions can be downloaded from the console.

-  **GDS tool package**

   Gauss Data Service (GDS) is a data service tool. You can use the GDS tool to import a data file in a common file system to the GaussDB(DWS) database. The GDS tool package must be installed on the server where the data source file is located. The server where the data source file is located is called a data server or GDS server.


Downloading the Client
----------------------

#. Log in to the GaussDB(DWS) console. For details, see :ref:`Accessing the GaussDB(DWS) Management Console <dws_01_0157>`.

#. In the navigation pane on the left, click **Connections**.

#. Select the GaussDB(DWS) client of the corresponding version from the drop-down list of **gsql CLI** **Client**.

   Choose a corresponding client version according to the cluster version and operating system to which the client is to be installed.

   -  The **Redhat x86_64** client can be used on the following OSs:

      -  RHEL 6.4 to RHEL 7.6
      -  CentOS 6.4 to CentOS 7.4
      -  EulerOS 2.3

   -  The **SUSE x86_64** client can be used on the following OSs:

      -  SLES 11.1 to SLES 11.4
      -  SLES 12.0 to SLES 12.3

   -  The **Microsoft Windows** client can be used on the following OSs:

      -  Windows 7 or later
      -  Windows Server 2008 or later

#. Click **Download** to download the gsql tool matching the 8.1.x cluster version. Click **Historical Version** to download the gsql tool corresponding to the cluster version.

   -  You are advised to download the gsql tool that matches the cluster version. That is, use gsql 8.1.x for clusters of 8.1.0 or later, and use gsql 8.2.x for clusters of 8.2.0 or later.
   -  The following table describes the files and folders in the Linux gsql tool package.

      .. table:: **Table 1** Files and folders in the Linux gsql tool package

         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | File or Folder                    | Description                                                                                                                                                                                          |
         +===================================+======================================================================================================================================================================================================+
         | bin                               | This folder contains the executable files of gsql on Linux, including the tools gsql, GDS, gs_dump, gs_dumpall, and gs_restore. For details, see "Server Tool".                                      |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | gds                               | This folder contains the files of the GDS data service tool. The GDS tool is used for parallel data loading and can import the data files stored in a common file system to a GaussDB(DWS) database. |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | lib                               | This folder contains the **lib** library required for executing the gsql client.                                                                                                                     |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | sample                            | This folder contains the following directories and files:                                                                                                                                            |
         |                                   |                                                                                                                                                                                                      |
         |                                   | -  **setup.sh**: script file for configuring the AK/SK before using gsql to import sample data                                                                                                       |
         |                                   | -  **tpcds_load_data_from_obs.sql**: script file for importing the TPC-DS sample data using the gsql client                                                                                          |
         |                                   | -  **query_sql** directory: script file for querying the TPC-DS sample data                                                                                                                          |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | gsql_env.sh                       | Script file for configuring environment variables before running the gsql client.                                                                                                                    |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   -  The following table describes the files and folders in the Windows gsql tool package.

      .. table:: **Table 2** Files and folders in the Windows gsql tool package

         +----------------+---------------------------------------------------------------------------------------------+
         | File or Folder | Description                                                                                 |
         +================+=============================================================================================+
         | x64            | This folder contains the 64-bit Windows gsql execution binary file and the dynamic library. |
         +----------------+---------------------------------------------------------------------------------------------+
         | x86            | This folder contains the 32-bit Windows gsql execution binary file and the dynamic library. |
         +----------------+---------------------------------------------------------------------------------------------+

      .. note::

         -  In the cluster list on the **Clusters** page, click the name of the specified cluster to go to the **Cluster Information** page and view the cluster version.
