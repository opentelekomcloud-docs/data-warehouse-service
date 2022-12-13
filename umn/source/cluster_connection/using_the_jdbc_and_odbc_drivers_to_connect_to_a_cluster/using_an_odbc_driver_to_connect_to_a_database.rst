:original_name: dws_01_0086.html

.. _dws_01_0086:

Using an ODBC Driver to Connect to a Database
=============================================

GaussDB(DWS) allows you to use an ODBC driver to connect to the database through an ECS on the cloud platform or over the Internet.

For details about how to use the ODBC API, see the official document.

Prerequisites
-------------

-  You have downloaded ODBC driver packages **dws_odbc_driver_for_linux.zip** (for Linux) and **dws_odbc_driver_for_windows.zip** (for Windows). For details, see :ref:`Downloading the JDBC or ODBC Driver <dws_01_0032>`.

   GaussDB(DWS) also supports open-source ODBC driver: PostgreSQL ODBC 09.01.0200 or later.

-  You have downloaded the open-source unixODBC code file 2.3.0 from https://sourceforge.net/projects/unixodbc/files/unixODBC/2.3.0/unixODBC-2.3.0.tar.gz/download.

-  You have downloaded the SSL certificate file. For details, see :ref:`(Optional) Downloading the SSL Certificate <dws_01_0083>`.

Using an ODBC Driver to Connect to a Database (Linux)
-----------------------------------------------------

#. Upload the ODBC package and code file to the Linux environment and decompress them to the specified directory.

#. Log in to the Linux environment as user **root**.

#. Prepare **unixODBC**.

   a. Decompress the **unixODBC** code file.

      .. code-block::

         tar -xvf unixODBC-2.3.0.tar.gz

   b. Modify the configuration.

      .. code-block::

         cd unixODBC-2.3.0
         vi configure

      Change the value of **LIB_VERSION** to the following. Save the change and exit.

      .. code-block::

         LIB_VERSION="1:0:0"

   c. Compile the code file and install the driver.

      .. code-block::

         ./configure --enable-gui=no
         make
         make install

#. Replace the driver file.

   a. Decompress **dws_odbc_driver_for_linux.zip**.

      .. code-block::

         unzip dws_odbc_driver_for_linux.zip

   b. Copy all files in the **lib** directory to **/usr/local/lib**. If there are files with the same name, overwrite them.

   c. Copy **psqlodbcw.la** and **psqlodbcw.so** in the **odbc/lib** directory to **/usr/local/lib**.

#. Run the following command to modify the configuration of the driver file:

   .. code-block::

      vi /usr/local/etc/odbcinst.ini

   Copy the following content to the file:

   .. code-block::

      [DWS]
      Driver64=/usr/local/lib/psqlodbcw.so

   The parameters are as follows:

   -  **[DWS]**: indicates the driver name. You can customize the name.
   -  **Driver64** or **Driver**: indicates the path where the dynamic library of the driver resides. For a 64-bit operating system, search for **Driver64** first. If **Driver64** is not configured, search for **Driver**.

#. Run the following command to modify the data source file:

   .. code-block::

      vi /usr/local/etc/odbc.ini

   Copy the following content to the configuration file, save the modification, and exit.

   .. code-block::

      [DWSODBC]
      Driver=DWS
      Servername=10.10.0.13
      Database=gaussdb
      Username=dbadmin
      Password=password
      Port=8000
      Sslmode=allow

   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | Parameter             | Description                                                                                                                                                                                                                                                         | Example Value         |
   +=======================+=====================================================================================================================================================================================================================================================================+=======================+
   | [DSN]                 | Data source name.                                                                                                                                                                                                                                                   | [DWSODBC]             |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | Driver                | Driver name, corresponding to **DriverName** in **odbcinst.ini**.                                                                                                                                                                                                   | Driver=DWS            |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | Servername            | Server IP address.                                                                                                                                                                                                                                                  | Servername=10.10.0.13 |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | Database              | Name of the database to be connected to.                                                                                                                                                                                                                            | Database=gaussdb      |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | Username              | Database username.                                                                                                                                                                                                                                                  | Username=dbadmin      |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | Password              | Database user password.                                                                                                                                                                                                                                             | Password=\ *password* |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | Port                  | Port number of the server.                                                                                                                                                                                                                                          | Port=8000             |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | Sslmode               | SSL certification mode. This parameter is enabled for the cluster by default.                                                                                                                                                                                       | Sslmode=allow         |
   |                       |                                                                                                                                                                                                                                                                     |                       |
   |                       | Values and meanings:                                                                                                                                                                                                                                                |                       |
   |                       |                                                                                                                                                                                                                                                                     |                       |
   |                       | -  **disable**: only tries to establish a non-SSL connection.                                                                                                                                                                                                       |                       |
   |                       | -  **allow**: tries establishing a non-SSL connection first, and then an SSL connection if the attempt fails.                                                                                                                                                       |                       |
   |                       | -  **prefer**: tries establishing an SSL connection first, and then a non-SSL connection if the attempt fails.                                                                                                                                                      |                       |
   |                       | -  **require**: only tries establishing an SSL connection. If there is a CA file, perform the verification according to the scenario in which the parameter is set to **verify-ca**.                                                                                |                       |
   |                       | -  **verify-ca**: tries establishing an SSL connection and checks whether the server certificate is issued by a trusted CA.                                                                                                                                         |                       |
   |                       | -  **verify-full**: not supported by GaussDB(DWS)                                                                                                                                                                                                                   |                       |
   |                       |                                                                                                                                                                                                                                                                     |                       |
   |                       | .. note::                                                                                                                                                                                                                                                           |                       |
   |                       |                                                                                                                                                                                                                                                                     |                       |
   |                       |    The SSL mode delivers higher security than the common mode. By default, the SSL function is enabled in a cluster to allow SSL or non-SSL connections from the client. You are advised to use the SSL mode when using ODBC to connect to a GaussDB (DWS) cluster. |                       |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+

   .. note::

      You can view the values of **Servername** and **Port** on the GaussDB(DWS) management console. Log in to the GaussDB(DWS) management console and click **Connection Management**. In the **Data Warehouse Connection String** area, select the target cluster and obtain **Private Network Address** or **Public Network Address**. For details, see :ref:`Obtaining the Cluster Connection Address <dws_01_0033>`.

#. Configure environment variables.

   .. code-block::

      vi ~/.bashrc

   Add the following information to the configuration file:

   .. code-block::

      export LD_LIBRARY_PATH=/usr/local/lib/:$LD_LIBRARY_PATH
      export ODBCSYSINI=/usr/local/etc
      export ODBCINI=/usr/local/etc/odbc.ini

#. Import environment variables.

   .. code-block::

      source ~/.bashrc

#. Run the following commands to connect to the database:

   .. code-block::

      /usr/local/bin/isql -v DWSODBC

   If the following information is displayed, the connection is successful:

   ::

      +---------------------------------------+
      | Connected!                            |
      |                                       |
      | sql-statement                         |
      | help [tablename]                      |
      | quit                                  |
      |                                       |
      +---------------------------------------+
      SQL>

Using an ODBC Driver to Connect to a Database (Windows)
-------------------------------------------------------

#. Decompress ODBC driver package **dws_odbc_driver_for_windows.zip** (for Windows) and install **psqlodbc.msi**.

#. Decompress the SSL certificate package to obtain the certificate file.

   You can choose to automatically or manually deploy the certificate based on your needs.

   Automatic deployment:

   Double-click the **sslcert_env.bat** file. The certificate is automatically deployed to a default location.

   .. note::

      The **sslcert_env.bat** file ensures the purity of the certificate environment. When the **%APPDATA%\\postgresql** directory exists, a message will be prompted asking you whether you want to remove related directories. If you want to remove related directories, back up files in the directory.

   Manual deployment:

   a. Create a new folder named **postgresql** in the **%APPDATA%\\** directory.
   b. Copy files **client.crt**, **client.key**, **client.key.cipher**, and **client.key.rand** to the **%APPDATA%\\postgresql** directory and change **client** in the file name to **postgres**. For example, change the name of **client.key** to **postgres.key**.
   c. Copy **cacert.pem** to **%APPDATA%\\postgresql** and change the name of **cacert.pem** to **root.crt**.

#. Open Driver Manager.

   Currently, because GaussDB(DWS) only provides a 32-bit ODBC driver, it only supports 32-bit application development. Use the 32-bit Driver Manager when you configure the data source. (Assume the Windows system drive is drive C. If another disk drive is used, modify the path accordingly.)

   -  In a 64-bit Windows operating system, open **C:\\Windows\\SysWOW64\\odbcad32.exe**.

      Do not choose **Control Panel** > **System and Security** > **Administrative Tools** > **Data Sources (ODBC)** directly.

      .. note::

         WOW64 is the acronym for Windows 32-bit on Windows 64-bit. **C:\\Windows\\SysWOW64\\** stores the 32-bit environment on a 64-bit system. **C:\\Windows\\System32\\** stores the environment consistent with the current operating system. For technical details, see the Windows technical documents.

   -  In a 32-bit Windows operating system, open **C:\\Windows\\System32\\odbcad32.exe**.

      You can also open Driver Manager by choosing **Control Panel** > **System and Security** > **Administrative Tools** > **Data Sources (ODBC)**.

#. Configure a data source to be connected to.

   a. On the **User DSN** tab, click **Add** and choose **PostgreSQL Unicode** for setup.


      .. figure:: /_static/images/en-us_image_0000001180320325.png
         :alt: **Figure 1** Configuring a data source to be connected to

         **Figure 1** Configuring a data source to be connected to

      You can view the values of **Server** and **Port** on the GaussDB(DWS) management console. Log in to the GaussDB(DWS) management console and click **Connections**. In the **Data Warehouse Connection String** area, select the target cluster and obtain **Private Network Address** or **Public Network Address**. For details, see :ref:`Obtaining the Cluster Connection Address <dws_01_0033>`.

   b. Click **Test** to verify that the connection is correct. If **Connection successful** is displayed, the connection is correct.

#. Compile an ODBC sample program to connect to the data source.

   The ODBC API does not provide the database connection retry capability. You need to implement the connection retry processing in the service code.

   The sample code is as follows:

   ::

      // This example shows how to obtain GaussDB(DWS) data through the ODBC driver.
      // DBtest.c (compile with: libodbc.so)
      #include <stdlib.h>
      #include <stdio.h>
      #include <sqlext.h>
      #ifdef WIN32
      #include <windows.h>
      #endif
      SQLHENV       V_OD_Env;        // Handle ODBC environment
      SQLHSTMT      V_OD_hstmt;      // Handle statement
      SQLHDBC       V_OD_hdbc;       // Handle connection
      char          typename[100];
      SQLINTEGER    value = 100;
      SQLINTEGER    V_OD_erg,V_OD_buffer,V_OD_err,V_OD_id;
      int main(int argc,char *argv[])
      {
            // 1. Apply for an environment handle.
            V_OD_erg = SQLAllocHandle(SQL_HANDLE_ENV,SQL_NULL_HANDLE,&V_OD_Env);
            if ((V_OD_erg != SQL_SUCCESS) && (V_OD_erg != SQL_SUCCESS_WITH_INFO))
            {
                 printf("Error AllocHandle\n");
                 exit(0);
            }
            // 2. Set environment attributes (version information).
            SQLSetEnvAttr(V_OD_Env, SQL_ATTR_ODBC_VERSION, (void*)SQL_OV_ODBC3, 0);
            // 3. Apply for a connection handle.
            V_OD_erg = SQLAllocHandle(SQL_HANDLE_DBC, V_OD_Env, &V_OD_hdbc);
            if ((V_OD_erg != SQL_SUCCESS) && (V_OD_erg != SQL_SUCCESS_WITH_INFO))
            {
                 SQLFreeHandle(SQL_HANDLE_ENV, V_OD_Env);
                 exit(0);
            }
            // 4. Set connection attributes.
            SQLSetConnectAttr(V_OD_hdbc, SQL_ATTR_AUTOCOMMIT, SQL_AUTOCOMMIT_ON, 0);
            // 5. Connect to a data source. You do not need to enter the username and password if you have configured them in the odbc.ini file. If you have not configured them, specify the name and password of the user who wants to connect to the database in the SQLConnect function.
            V_OD_erg = SQLConnect(V_OD_hdbc, (SQLCHAR*) "gaussdb", SQL_NTS,
                                 (SQLCHAR*) "", SQL_NTS,  (SQLCHAR*) "", SQL_NTS);
            if ((V_OD_erg != SQL_SUCCESS) && (V_OD_erg != SQL_SUCCESS_WITH_INFO))
            {
                printf("Error SQLConnect %d\n",V_OD_erg);
                SQLFreeHandle(SQL_HANDLE_ENV, V_OD_Env);
                exit(0);
            }
            printf("Connected !\n");
            // 6. Set statement attributes.
            SQLSetStmtAttr(V_OD_hstmt,SQL_ATTR_QUERY_TIMEOUT,(SQLPOINTER *)3,0);
            // 7. Apply for a statement handle.
            SQLAllocHandle(SQL_HANDLE_STMT, V_OD_hdbc, &V_OD_hstmt);
            // 8. Executes an SQL statement directly.
            SQLExecDirect(V_OD_hstmt,"drop table IF EXISTS testtable",SQL_NTS);
            SQLExecDirect(V_OD_hstmt,"create table testtable(id int)",SQL_NTS);
            SQLExecDirect(V_OD_hstmt,"insert into testtable values(25)",SQL_NTS);
            // 9. Prepare for execution.
            SQLPrepare(V_OD_hstmt,"insert into testtable values(?)",SQL_NTS);
            // 10. Bind parameters.
            SQLBindParameter(V_OD_hstmt,1,SQL_PARAM_INPUT,SQL_C_SLONG,SQL_INTEGER,0,0,
                             &value,0,NULL);
            // 11. Execute the ready statement.
            SQLExecute(V_OD_hstmt);
            SQLExecDirect(V_OD_hstmt,"select id from testtable",SQL_NTS);
            // 12. Obtain the attributes of a certain column in the result set.
            SQLColAttribute(V_OD_hstmt,1,SQL_DESC_TYPE,typename,100,NULL,NULL);
            printf("SQLColAtrribute %s\n",typename);
            // 13. Bind the result set.
            SQLBindCol(V_OD_hstmt,1,SQL_C_SLONG, (SQLPOINTER)&V_OD_buffer,150,
                      (SQLLEN *)&V_OD_err);
            // 14. Collect data using SQLFetch.
            V_OD_erg=SQLFetch(V_OD_hstmt);
            // 15. Obtain and return data using SQLGetData.
            while(V_OD_erg != SQL_NO_DATA)
            {
                SQLGetData(V_OD_hstmt,1,SQL_C_SLONG,(SQLPOINTER)&V_OD_id,0,NULL);
                printf("SQLGetData ----ID = %d\n",V_OD_id);
                V_OD_erg=SQLFetch(V_OD_hstmt);
            };
            printf("Done !\n");pgadmin
            // 16. Disconnect from the data source and release handles.
            SQLFreeHandle(SQL_HANDLE_STMT,V_OD_hstmt);
            SQLDisconnect(V_OD_hdbc);
            SQLFreeHandle(SQL_HANDLE_DBC,V_OD_hdbc);
            SQLFreeHandle(SQL_HANDLE_ENV, V_OD_Env);
            return(0);
       }
