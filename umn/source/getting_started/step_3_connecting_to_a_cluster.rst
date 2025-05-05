:original_name: dws_01_0107.html

.. _dws_01_0107:

Step 3: Connecting to a Cluster
===============================

Scenario
--------

This section describes how to use a database client to connect to the database in a GaussDB(DWS) cluster. In the following example, the Data Studio client tool is used for connection through the public network address. You can also use other SQL clients to connect to the cluster. For more connection methods, see :ref:`Overview <dws_01_0137>`.

#. Obtain the name, username, and password of the database to be connected.

   If you use the client to connect to the cluster for the first time, use the administrator username and password set in :ref:`Step 2: Creating a Cluster <dws_01_0013>` to connect to the default database **gaussdb**.

#. :ref:`Obtaining the Public Network Address of the Cluster <en-us_topic_0000002203426657__section2301113635>`: Connect to the cluster database using the public network address.

#. :ref:`Connecting to the Cluster Database Using Data Studio <en-us_topic_0000002203426657__section99729517811>`: Download and configure the Data Studio client and connect to the cluster database.

.. _en-us_topic_0000002203426657__section2301113635:

Obtaining the Public Network Address of the Cluster
---------------------------------------------------

#. Log in to the GaussDB(DWS) console.

#. Choose **Dedicated Clusters** > **Clusters** in the navigation pane.

#. In the cluster list, select a created cluster (for example, **dws-demo**) and click |image1| next to the cluster name to obtain the public network address.

   The public network address will be used in :ref:`Connecting to the Cluster Database Using Data Studio <en-us_topic_0000002203426657__section99729517811>`.

.. _en-us_topic_0000002203426657__section99729517811:

Connecting to the Cluster Database Using Data Studio
----------------------------------------------------

#. GaussDB(DWS) provides a Windows-based Data Studio GUI client. The tool depends on JDK, so you must install Java 1.8.0_141 or later on the client host.

   In the Windows operating system, you can download the required JDK version from the official website of JDK, and install it by following the installation guide.

#. Log in to the GaussDB(DWS) console.

#. Click **Client Connections**.

#. On the **Download Client and Driver** page, download **Data Studio GUI Client**.

   -  Select **Windows x86** or **Windows x64** based on the operating system type and click **Download** to download the Data Studio tool matching the current cluster version.

      If clusters of different versions are available, you will download the Data Studio tool matching the earliest cluster version after clicking **Download**. If there is no cluster, you will download the Data Studio tool of the earliest version after clicking **Download**. GaussDB(DWS) clusters are compatible with earlier versions of Data Studio.

   -  Click **Historical Version** to download the corresponding Data Studio version. You are advised to download the Data Studio based on the cluster version.


   .. figure:: /_static/images/en-us_image_0000002229258184.png
      :alt: **Figure 1** Downloading clients

      **Figure 1** Downloading clients

#. Decompress the downloaded client software package (32-bit or 64-bit) to the installation directory.

#. Open the installation directory and double-click **Data Studio.exe** to start the Data Studio client. See :ref:`Figure 2 <en-us_topic_0000002203426657__fig6324139192412>`.

   .. _en-us_topic_0000002203426657__fig6324139192412:

   .. figure:: /_static/images/en-us_image_0000002203312589.png
      :alt: **Figure 2** Starting the client

      **Figure 2** Starting the client

#. Choose **File** > **New Connection** from the main menu. See :ref:`Figure 3 <en-us_topic_0000002203426657__fig14311312192811>`.

   .. _en-us_topic_0000002203426657__fig14311312192811:

   .. figure:: /_static/images/en-us_image_0000002203427037.png
      :alt: **Figure 3** Creating a connection

      **Figure 3** Creating a connection

#. In the displayed **New Database Connection** window, enter the connection parameters.

   .. table:: **Table 1** Connection parameters

      +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Parameter             | Description                                                                                                                                                               | Example               |
      +=======================+===========================================================================================================================================================================+=======================+
      | Database Type         | Select **GaussDB A**.                                                                                                                                                     | GaussDB A             |
      +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Connection Name       | Name of a connection                                                                                                                                                      | dws-demo              |
      +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Host                  | IP address (IPv4) or domain name of the cluster to be connected                                                                                                           | ``-``                 |
      +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Host Port             | Database port                                                                                                                                                             | 8000                  |
      +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Database Name         | Database name                                                                                                                                                             | gaussdb               |
      +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | User Name             | Username for connecting to the database                                                                                                                                   | ``-``                 |
      +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Password              | Password for logging in to the database to be connected                                                                                                                   | ``-``                 |
      +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Save Password         | Select an option from the drop-down list:                                                                                                                                 | ``-``                 |
      |                       |                                                                                                                                                                           |                       |
      |                       | -  ****Current Session Only****: The password is saved only in the current session.                                                                                       |                       |
      |                       | -  ****Do Not Save****: The password is not saved.                                                                                                                        |                       |
      +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Enable SSL            | If **Enable SSL** is selected, the client can use SSL to encrypt connections. The SSL mode is more secure than common modes, so you are advised to enable SSL connection. | ``-``                 |
      +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+

   When Enable SSL is selected, download the SSL certificate and decompress it by referring to :ref:`Downloading SSL Certificate <en-us_topic_0000002167905932__li13478842115911>`. Click the **SSL** tab and configure the following parameters:

   .. table:: **Table 2** Configuring SSL parameters

      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                                                                                |
      +===================================+============================================================================================================================================================================+
      | Client SSL Certificate            | Select the **sslcert\\client.crt** file in the decompressed SSL certificate directory.                                                                                     |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Client SSL Key                    | Only the PK8 format is supported. Select the **sslcert\\client.key.pk8** file in the directory where the SSL certificate is decompressed.                                  |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Root Certificate                  | When **SSL Mode** is set to **verify-ca**, the root certificate must be configured. Select the **sslcert\\cacert.pem** file in the decompressed SSL certificate directory. |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | SSL Password                      | Set the password for the client SSL key in PK8 format.                                                                                                                     |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | SSL Mode                          | Supported SSL modes include:                                                                                                                                               |
      |                                   |                                                                                                                                                                            |
      |                                   | -  require                                                                                                                                                                 |
      |                                   | -  verify-ca                                                                                                                                                               |
      |                                   |                                                                                                                                                                            |
      |                                   | GaussDB(DWS) does not support the **verify-full** mode.                                                                                                                    |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+


   .. figure:: /_static/images/en-us_image_0000002167906344.png
      :alt: **Figure 4** Configuring SSL parameters

      **Figure 4** Configuring SSL parameters

#. Click **OK** to establish the database connection.

   If SSL is enabled, click **Continue** in the displayed **Connection Security Alert** dialog box.

   After the login is successful, the **RECENT LOGIN ACTIVITY** dialog box is displayed, indicating that Data Studio is connected to the database. You can run the SQL statement in the **SQL Terminal** window on the Data Studio page.


   .. figure:: /_static/images/en-us_image_0000002168066044.png
      :alt: **Figure 5** Successful login

      **Figure 5** Successful login

   For details about how to use other functions of Data Studio, press **F1** to view the Data Studio user manual.

.. |image1| image:: /_static/images/en-us_image_0000002203312593.png
