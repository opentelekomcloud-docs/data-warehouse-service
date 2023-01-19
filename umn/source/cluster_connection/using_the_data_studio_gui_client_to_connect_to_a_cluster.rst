:original_name: dws_01_0094.html

.. _dws_01_0094:

Using the Data Studio GUI Client to Connect to a Cluster
========================================================

Data Studio is a SQL client tool running on the Windows operating system. It provides various GUIs for you to manage databases and database objects, as well as edit, run, and debug SQL scripts, and view execution plans. Download the Data Studio software package from the GaussDB(DWS) management console. The package can be used without installation after being decompressed.

Data Studio versions include **Windows x86** (32-bit Windows system) and **Windows x64** (64-bit Windows system).

Preparations Before Connecting to a Cluster
-------------------------------------------

-  An EIP has been bound to the GaussDB(DWS) cluster.

-  You have obtained the database administrator username and password for logging in to the data warehouse cluster.

-  You have obtained the public network address, including the IP address and port number in the data warehouse cluster. For details, see :ref:`Obtaining the Cluster Connection Address <dws_01_0033>`.

-  You have configured the security group to which the data warehouse cluster belongs and added a rule that allows users' IP addresses to access ports using the TCP.

   For details, see "Security > Security Group > Adding a Security Group Rule" in the *Virtual Private Cloud User Guide*.

Connecting to the Cluster Database Using Data Studio
----------------------------------------------------

#. GaussDB(DWS) provides a Windows-based Data Studio client and the tool depends on the JDK. You need to install the JDK on the client host first.

   .. important::

      Only JDK 1.8 is supported.

   In the Windows operating system, you can download the required JDK version from the official website of SDK, and install it by following the installation guidance.

#. Log in to the GaussDB(DWS) management console.

#. Click **Connections**.

#. On the **Download Client and Driver** page, download **Data Studio GUI Client**.

   -  Select **Windows x86** or **Windows x64** based on the OS type and click **Download** to download a Data Studio version that matches the current cluster.

      If clusters of different versions are available, you will download the Data Studio matching the earliest cluster version after clicking **Download**. If there is no cluster, you will download the Data Studio tool of the earliest version after clicking **Download**. GaussDB(DWS) clusters are compatible with earlier versions of Data Studio.

   -  Click **Historical Version** to download the corresponding Data Studio version. You are advised to download Data Studio based on the cluster version.

#. Decompress the downloaded client software package (32-bit or 64-bit) to the installation directory.

#. Open the installation directory and double-click **Data Studio.exe** to start the Data Studio client. See :ref:`Figure 1 <en-us_topic_0000001180440077__febe974d888e24ddd8948572b362f7ef2>`.

   .. _en-us_topic_0000001180440077__febe974d888e24ddd8948572b362f7ef2:

   .. figure:: /_static/images/en-us_image_0000001134400858.png
      :alt: **Figure 1** Starting the client

      **Figure 1** Starting the client

   .. note::

      If your computer blocks the running of the application, you can unlock the **Data Studio.exe** file to start the application.

#. Choose **File** > **New Connection** from the main menu. See :ref:`Figure 2 <en-us_topic_0000001180440077__f5e30eb5f20c9434e971d8b2786b02dbf>`.

   .. _en-us_topic_0000001180440077__f5e30eb5f20c9434e971d8b2786b02dbf:

   .. figure:: /_static/images/en-us_image_0000001134400854.png
      :alt: **Figure 2** New connection

      **Figure 2** New connection


   .. figure:: /_static/images/en-us_image_0000001180320283.png
      :alt: **Figure 3** New connection

      **Figure 3** New connection

#. In the displayed **New Database Connection** window, enter the connection parameters.

   .. table:: **Table 1** Connection parameters

      +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Field                 | Description                                                                                                                                                                          | Example Value         |
      +=======================+======================================================================================================================================================================================+=======================+
      | Database Type         | Select **GaussDB A**                                                                                                                                                                 | GaussDB A             |
      +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Connection Name       | Name of the connection                                                                                                                                                               | dws-demo              |
      +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Host                  | IP address (IPv4) or domain name of the cluster to be connected                                                                                                                      | ``-``                 |
      +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Port Number           | Database port                                                                                                                                                                        | 8000                  |
      +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Database Name         | Database name                                                                                                                                                                        | gaussdb               |
      +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Username              | Username for connecting to the database                                                                                                                                              | ``-``                 |
      +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Password              | Password for logging in to the database to be connected                                                                                                                              | ``-``                 |
      +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Save Password         | Select an option from the drop-down list:                                                                                                                                            | ``-``                 |
      |                       |                                                                                                                                                                                      |                       |
      |                       | -  ****Current Session Only****: The password is saved only in the current session.                                                                                                  |                       |
      |                       | -  ****Do Not Save****: The password is not saved.                                                                                                                                   |                       |
      +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Enable SSL            | If **Enable SSL** is selected, the client can use SSL to encrypt connections. The SSL connection mode is more secure than common modes, so you are advised to enable SSL connection. | ``-``                 |
      +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+

   When **Enable SSL** is selected, download the SSL certificate and decompress it by referring to :ref:`(Optional) Downloading the SSL Certificate <dws_01_0083>`. Click the **SSL** tab and configure the following parameters:

   .. table:: **Table 2** Configuring SSL parameters

      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Field                             | Description                                                                                                                                                                |
      +===================================+============================================================================================================================================================================+
      | Client SSL Certificate            | Select the **sslcert\\client.crt** file in the decompressed SSL certificate directory.                                                                                     |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Client SSL Key                    | Only the PK8 format is supported. Select the **sslcert\\client.key.pk8** file in the directory where the SSL certificate is decompressed.                                  |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Root Certificate                  | When **SSL Mode** is set to **verify-ca**, the root certificate must be configured. Select the **sslcert\\cacert.pem** file in the decompressed SSL certificate directory. |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | SSL Cipher                        | Set the password for the client SSL key in PK8 format.                                                                                                                     |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | SSL Mode                          | GaussDB(DWS) supports the following SSL modes:                                                                                                                             |
      |                                   |                                                                                                                                                                            |
      |                                   | -  require                                                                                                                                                                 |
      |                                   | -  verify-ca                                                                                                                                                               |
      |                                   |                                                                                                                                                                            |
      |                                   | GaussDB(DWS) does not support the **verify-full** mode.                                                                                                                    |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+


   .. figure:: /_static/images/en-us_image_0000001134560646.png
      :alt: **Figure 4** Configuring SSL parameters

      **Figure 4** Configuring SSL parameters

#. Click **OK** to establish the database connection.

   If SSL is enabled, click **Continue** in the displayed **Connection Security Alert** dialog box.

   After the login is successful, the **RECENT LOGIN ACTIVITY** dialog box is displayed, indicating that Data Studio is connected to the database. You can run the SQL statement in the **SQL Terminal** window on the Data Studio page.


   .. figure:: /_static/images/en-us_image_0000001134400862.png
      :alt: **Figure 5** Successful login

      **Figure 5** Successful login

   For details about how to use other functions of Data Studio, press **F1** to view the Data Studio user manual.
