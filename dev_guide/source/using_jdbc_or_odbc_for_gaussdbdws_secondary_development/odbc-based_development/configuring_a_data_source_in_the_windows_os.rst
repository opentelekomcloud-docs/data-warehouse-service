:original_name: dws_04_0120.html

.. _dws_04_0120:

Configuring a Data Source in the Windows OS
===========================================

Configure the ODBC data source using the ODBC data source manager preinstalled in the Windows OS.

Procedure
---------

#. Replace the GaussDB(DWS) client driver.

   Decompress **GaussDB-8.2.1-Windows-Odbc.tar.gz** and install **psqlodbc.msi** (for 32-bit OS) or **psqlodbc_x64.msi** (for 64-bit OS).

#. Open Driver Manager.

   Use the Driver Manager suitable for your OS to configure the data source. (Assume the Windows system drive is drive C.)

   -  If you develop 32-bit programs in the 64-bit Windows OS, open the 32-bit Driver Manager at **C:\\Windows\\SysWOW64\\odbcad32.exe** after you install the 32-bit driver.

      Do not open Driver Manager by choosing **Control Panel**, clicking **Administrative Tools**, and clicking **Data Sources (ODBC)**.

      .. note::

         WoW64 is the acronym for "Windows 32-bit on Windows 64-bit". **C:\\Windows\\SysWOW64\\** stores the 32-bit environment on a 64-bit system.

   -  If you develop 64-bit programs in the 64-bit Windows OS, open the 64-bit Driver Manager at **C:\\Windows\\System32\\odbcad32.exe** after you install the 64-bit driver.

      Do not open **Driver Manager** by choosing **Control Panel**, clicking **Administrative Tools**, and clicking **Data Sources (ODBC)**.

      .. note::

         **C:\\Windows\\System32\\** stores the environment consistent with the current OS. For technical details, see Windows technical documents.

   -  In a 32-bit Windows OS, open **C:\\Windows\\System32\\odbcad32.exe**.

      In the Windows OS, click **Computer**, and choose **Control Panel**. Click **Administrative Tools** and click **Data Sources (ODBC)**.

#. Configure the data source.

   On the **User DSN** tab, click **Add**, and choose **PostgreSQL Unicode** for setup. (An identifier will be displayed for the 64-bit OS.)

   |image1|

   .. important::

      The entered username and password will be recorded in the Windows registry and you do not need to enter them again when connecting to the database next time. For security purposes, you are advised to delete sensitive information before clicking **Save** and enter the required username and password again when using ODBC APIs to connect to the database.

#. Enable the SSL mode.

   To use SSL certificates for connection, decompress the certificate package contained in the GaussDB(DWS) installation package, and double-click the **sslcert_env.bat** file to deploy certificates in the default location.

   .. important::

      The **sslcert_env.bat** file ensures the purity of the certificate environment. When the **%APPDATA%\\postgresql** directory exists, a message will be prompted asking you whether you want to remove related directories. If you want to remove related directories, back up files in the directory.

   Alternatively, you can copy the **client.crt**, **client.key**, **client.key.cipher**, and **client.key.rand** files in the certificate file folder to the manually created **%APPDATA%\\postgresql** directory. Change **client** in the file names to **postgres**, for example, change **client.key** to **postgres.key**. Copy the **cacert.pem** file to the **%APPDATA%\\postgresql** directory and change its name to **root.crt**.

   Change the value of **SSL Mode** in step 2 to **verify-ca**.

   .. _en-us_topic_0000001510522705__tbff3516afc1b4dd59cf87017f2af1d56:

   .. table:: **Table 1** sslmode options

      +-----------------------+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | sslmode               | Whether SSL Encryption Is Enabled | Description                                                                                                                                                                                                  |
      +=======================+===================================+==============================================================================================================================================================================================================+
      | disable               | No                                | The SSL secure connection is not used.                                                                                                                                                                       |
      +-----------------------+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | allow                 | Probably                          | The SSL secure encrypted connection is used if required by the database server, but does not check the authenticity of the server.                                                                           |
      +-----------------------+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | prefer                | Probably                          | The SSL secure encrypted connection is used as a preferred mode if supported by the database, but does not check the authenticity of the server.                                                             |
      +-----------------------+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | require               | Yes                               | The SSL secure connection must be used, but it only encrypts data and does not check the authenticity of the server.                                                                                         |
      +-----------------------+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | verify-ca             | Yes                               | The SSL secure connection must be used, and it checks whether the database has certificates issued by a trusted CA.                                                                                          |
      +-----------------------+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | verify-full           | Yes                               | The SSL secure connection must be used. In addition to the check scope specified by **verify-ca**, it checks whether the name of the host where the database resides is the same as that on the certificate. |
      |                       |                                   |                                                                                                                                                                                                              |
      |                       |                                   | .. note::                                                                                                                                                                                                    |
      |                       |                                   |                                                                                                                                                                                                              |
      |                       |                                   |    This mode cannot be used.                                                                                                                                                                                 |
      +-----------------------+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Add the IP address segment of the host where the client is located to the security group rules of GaussDB(DWS) to ensure that the host can communicate with GaussDB(DWS).

Testing Data Source Configuration
---------------------------------

Click **Test**.

-  If the following information is displayed, the configuration is correct and the connection succeeds.

   |image2|

-  If error information is displayed, the configuration is incorrect. Check the configuration.

Troubleshooting
---------------

-  Server common name "xxxx" does not match host name "xxxxx"

   This problem occurs because when **verify-full** is used for SSL encryption, the driver checks whether the host name in certificates is the same as the actual one. To solve this problem, use **verify-ca** to stop checking host names, or generate a set of CA certificates containing the actual host names.

-  connect to server failed: no such file or directory

   Possible causes:

   -  An incorrect or unreachable database IP address or port was configured.

      Check the **Servername** and **Port** configuration items in data sources.

   -  Server monitoring is improper.

      If **Servername** and **Port** are correctly configured, ensure the proper network adapter and port are monitored based on database server configurations in the procedure in this section.

   -  Firewall and network gatekeeper settings are improper.

      Check firewall settings, ensuring that the database communication port is trusted.

      Check to ensure network gatekeeper settings are proper (if any).

-  In the specified DSN, the system structures of the drive do not match those of the application.

   Possible cause: The bit versions of the drive and program are different.

   **C:\\Windows\\SysWOW64\\odbcad32.exe** is a 32-bit ODBC Drive Manager.

   **C:\\Windows\\System32\\odbcad32.exe** is a 64-bit ODBC Drive Manager.

-  The password-stored method is not supported.

   Possible causes:

   **sslmode** is not configured for the data source. Set this configuration item to **allow** or a higher level to enable SSL connections. For details about **sslmode**, see :ref:`Table 1 <en-us_topic_0000001510522705__tbff3516afc1b4dd59cf87017f2af1d56>`.

-  authentication method 10 not supported.

   If this error occurs on an open source client, the cause may be:

   The database stores only the SHA-256 hash of the password, but the open source client supports only MD5 hashes.

   .. note::

      -  The database stores the hashes of user passwords instead of actual passwords.
      -  In versions earlier than V100R002C80SPC300, the database stores only SHA-256 hashes and no MD5 hashes. Therefore, MD5 cannot be used for user password authentication.
      -  In V100R002C80SPC300 and later, if a password is updated or a user is created, both types of hashes will be stored, compatible with open-source authentication protocols.
      -  An MD5 hash can only be generated using the original password, but the password cannot be obtained by reversing its SHA-256 hash. If your database is upgraded from a version earlier than V100R002C80SPC300, passwords in the old version will only have SHA-256 hashes and not support MD5 authentication.

   To solve this problem, perform the following operations:

   #. Set **password_encryption_type** to **1**. For details, see "Modifying Database Parameters" in *User Guide*.
   #. Create a database user for connection or reset the password of the existing database user.

      -  If you use an administrator account, reset the password. For details, see "Password Reset" in *User Guide*.
      -  If you are a common user, use another client tool (such as Data Studio) to connect to the database and run the **ALTER USER** statement to change your password.

   #. Connect to the database.

-  unsupported frontend protocol 3.51: server supports 1.0 to 3.0

   The database version is too early or the database is an open-source database. Use the driver of the required version to connect to the database.

-  FATAL: GSS authentication method is not allowed because XXXX user password is not disabled.

   In some cases, the error is: GSSAPI authentication not supported.

   In **pg_hba.conf** of the target CN, the authentication mode is set to **gss** for authenticating the IP address of the current client. However, this authentication algorithm cannot authenticate clients. Change the authentication algorithm to **sha256** and try again.

   Note that cross-node connection to the database in the cluster is not supported. If the error is caused by cross-node connection to the CN in the cluster, connect the service program to the database from a node outside the cluster and try again.

.. |image1| image:: /_static/images/en-us_image_0000001460882968.jpg
.. |image2| image:: /_static/images/en-us_image_0000001510163345.jpg
