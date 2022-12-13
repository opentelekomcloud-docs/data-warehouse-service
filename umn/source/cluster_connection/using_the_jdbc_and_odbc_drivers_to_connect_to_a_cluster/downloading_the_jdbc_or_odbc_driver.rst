:original_name: dws_01_0032.html

.. _dws_01_0032:

Downloading the JDBC or ODBC Driver
===================================

The JDBC or ODBC driver is used to connect to data warehouse clusters. You can download the JDBC or ODBC driver provided by GaussDB(DWS) from the management console or use the open-source JDBC or ODBC driver.

Open-Source JDBC or ODBC Driver
-------------------------------

GaussDB(DWS) also supports open-source JDBC and ODBC drivers: PostgreSQL JDBC 9.3-1103 or later; PostgreSQL ODBC 09.01.0200 or later


Downloading the JDBC or ODBC Driver
-----------------------------------

#. Log in to the GaussDB(DWS) management console.
#. In the navigation pane on the left, click **Connections**.
#. In the **Driver** area, choose a driver that you want to download.

   -  **JDBC Driver**

      Select **DWS JDBC Driver** and click **Download** to download the JDBC driver matching the current cluster version. If clusters of different versions are available, you will download the JDBC driver matching the earliest cluster version after clicking **Download**. If there is no cluster, you will download the JDBC driver of the earliest version after clicking **Download**. GaussDB(DWS) clusters are compatible with earlier versions of JDBC drivers.

      Click **Historical Version** to download the corresponding JDBC driver version. You are advised to download the JDBC driver based on the cluster version.

      The JDBC driver can be used on all platforms and depends on JDK 1.6 or later.

   -  **ODBC Driver**

      Select a corresponding version and click **Download** to download the ODBC driver matching the current cluster version. If clusters of different versions are available, you will download the ODBC driver matching the earliest cluster version after clicking **Download**. If there is no cluster, you will download the ODBC driver of the earliest version after clicking **Download**. GaussDB(DWS) clusters are compatible with earlier versions of ODBC drivers.

      Click **Historical Version** to download the corresponding ODBC driver version. You are advised to download the ODBC driver based on the cluster version.

      The ODBC driver is applicable to the following operating systems only:

      -  Windows Server 2008 or Windows 7 or later
      -  x86 servers: RHEL 6.4 to RHEL 7.6
      -  x86 servers: CentOS 6.4 to CentOS 7.4
      -  x86 servers: SUSE 11.1 to SUSE 11.4; SUSE 12.0 to SUSE 12.3

      .. note::

         Windows drivers can only be 32-bit and can be used in 32-bit or 64-bit operating systems. However, the applications must be 32-bit.
